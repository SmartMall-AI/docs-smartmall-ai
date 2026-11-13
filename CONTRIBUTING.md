# Guia de contribuição

## Divisão do trabalho

Código de servidor fica em `be-smartmall-ai`, interface em `fe-smartmall-ai` e documentação compartilhada neste repositório. A separação segue a organização por frentes da equipe.

## Alterações

Criar uma branch por tarefa, por exemplo `feat/occurrences` ou `docs/api-contract`. Abrir um pull request com o problema resolvido, o comportamento entregue e a verificação realizada. Não apresentar uma tarefa como concluída apenas por existir uma tela ou arquivo reservado.

Se uma alteração modificar a comunicação entre frontend e backend, atualizar `contrato-api.md` e avisar a outra frente antes da integração.

## Critério de conclusão

- A entrega satisfaz o critério do backlog.
- O fluxo afetado foi verificado com entradas e falhas relevantes.
- A documentação corresponde ao comportamento entregue.
- Não há chaves, senhas, arquivos `.env` ou dados reais de participantes no commit.

As regras de revisão e proteção de branches serão combinadas pela equipe. Este guia não indica que configurações já tenham sido aplicadas no GitHub.
