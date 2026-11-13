# Liderança, permissões e revisão

## Estrutura

João Geraldo (`@geraldojoao`) é o líder e único proprietário da organização SmartMall-AI. Ele mantém administração dos três repositórios, revisa alterações e decide sua integração à `main`.

Os integrantes autorizados recebem acesso individual de escrita aos três repositórios. Eles trabalham em branches e enviam pull requests. As funções de frontend e QA/documentação ainda serão distribuídas. A entrada de um novo integrante é coordenada pela liderança.

A permissão base da organização é de leitura. A liderança mantém a administração da organização e dos repositórios; os colaboradores recebem apenas a permissão de escrita necessária para contribuir. Os três repositórios do projeto continuam públicos.

## Política da main

| Controle | Política |
| --- | --- |
| Solicitação de alteração | Pull request para `main` |
| Revisão | Uma aprovação e revisão obrigatória de CODEOWNERS |
| Responsável pela revisão | `@geraldojoao` para todos os arquivos |
| Novos commits | Invalidam aprovações anteriores |
| Conversas de revisão | Devem ser resolvidas |
| Integração na main | Reservada a `@geraldojoao` |
| Force push e exclusão da main | Bloqueados |
| Backend | Check `check` aprovado e branch atualizada com a main |
| Frontend e documentação | Revisão e regras de integração; sem checks funcionais inexistentes |
| Método de integração | Merge commit, preservando autoria e histórico |

A proteção inclui administradores. Existe uma exceção explícita apenas para `@geraldojoao` na exigência de aprovação de PR. Isso permite integrar alterações próprias, já que o autor não pode aprovar seu próprio PR. Essa exceção não dispensa checks obrigatórios nem autoriza outros integrantes a integrar alterações.

Commits locais e commits em branches de trabalho são livres para os integrantes com escrita. O pedido de revisão acontece pelo pull request antes da integração à `main`.

## Evidências e consulta

- Arquivos `.github/CODEOWNERS` nos três repositórios identificam a liderança.
- Pull requests registram alterações, revisão, verificações e integração.
- O GitHub Actions guarda os resultados do backend.
- O Trello registra etapas e andamento; o acesso ao quadro privado é concedido separadamente pela liderança.

Os modelos de pull request e issues padronizam as informações para a equipe e para a banca. Issues não duplicam automaticamente todo o backlog do Trello.
