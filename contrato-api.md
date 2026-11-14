# Contrato da API — MVP

Contrato implementado pelo [backend no PR #3](https://github.com/SmartMall-AI/be-smartmall-ai/pull/3). O alinhamento das telas e a validação com usuários ainda precisam ser concluídos. O [OpenAPI exportado](https://github.com/SmartMall-AI/be-smartmall-ai/blob/main/openapi.json) contém os schemas completos gerados pelo FastAPI.

Base `/api/v1`. JSON em `snake_case`, identificadores UUID em string e datas ISO 8601 em UTC. A interface traduz enums para português. Listas retornam arrays sem paginação no MVP. As rotas ainda não têm autenticação: usar apenas ambiente local e dados fictícios.

## Ocorrências

| Método | Rota | Comportamento |
| --- | --- | --- |
| GET | `/occurrences` | Lista da mais recente para a mais antiga; filtros combináveis `status`, `priority`, `category`, `equipment_id`, `location` |
| POST | `/occurrences` | Persiste ocorrência; retorna 201 e cabeçalho `Location` |
| GET | `/occurrences/{id}` | Detalhe, análise e tarefas vinculadas |
| PATCH | `/occurrences/{id}` | Atualiza campos enviados; retorna detalhe |
| POST | `/occurrences/{id}/analyze` | Solicita, salva e retorna sugestões de IA |
| GET | `/occurrences/{id}/history` | Eventos ordenados por data de criação |

Criação:

```json
{
  "title": "Ar-condicionado sem refrigeração",
  "description": "Exemplo fictício: equipamento ligado sem resfriar o ambiente.",
  "location": "Praça de alimentação",
  "category": "hvac",
  "equipment_id": null
}
```

`title`, `description`, `location` obrigatórios. Limites: 240, 5.000 e 240 caracteres. Textos são aparados e não podem ficar vazios. `category` e `equipment_id` são opcionais; equipamento referenciado deve existir. Servidor gera UUID, datas, `status=open` e `priority=untriaged`.

A resposta inclui os campos acima mais `id`, `status`, `priority`, `created_at`, `updated_at`, `ai_analysis`, `assigned_team`, `recommendation`, `ai_reviewed_at`. Campos de análise, equipe, recomendação e revisão começam nulos. Somente o detalhe inclui `tasks`.

PATCH aceita `title`, `description`, `location`, `category`, `equipment_id`, `status`, `priority`, `assigned_team`, `recommendation`, `confirm_ai_review`. Somente categoria, equipamento, equipe e recomendação aceitam `null` para limpar. Equipe limita-se a 240 caracteres; recomendação a 5.000. Campos desconhecidos, IDs/datas enviados e PATCH vazio retornam 422.

`confirm_ai_review=true` registra a revisão humana de uma análise existente e atual. O frontend envia explicitamente os valores escolhidos; nenhuma sugestão é copiada automaticamente. Exemplo:

```json
{"category":"hvac","priority":"high","assigned_team":"Climatização","confirm_ai_review":true}
```

A classificação manual funciona sem IA, omitindo a confirmação. Alterar título, descrição, local ou equipamento invalida `ai_analysis` e `ai_reviewed_at`. Se o relato mudar na mesma requisição de confirmação, retorna 409 sem salvar. O vínculo `created_by` e a autoria autenticada não estão implementados.

## Enums

| Campo | Valores |
| --- | --- |
| occurrence.status | `open`, `in_progress`, `resolved` |
| occurrence.priority | `untriaged`, `low`, `medium`, `high`, `critical` |
| category | `hvac`, `electrical`, `plumbing`, `cleaning`, `other` |
| task.status | `pending`, `in_progress`, `completed` |
| equipment.operational_status | `operational`, `maintenance`, `out_of_service` |
| user.role | `operator`, `technician`, `manager` (descritivos; não autorizam acesso) |

## Análise por IA

OpenAI Responses API, modelo configurável pelo backend (`gpt-5.4-mini` por padrão), resposta estruturada validada. Exemplo de formato, sem representar análise real validada:

```json
{
  "category": "hvac",
  "suggested_priority": "medium",
  "suggested_team": "Manutenção de climatização",
  "rationale": "O relato descreve perda de refrigeração; é necessário confirmar o impacto.",
  "recommendation": "Encaminhar para avaliação da equipe responsável.",
  "is_simulation": true
}
```

A prioridade sugerida não pode ser `untriaged`. Equipe sugerida é texto, não um usuário atribuído. O operador escolhe o responsável ao criar a tarefa. Categoria, prioridade e decisões reais exigem revisão conforme o contexto.

Falha, timeout, limite de uso, recusa ou estrutura inválida retornam 503, mantendo ocorrência e análise anterior. A edição concorrente da ocorrência durante a chamada descarta a resposta com 409. A análise não cria tarefas nem encerra ocorrências.

`AI_SIMULATION=true` habilita explicitamente uma resposta demonstrativa determinística e retorna `is_simulation=true`. O modo real retorna `false`. A interface deve indicar a simulação; falhas reais não a ativam automaticamente. `store=false` é enviado ao provedor, sem representar garantia geral de retenção zero. A chave fica exclusivamente no backend.

**Limitação verificada em 13/11/2026:** a chamada real retornou HTTP 429, código `credit_balance_exhausted`. Os testes controlados da integração passaram, mas a análise real depende de saldo na conta da API e nova validação.

## Responsáveis

- `POST /users`: `name` obrigatório (até 240 caracteres), `role` opcional, padrão `technician`; retorna 201 com `id`, `name`, `role`, `is_active`.
- `GET /users?role=technician`: lista somente ativos, por nome; filtro opcional.

É um catálogo para atribuição de tarefas com dados fictícios, sem login ou verificação de identidade. Não há endpoint público para alterar ativação neste MVP. Referência inexistente retorna 404; responsável inativo retorna 409.

## Tarefas e regras de atendimento

| Método | Rota | Comportamento |
| --- | --- | --- |
| GET | `/tasks` | Lista por prazo; filtros combináveis `status`, `assigned_to`, `occurrence_id` |
| POST | `/tasks` | Cria tarefa pendente, retorna 201 |
| GET | `/tasks/{id}` | Detalhe |
| PATCH | `/tasks/{id}` | Atualiza campos enviados, retorna tarefa |

Criação exige `occurrence_id`, `title`, `assigned_to`, `due_at`; `details` é opcional. Prazo deve conter fuso horário. Título até 240 caracteres; detalhes até 5.000. Resposta inclui esses campos, `id`, `status`, `completed_at`, `created_at`, `updated_at`. PATCH aceita título, detalhes, responsável, prazo e status. Somente detalhes aceitam `null`.

Transições: `pending` → `in_progress`; `in_progress` → `pending` ou `completed`; `completed` → `in_progress`. Repetir o mesmo status não cria evento novo. Transição inválida retorna 409.

Iniciar tarefa muda uma ocorrência aberta para `in_progress`. Concluir tarefa grava `completed_at` e não encerra a ocorrência automaticamente. Reativar limpa a data de conclusão. Resolver a ocorrência exige PATCH explícito e nenhuma tarefa pendente ou em andamento (409 caso contrário). Sem tarefas, pode haver encerramento manual. Criar ou reativar tarefa de ocorrência resolvida exige reabertura prévia.

Escritas de tarefa e encerramento bloqueiam a ocorrência durante a transação para serializar as decisões. O histórico é persistido na mesma transação da alteração.

## Equipamentos

| Método | Rota | Comportamento |
| --- | --- | --- |
| GET | `/equipment` | Lista por nome; filtros `operational_status`, `location` |
| POST | `/equipment` | Cria; retorna 201 |
| GET | `/equipment/{id}` | Detalhe |
| PATCH | `/equipment/{id}` | Atualiza campos enviados |
| GET | `/equipment/{id}/occurrences` | Ocorrências vinculadas |

Criação exige `name`, `type`, `location`; `operational_status` padrão `operational`. Limites: 240, 120 e 240 caracteres. Cada resposta inclui seu `id`. PATCH aceita os mesmos campos, sem `null`. Filtros de local de equipamento e ocorrência procuram trecho literal; a comparação de caixa depende da configuração do banco e não remove acentos.

## Histórico

Cada evento inclui `id`, `occurrence_id`, `task_id` opcional, `event_type`, `previous_status`, `new_status`, `details`, `created_at`. Valores de status podem ser nulos em eventos sem transição. Tipos atuais: `occurrence_created`, `occurrence_updated`, `occurrence_status_changed`, `task_created`, `task_updated`, `task_status_changed`, `ai_analyzed`, `ai_reviewed`.

O histórico não comprova autoria individual: a API ainda não autentica usuários. Eventos anteriores à migração de histórico não são reconstruídos.

## Dashboard

`GET /dashboard/summary` retorna contagens reais, inclusive chaves com zero:

```json
{
  "occurrences_total": 0,
  "occurrences_by_status": {"open":0,"in_progress":0,"resolved":0},
  "occurrences_by_priority": {"untriaged":0,"low":0,"medium":0,"high":0,"critical":0},
  "tasks_pending": 0,
  "equipment_by_status": {"operational":0,"maintenance":0,"out_of_service":0}
}
```

`tasks_pending` soma `pending` e `in_progress`. Os exemplos zerados ilustram formato. As consultas usam a mesma visão transacional do banco.

## Erros e disponibilidade

- 404: recurso ou referência inexistente.
- 409: conflito com regras de status, revisão ou alteração concorrente.
- 422: entrada inválida, no formato de validação FastAPI.
- 503: banco ou IA indisponíveis, com mensagem sem credenciais.

Erros de domínio usam `{"detail":"Mensagem compreensível"}`. O frontend trata `detail` como texto ou lista de validação, sem depender de uma mensagem literal. `GET /health` e `GET /health/ready` ficam fora de `/api/v1`; a segunda verifica conexão com o banco.
