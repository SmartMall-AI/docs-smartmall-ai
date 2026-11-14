# SmartMall AI

Centralização de ocorrências operacionais de um shopping, com IA para apoiar a triagem e revisão humana para confirmar as decisões.

Este repositório é o ponto de entrada para conhecer a proposta, acompanhar o desenvolvimento e consultar as evidências do hackathon. A redução do tempo de atendimento é uma hipótese a validar.

**Liderança do projeto e backend:** [João Geraldo — @geraldojoao](https://github.com/geraldojoao). A equipe tem quatro pessoas; as demais funções ainda serão distribuídas.

**Para a banca:** comece pelo [roteiro de avaliação](avaliacao.md). Ele distingue planejamento, implementação em revisão e evidências já verificadas.

## Índice

- [Escopo e critérios de aceite](escopo.md)
- [Arquitetura e responsabilidades](arquitetura.md)
- [Contrato implementado da API](contrato-api.md)
- [Entrega do backend e guia de integração](entrega-backend.md)
- [Backlog por frente](backlog.md)
- [Validação com usuários](validacao-usuarios.md)
- [Roteiro de demonstração](demonstracao.md)
- [Guia de contribuição](CONTRIBUTING.md)
- [Liderança, permissões e revisão](governanca.md)
- [Equipe e responsabilidades](equipe.md)
- [Roteiro e evidências para a banca](avaliacao.md)

## Divisão dos repositórios

| Repositório | Responsabilidade | Situação |
| --- | --- | --- |
| [be-smartmall-ai](https://github.com/SmartMall-AI/be-smartmall-ai) | API, banco e integração com IA | Fluxo do MVP integrado à main no [PR #3](https://github.com/SmartMall-AI/be-smartmall-ai/pull/3); análise real pendente de saldo da API |
| [fe-smartmall-ai](https://github.com/SmartMall-AI/fe-smartmall-ai) | Interface, formulários e dashboard | Estrutura inicial; telas ainda não implementadas |
| [docs-smartmall-ai](https://github.com/SmartMall-AI/docs-smartmall-ai) | Escopo, contrato, governança e evidências | Documentação do desenvolvimento |

A organização é [SmartMall-AI](https://github.com/SmartMall-AI). Os três repositórios são públicos.

## Acompanhamento

- [Fluxograma no Miro](https://miro.com/app/board/uXjVEOPHRG4=/?share_link_id=465697087214): fluxo planejado e responsabilidades dos módulos.
- [Trello](https://trello.com/b/TMJ0EfTl): etapas, responsáveis, checklists e evidências. O quadro é privado; sua consulta exige acesso concedido pela liderança.
- [Pull request do backend](https://github.com/SmartMall-AI/be-smartmall-ai/pull/3): fluxo do MVP e verificações automáticas públicas.

As etapas de desenvolvimento são: cadastro e consulta; triagem e atendimento; validação e apresentação. O fluxo de contribuição é **branch → pull request → revisão da liderança → integração à main**.

## Fluxo principal

Registrar ocorrência → solicitar análise → revisar sugestões → atribuir tarefa → atualizar atendimento → consultar dashboard.

## Estado do planejamento

O backend implementa ocorrências, triagem estruturada, revisão humana, tarefas, equipamentos, filtros, histórico e dashboard. Foram verificados 46 testes com PostgreSQL real. A chamada real da OpenAI foi recusada por saldo esgotado; testes controlados e modo explícito de simulação estão disponíveis. As telas, a integração entre frentes, a validação com usuários e a hospedagem continuam pendentes. Consulte a [entrega do backend](entrega-backend.md).
