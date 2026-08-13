# Etapa 7 — DevSecOps e Vídeo Final

## Pipeline DevSecOps proposto

O pipeline a seguir integra as etapas já desenvolvidas ao longo da disciplina, mostrando como a segurança poderia acompanhar o ciclo de vida do BAITA Eventos caso o sistema fosse efetivamente implementado e mantido.

| Momento | Atividade de segurança | Evidência produzida | Condição para continuar |
|---|---|---|---|
| Planejamento | Modelagem de ameaças com STRIDE e definição dos casos de abuso (Etapa 1) | [docs/entrega-1/04-modelagem-stride.md](docs/entrega-1/04-modelagem-stride.md), [docs/entrega-1/05-casos-de-abuso.md](docs/entrega-1/05-casos-de-abuso.md) | As 6 categorias do STRIDE foram cobertas e os casos de abuso estão rastreáveis às ameaças |
| Planejamento | Análise, priorização e tratamento de riscos com o NIST CSF (Etapa 2) | [docs/entrega-2/01-criterios-e-registro-de-riscos.md](docs/entrega-2/01-criterios-e-registro-de-riscos.md) a [docs/entrega-2/06-implementacao-e-risco-residual.md](docs/entrega-2/06-implementacao-e-risco-residual.md) (registro, priorização, tratamento) | Riscos críticos e altos (R09, R01, R02, R05, R04) possuem estratégia de tratamento definida |
| Arquitetura | Requisitos de segurança e decisões de arquitetura derivados dos riscos prioritários (Etapa 3) | [docs/entrega-3/01-requisito-rs01.md](docs/entrega-3/01-requisito-rs01.md), [docs/entrega-3/02-requisito-rs02.md](docs/entrega-3/02-requisito-rs02.md) e [docs/entrega-3/05-decisoes-arquitetura.md](docs/entrega-3/05-decisoes-arquitetura.md), diagrama de arquitetura segura | Cada requisito (RS01-RS03) está vinculado a um risco de origem e a um critério de verificação |
| Código | Implementação das práticas de código seguro, com testes definidos antes da implementação (Etapa 4) | [docs/entrega-4/01-contexto-e-testes-autorizacao.md](docs/entrega-4/01-contexto-e-testes-autorizacao.md), [docs/entrega-4/02-implementacao-autorizacao.md](docs/entrega-4/02-implementacao-autorizacao.md) e [docs/entrega-4/03-resultados-e-referencia-autorizacao.md](docs/entrega-4/03-resultados-e-referencia-autorizacao.md) (autorização e sessões) | Os testes de segurança (casos válido e malicioso) estão definidos e o resultado esperado é seguro |
| Verificação | Análise dinâmica com o OWASP ZAP sobre o Juice Shop (Etapa 5) | [evidencias/etapa-5/](evidencias/etapa-5/) (capturas de tela e relatório) | Os achados críticos foram analisados e receberam proposta de correção |
| Operação | Registro de logs e aplicação das regras de detecção (Etapa 6) | [roteiros/entrega-6/etapa-6-deteccao-de-intrusoes.md](roteiros/entrega-6/etapa-6-deteccao-de-intrusoes.md) | Um alerta gerado é investigado, respondido e registrado antes de ser considerado encerrado |

### Condições que impediriam a continuidade do pipeline

Pelo menos uma das condições abaixo, se identificada em qualquer etapa, deveria interromper o avanço do pipeline até ser corrigida:

1. **Teste de segurança reprovado** — um dos testes definidos na Etapa 4 (ex.: tentativa de acesso sem autorização sendo aceita pelo sistema) falha, indicando que o controle de autorização ou de sessão não funciona como esperado.
2. **Vulnerabilidade crítica não analisada** — um alerta crítico do ZAP (Etapa 5) é identificado, mas nenhuma análise de impacto ou proposta de correção é registrada.
3. **Falha no controle de acesso** — a verificação de vínculo do usuário com o evento (base dos requisitos RS02/RS03 e dos riscos R01/R02) não é aplicada corretamente em uma rota da API, permitindo acesso a recursos de outro evento.
4. **Segredo encontrado no repositório** — credenciais, chaves de API ou strings de conexão com o MongoDB e o serviço de e-mail são encontradas versionadas diretamente no código-fonte.

Enquanto qualquer uma dessas condições estiver presente, a implantação (deploy) não deveria avançar, e o time deveria retornar à etapa correspondente (código, arquitetura ou verificação) para tratar o problema antes de seguir para a operação.

## Roteiro do vídeo final

**Duração alvo:** 5 a 8 minutos.

| Tempo aproximado | Conteúdo | O que apresentar |
|---|---|---|
| 0:00 – 0:45 | Introdução e sistema escolhido | Apresentação rápida do grupo e do BAITA Eventos: o que o sistema faz, quem são os usuários (participantes, organizadores, avaliadores, administradores) |
| 0:45 – 1:45 | Principais ameaças e casos de abuso | Destacar 2-3 casos de abuso mais relevantes (ex.: CA01 — comprometimento de conta, CA05 — injeção de conteúdo malicioso, CA09 — indisponibilidade da plataforma) e a que categorias STRIDE eles se relacionam |
| 1:45 – 2:45 | Riscos prioritários | Mostrar a tabela de priorização da Etapa 2 e explicar por que R09, R01 e R02 ficaram no topo (maior pontuação e maior abrangência) |
| 2:45 – 3:45 | Decisões de arquitetura | Apresentar o diagrama da arquitetura segura e 1-2 decisões de arquitetura (RS01-RS03), explicando qual risco cada uma trata |
| 3:45 – 4:45 | Práticas de código seguro | Mostrar rapidamente as duas práticas implementadas na Etapa 4 (autorização e proteção de sessões), incluindo um teste malicioso e o resultado esperado |
| 4:45 – 5:30 | Resultados da verificação (ZAP) | Mostrar 1-2 alertas encontrados no Juice Shop e a correção proposta para cada um |
| 5:30 – 6:15 | Regras de detecção | Apresentar as 3 regras da Etapa 6 (R09, R01, R05) e o que aconteceria depois de um alerta |
| 6:15 – 7:00 | Pipeline DevSecOps | Mostrar a tabela do pipeline e pelo menos uma condição que bloquearia a continuidade |
| 7:00 – 7:30 | Aprendizados do grupo | Cada integrante comenta brevemente uma dificuldade ou aprendizado da disciplina |
| 7:30 – 8:00 | Encerramento | Fechamento e agradecimento |

### Observações para a gravação

- Não é necessário demonstrar todas as tabelas produzidas ao longo do trabalho — o foco deve estar nas decisões e na evolução do projeto, não em ler documentos na tela.
- A participação de cada integrante pode ser dividida por etapa (quem fez a Etapa 3 apresenta a Etapa 3, e assim por diante), mas a avaliação individual também considera os commits de cada um ao longo do repositório, então a apresentação no vídeo não substitui a participação escrita.
- Vale gravar a tela mostrando rapidamente a estrutura do repositório no início ou no fim, para reforçar que tudo está versionado no GitHub.