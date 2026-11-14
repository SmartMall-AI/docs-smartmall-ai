# Roteiro e evidências para a banca

Este roteiro organiza a consulta ao projeto. Não representa critérios oficiais do evento nem comprova funcionalidades apenas por estarem planejadas.

## 1. Problema e proposta

O SmartMall AI busca centralizar ocorrências operacionais de um shopping. A IA sugere uma classificação e um encaminhamento; o gestor revisa as sugestões. O atendimento deve continuar manualmente se a IA estiver indisponível.

- [Escopo e critérios de aceite](escopo.md)
- [Arquitetura](arquitetura.md)
- [Fluxograma no Miro](https://miro.com/app/board/uXjVEOPHRG4=/?share_link_id=465697087214)

## 2. Estado da entrega

| Frente | Situação verificável |
| --- | --- |
| Estrutura dos repositórios | Três repositórios separados e públicos |
| Backend do MVP | Ocorrências, tarefas, equipamentos, filtros, histórico e dashboard integrados à main no [PR #3](https://github.com/SmartMall-AI/be-smartmall-ai/pull/3) |
| Verificação do backend | 46 testes passaram com PostgreSQL real, além de lint, formatação e verificação da migração |
| Frontend | Estrutura inicial; telas ainda não implementadas |
| IA | Integração e testes controlados implementados; chamada real recusada por saldo esgotado (`credit_balance_exhausted`); simulação explícita disponível |
| Validação com usuários | Roteiro preparado; sessões e resultados ainda não coletados |
| Hospedagem pública | Ambientes ainda não provisionados |

## 3. Evidências técnicas

- [Commit da base FastAPI e PostgreSQL](https://github.com/SmartMall-AI/be-smartmall-ai/commit/e18abcfd6095164b0126af7e658d565d817d90fd)
- [Commit de cadastro, listagem e detalhe](https://github.com/SmartMall-AI/be-smartmall-ai/commit/21b4ef1f6bd7c2947f60e4cb488d2aee8ad8fab2)
- [Execução aprovada dos checks do PR](https://github.com/SmartMall-AI/be-smartmall-ai/actions/runs/43513676632)
- [README de execução do backend](https://github.com/SmartMall-AI/be-smartmall-ai/blob/main/README.md)
- [Checks atuais do PR #3](https://github.com/SmartMall-AI/be-smartmall-ai/pull/3/checks)
- [Entrega do backend e limites da verificação](entrega-backend.md)
- [Commit do fluxo completo do backend](https://github.com/SmartMall-AI/be-smartmall-ai/commit/f1500db)

A verificação HTTP local também confirmou criação com status 201 e consulta do registro após reiniciar a API. A URL local do Swagger é uma ferramenta de desenvolvimento, não um ambiente público para a banca.

## 4. Organização e participação

- [Equipe e frentes](equipe.md)
- [Liderança e permissões](governanca.md)
- [Fluxo de contribuição](CONTRIBUTING.md)
- [Trello](https://trello.com/b/TMJ0EfTl): quadro privado com três etapas, checklists e legenda de cores; acesso depende de autorização da liderança.

Pull requests e commits preservam a autoria de cada integrante. O quadro é atualizado conforme o trabalho real, sem apresentar tarefas planejadas como concluídas.

## 5. Demonstração e limites

Use o [roteiro de demonstração](demonstracao.md) e o [plano de validação](validacao-usuarios.md). Ajuste a apresentação ao que estiver funcionando. Resultados de uso, redução de tempo e aprovação de usuários só devem ser apresentados quando houver evidências coletadas.
