# Guia de contribuição

## Liderança e responsabilidades

[@geraldojoao](https://github.com/geraldojoao) lidera o projeto, revisa as alterações e coordena a integração à `main`. Backend fica em `be-smartmall-ai`, frontend em `fe-smartmall-ai` e documentação compartilhada neste repositório.

Os integrantes autorizados têm escrita para trabalhar em branches. A `main` é protegida e a integração é reservada à liderança. Quem ainda não tem acesso de escrita pode contribuir por um fork e enviar um pull request.

## Fluxo de uma entrega

1. Escolha um cartão do Trello e alinhe o escopo com a liderança.
2. Atualize a `main` local e crie uma branch por tarefa.
3. Implemente, verifique e faça commits com sua própria identidade Git.
4. Envie a branch e abra um pull request para `main`.
5. Preencha o modelo, vincule o cartão e as evidências e aguarde a revisão de @geraldojoao.
6. Corrija o que for solicitado e aguarde nova revisão quando houver novos commits.
7. A liderança integra o PR após aprová-lo e conferir os checks exigidos.

Exemplo, a partir de um repositório já clonado:

```powershell
git switch main
git pull --ff-only origin main
git switch -c feat/FE-02-formulario-ocorrencia
# Implemente e verifique a entrega.
git add caminho/do/arquivo
git commit -m "feat: adiciona formulario de ocorrencia"
git push -u origin feat/FE-02-formulario-ocorrencia
```

Abra o pull request na página do repositório. Preserve nos commits o nome e o e-mail da própria conta GitHub; não use a identidade de outro integrante. Use nomes como `feat/BE-02-ocorrencias`, `fix/QA-01-validacao` e `docs/contrato-api`.

## Revisão e integração

- Uma aprovação é exigida e a revisão do responsável definido em `.github/CODEOWNERS` é obrigatória para os integrantes.
- Novos commits invalidam aprovações anteriores; conversas de revisão precisam ser resolvidas.
- Apenas @geraldojoao pode integrar alterações na `main`.
- Force push e exclusão da `main` são bloqueados.
- No backend, o check `check` do GitHub Actions precisa passar e a branch deve estar atualizada com a `main`.
- Alterações são integradas com merge commit, preservando os commits e sua autoria.

O GitHub não permite aprovar o próprio PR. A liderança possui uma exceção nominal à exigência de aprovação de PR para alterações próprias e administração. Os checks obrigatórios continuam exigidos. Sempre que possível, outro integrante deve revisar a entrega da liderança.

Se a comunicação entre frontend e backend mudar, atualize `contrato-api.md` e alinhe a alteração entre as frentes antes da integração.

## Critério de conclusão

- A entrega satisfaz o aceite do cartão e foi verificada com entradas e falhas relevantes.
- O PR vincula os resultados da verificação e a documentação reflete o comportamento entregue.
- Não há chaves, senhas, arquivos `.env` ou dados reais de participantes no commit.
- A liderança concluiu a revisão e integrou a entrega.

No Trello, use somente as cores de andamento e consulte a lista “Legenda das cores”. Os títulos das tarefas mantêm seus nomes e identificadores.
