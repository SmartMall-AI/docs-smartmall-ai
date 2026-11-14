# Entrega do backend e integração da equipe

Responsável: [João Geraldo — @geraldojoao](https://github.com/geraldojoao). Implementação integrada à `main` no [PR #3](https://github.com/SmartMall-AI/be-smartmall-ai/pull/3), com autoria dos commits no seu perfil. Esta entrega inclui BE-01, BE-02, BE-03, BE-04, BE-05, AI-01 e a parte de API de HIST-01. A revisão entre frentes, as telas e o aceite integrado não estão concluídos.

## Evidências

- [Commit do backend](https://github.com/SmartMall-AI/be-smartmall-ai/commit/f1500db).
- [Checks aprovados do PR](https://github.com/SmartMall-AI/be-smartmall-ai/actions/runs/43542502531) e [merge na main](https://github.com/SmartMall-AI/be-smartmall-ai/commit/a335e02a63a201c5381417c9a83d2d1de89d6f55). As proteções foram mantidas; integração pela exceção nominal do líder, sem revisão independente de outro integrante.
- 46 testes passaram localmente com PostgreSQL 18.7 real, além de lint, formatação e Alembic sem divergências. Os testes criam um banco descartável; não apagam o banco de desenvolvimento.
- Casos de IA exercitam o SDK com transporte HTTP controlado: saída estruturada, timeout, limite de uso, recusa, resposta inválida, preservação da análise anterior e alteração concorrente.
- Chamada real com relato fictício retornou HTTP 429 / `credit_balance_exhausted`; o backend retornou 503 e preservou a ocorrência. Não há resultado real de IA aprovado nesta entrega.
- Fluxo HTTP local verificado com servidor Uvicorn: Swagger e prontidão 200, cadastro 201, análise explicitamente simulada, revisão, criação/início/conclusão de tarefa, bloqueio de encerramento prematuro (409), resolução explícita, dez eventos de histórico e consulta do dashboard.
- Seed fictício idempotente e OpenAPI exportado disponíveis no backend. Nenhuma credencial integra o repositório.

## Primeira integração

O backend [README](https://github.com/SmartMall-AI/be-smartmall-ai/blob/main/README.md) traz a execução local. O contrato está em [contrato-api.md](contrato-api.md).

1. João inicia PostgreSQL, aplica `uv run alembic upgrade head`, executa o seed opcional e inicia Uvicorn em `127.0.0.1:8000`.
2. Frontend A implementa formulário/lista/detalhe usando ocorrências; configura a URL local da API. Validar persistência recarregando a página.
3. Frontend B implementa revisão das sugestões, seleção de responsável em `/users`, tarefas e indicadores. Pode começar consumindo o seed pelo Swagger.
4. QA/documentação verifica o fluxo abaixo e registra respostas, evidências e dificuldades. As funções nominais ainda precisam ser distribuídas pela equipe.

## Roteiro verificável pelo Swagger

1. Abrir `/docs`; cadastrar equipamento ou usar o seed.
2. Criar ocorrência fictícia. Consultar novamente e confirmar `untriaged`/`open`.
3. Com `AI_SIMULATION=true` e servidor reiniciado, solicitar análise. Explicar que é demonstração. Para testar o provedor real, voltar a `false`, garantir saldo e reiniciar.
4. Salvar classificação humana por PATCH. Se houver análise atual, enviar `confirm_ai_review=true`; para classificação manual sem IA, omitir esse campo.
5. Criar tarefa com responsável existente e prazo com fuso horário. Confirmar vínculo no detalhe da ocorrência.
6. Tentar resolver enquanto tarefa está pendente: esperar 409. Iniciar e concluir tarefa; confirmar que o encerramento da ocorrência continua explícito.
7. Resolver ocorrência e conferir histórico e dashboard. Reativação de tarefa exige primeiro reabrir a ocorrência.
8. Demonstrar falha da IA sem perder o relato; os testes automatizados cobrem falhas controladas. Não exibir chave, `.env` ou dados reais na apresentação.

## Pendências reais

- Repor saldo da API e validar uma análise real; sem isso, usar simulação claramente indicada ou triagem manual.
- Implementar e integrar frontend; revisar contrato entre as frentes (DOC-01).
- Executar aceite integrado (INT-01), validação com usuários (VAL-01) e ensaio (DEMO-01).
- Definir participantes, prazos e acesso à apresentação.
- Login, autorização, proteção contra abuso e hospedagem pública continuam fora desta entrega. Papéis de responsáveis não aplicam permissões. Usar dados fictícios em ambiente local.
- Compose não foi executado localmente porque Docker não estava disponível; PostgreSQL nativo e CI são as verificações executadas.

O término técnico do backend libera João para apoiar os colegas, mas não equivale à conclusão do produto inteiro nem a evidência de benefício real para usuários.
