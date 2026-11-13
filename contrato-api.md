# Proposta inicial de contrato da API

Este contrato detalha o planejamento para permitir o trabalho separado do frontend e do backend. Cadastro, listagem e detalhe de ocorrências têm implementação técnica no [PR #1 do backend](https://github.com/SmartMall-AI/be-smartmall-ai/pull/1), ainda em revisão. As demais rotas continuam propostas. O alinhamento entre as frentes e a validação com usuários ainda precisam ser concluídos.

Base sugerida: `/api/v1`. JSON com campos em `snake_case`, identificadores UUID em string e datas ISO 8601 com fuso horário. A interface traduz os valores de enum para português.

## Ocorrências

| Método | Rota | Comportamento proposto |
| --- | --- | --- |
| GET | `/occurrences?status=open&priority=high` | Lista filtrada; sem filtros lista todos os registros do MVP |
| POST | `/occurrences` | Cria e persiste uma ocorrência, retornando 201 |
| GET | `/occurrences/{id}` | Retorna detalhes, análise e tarefas vinculadas |
| PATCH | `/occurrences/{id}` | Atualiza campos permitidos, inclusive sugestões revisadas |
| POST | `/occurrences/{id}/analyze` | Solicita e salva uma análise, sem aplicar sua prioridade automaticamente |

Corpo proposto para criação:

```json
{
  "title": "Ar-condicionado sem refrigeração",
  "description": "Equipamento ligado, mas sem resfriar a praça de alimentação.",
  "location": "Praça de alimentação",
  "category": "hvac",
  "equipment_id": null
}
```

`title`, `description` e `location` são obrigatórios; `category` e `equipment_id` são opcionais. A API define inicialmente `status=open` e `priority=untriaged`. O vínculo `created_by` depende da estratégia de identidade a ser definida; não aceitar identidades arbitrárias como autenticação.

Resposta de uma ocorrência: os campos acima mais `id`, `status`, `priority`, `created_at`, `updated_at` e `ai_analysis` (nulo antes da análise). O detalhe inclui `tasks`, uma lista das tarefas vinculadas.

Campos editáveis propostos: `title`, `description`, `location`, `category`, `priority`, `status` e `equipment_id`. IDs e datas de auditoria são controlados pelo servidor.

Enums sugeridos:

| Campo | Valores |
| --- | --- |
| occurrence.status | `open`, `in_progress`, `resolved` |
| occurrence.priority | `untriaged`, `low`, `medium`, `high`, `critical` |
| category | `hvac`, `electrical`, `plumbing`, `cleaning`, `other` |
| task.status | `pending`, `in_progress`, `completed` |
| equipment.operational_status | `operational`, `maintenance`, `out_of_service` |

## Análise por IA

Exemplo **ilustrativo**, não produzido por um teste real:

```json
{
  "category": "hvac",
  "suggested_priority": "medium",
  "suggested_team": "Manutenção de climatização",
  "rationale": "A descrição indica perda de refrigeração e não relata perigo imediato.",
  "recommendation": "Encaminhar para avaliação da equipe responsável e confirmar o impacto no local.",
  "is_simulation": true
}
```

A equipe indicada é uma sugestão textual, não um usuário atribuído. O operador seleciona o responsável ao criar a tarefa. A prioridade real deve ser decidida conforme o contexto e revisada por uma pessoa.

O backend valida a estrutura da resposta. Se o provedor falhar, retorna erro 503 e mantém a ocorrência e qualquer análise válida anterior. Uma resposta demonstrativa só deve ser usada em modo explícito de simulação, com `is_simulation=true` e indicação visível no frontend. Respostas reais usam `is_simulation=false`.

## Tarefas

| Método | Rota | Comportamento proposto |
| --- | --- | --- |
| GET | `/tasks` | Lista tarefas |
| POST | `/tasks` | Cria tarefa com `occurrence_id`, `title`, `assigned_to` e `due_at` |
| PATCH | `/tasks/{id}` | Atualiza título, responsável, prazo ou status |

`assigned_to` referencia um usuário existente; `due_at` é uma data ISO 8601. A resposta inclui `id`, `occurrence_id`, `title`, `assigned_to`, `status`, `due_at` e `completed_at` (nulo antes da conclusão).

Regra inicial proposta: a ocorrência é atualizada explicitamente pelo operador. Concluir uma tarefa não resolve automaticamente a ocorrência, pois pode haver outras tarefas pendentes. O histórico deve registrar as alterações quando implementado.

## Equipamentos

| Método | Rota | Comportamento proposto |
| --- | --- | --- |
| GET | `/equipment` | Lista equipamentos |
| POST | `/equipment` | Cria com `name`, `type`, `location` e `operational_status` |
| PATCH | `/equipment/{id}` | Atualiza dados permitidos |

Cada registro retornado inclui seu `id`. A API deve validar equipamentos e responsáveis referenciados antes de salvar vínculos.

## Dashboard

`GET /dashboard/summary` retorna:

```json
{
  "occurrences_total": 0,
  "occurrences_by_status": {"open": 0, "in_progress": 0, "resolved": 0},
  "occurrences_by_priority": {"untriaged": 0, "low": 0, "medium": 0, "high": 0, "critical": 0},
  "tasks_pending": 0,
  "equipment_by_status": {"operational": 0, "maintenance": 0, "out_of_service": 0}
}
```

`tasks_pending` soma tarefas `pending` e `in_progress`. Os exemplos zerados representam formato, não métricas reais.

## Listas e erros

Para a primeira integração, listas retornam um array JSON. Paginação será definida se necessária e deve atualizar este contrato antes da adoção.

- 404 para um recurso inexistente.
- 422 para entradas inválidas, usando o formato de validação do FastAPI.
- 503 para indisponibilidade do provedor de IA.
- Outros erros de domínio usam `{"detail": "Mensagem compreensível"}`.

O frontend deve tratar `detail` tanto como texto quanto como lista de erros de validação, sem depender de uma mensagem literal.
