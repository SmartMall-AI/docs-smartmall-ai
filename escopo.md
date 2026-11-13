# Escopo do MVP

O SmartMall AI busca reduzir a dispersão de informações no atendimento operacional de um shopping, centralizando registros e acompanhando o encaminhamento das equipes. A redução de tempo de atendimento é uma hipótese a avaliar, não um resultado comprovado.

## Usuários

- Colaborador: registra o problema com descrição e local.
- Manutenção: consulta e atualiza o atendimento.
- Coordenação: revisa prioridades, distribui tarefas e acompanha indicadores.

## Entrega essencial

1. Cadastrar, listar e consultar uma ocorrência persistida.
2. Solicitar uma análise de IA em etapa separada.
3. Revisar categoria, prioridade, equipe indicada e recomendação.
4. Criar e atualizar uma tarefa vinculada à ocorrência.
5. Consultar indicadores calculados a partir do banco.

Filtros, cadastro básico de equipamentos e histórico de status vêm na sequência. A validação com usuários precisa de tempo reservado durante o evento, pois é a prática sorteada prevista no planejamento.

## Fora da primeira entrega

Upload de fotos, notificações, sensores/IoT, exportações e autenticação completa com permissões refinadas dependem de tempo adicional. A equipe deve definir os limites de acesso antes de expor qualquer ambiente com dados reais; a demonstração usa dados fictícios.

## Critérios de aceite do fluxo

| Etapa | Evidência esperada |
| --- | --- |
| Registro | Ocorrência reaparece após recarregar a página |
| Análise | Sugestões estruturadas aparecem sem substituir a decisão humana |
| Falha da IA | Ocorrência continua disponível e permite nova tentativa |
| Revisão | Usuário consegue ajustar e salvar categoria e prioridade |
| Tarefa | Responsável, prazo e status são persistidos |
| Dashboard | Contagens refletem os registros e suas atualizações |
| Validação | Observações e melhoria aplicada são documentadas, ou a ausência de problemas relevantes é registrada com a limitação da amostra |

## Decisões pendentes

Definir provedor de IA, versões das dependências, responsáveis reais, prazo do evento e estratégia de acesso à demonstração. Nenhuma dessas decisões pode ser inferida como já aprovada apenas pela existência deste documento.
