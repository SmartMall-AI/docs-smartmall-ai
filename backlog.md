# Backlog do hackrun

Este arquivo mantém as entregas da equipe. **BE-01 a BE-05, AI-01 e a parte de API de HIST-01 estão implementados** no [PR #3 do backend](https://github.com/SmartMall-AI/be-smartmall-ai/pull/3), com 46 testes locais aprovados. A análise real da OpenAI depende de saldo na conta; o restante do produto, a revisão entre frentes e o aceite integrado continuam pendentes. Veja a [entrega do backend](entrega-backend.md). O [Trello](https://trello.com/b/TMJ0EfTl) acompanha o andamento por cores. Não foram criadas issues para duplicar este backlog nem um quadro GitHub Projects.

O Trello mantém listas por etapa e uma única cor de andamento por cartão. A legenda final explica amarelo, laranja, roxo, verde, vermelho e azul; não adicionar prefixos de estado aos títulos.

| ID | Prioridade | Frente | Entrega | Critério de conclusão | Dependência |
| --- | --- | --- | --- | --- | --- |
| DOC-01 | P0 | Documentação | Alinhar contrato da API | Campos, enums, rotas e regra de status aceitos pelas duas frentes | Nenhuma |
| BE-01 | P0 | Backend | Preparar FastAPI e PostgreSQL | API inicia e migrações criam as entidades necessárias | DOC-01 |
| FE-01 | P0 | Frontend | Preparar React, navegação e cliente HTTP | Telas-base navegam e cliente usa URL configurável | DOC-01 |
| BE-02 | P0 | Backend | Criar e consultar ocorrências | Cadastro persiste e detalhe retorna o registro | BE-01 |
| FE-02 | P0 | Frontend | Formulário, lista e detalhe | Usuário registra e encontra a ocorrência após recarregar | FE-01, BE-02 |
| AI-01 | P0 | IA e backend | Serviço de análise estruturada | Retorno validado; falha não elimina ocorrência; simulação identificada | BE-02 |
| FE-03 | P0 | Frontend | Revisar sugestões de IA | Sugestões são editáveis e só aplicadas após ação explícita | FE-02, AI-01 |
| BE-03 | P0 | Backend | Criar e atualizar tarefas | Responsável, prazo e status persistidos | BE-02 |
| FE-04 | P0 | Frontend | Encaminhar e acompanhar tarefas | Tarefa vinculada aparece e pode ser atualizada | FE-02, BE-03 |
| BE-04 | P0 | Backend | Resumo do dashboard | Contagens calculadas do banco conforme contrato | BE-03 |
| FE-05 | P0 | Frontend | Dashboard integrado | Indicadores acompanham os registros e suas alterações | BE-04 |
| INT-01 | P0 | Integração | Validar fluxo completo e falha da IA | Critérios de aceite do escopo demonstrados com dados fictícios | FE-03, FE-04, FE-05 |
| VAL-01 | P0 | Produto | Executar validação com usuários | Sessões e limitações registradas; achados priorizados | Fluxo inicial navegável |
| DEMO-01 | P0 | Produto | Preparar apresentação | Roteiro ensaiado e evidências coerentes com o que funciona | INT-01, VAL-01 |
| BE-05 | P1 | Backend | Filtros e equipamentos | Filtros retornam dados corretos e vínculos são validados | BE-02 |
| FE-06 | P1 | Frontend | Filtros e equipamentos | Interface consome os filtros e consulta equipamentos | BE-05, FE-02 |
| HIST-01 | P1 | Backend e frontend | Histórico de status | Alterações exibem sequência e datas consistentes | Fluxo principal |

## Ordem de integração

Integrar o cadastro persistido primeiro. O frontend pode usar dados fictícios para desenvolver telas enquanto a API é construída, desde que estejam identificados e sejam substituídos na integração. Não deixar a primeira integração para o fim do evento.

A equipe tem quatro pessoas. João Geraldo lidera o projeto e assume backend, dados e integração de IA. A proposta para as outras três frentes é Frontend A, Frontend B e QA/documentação. A distribuição nominal e os prazos ainda serão definidos pela equipe.
