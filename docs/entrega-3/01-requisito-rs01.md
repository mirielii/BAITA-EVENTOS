# 18. Requisitos de segurança

Os requisitos de segurança foram derivados dos riscos prioritários identificados na Etapa 2. Cada requisito descreve uma condição verificável que a arquitetura da plataforma BAITA Eventos deve atender para reduzir a probabilidade ou o impacto do risco correspondente.

## 18.1 RS01 — Disponibilidade da plataforma

| ID   | Risco de origem | Requisito de segurança | Critério de verificação |
| ---- | --------------- | ---------------------- | ----------------------- |
| RS01 | R09 — Indisponibilidade da plataforma | A plataforma deverá limitar requisições abusivas à API REST, registrar eventos de disponibilidade e manter cópias de segurança testadas do MongoDB para permitir a recuperação dos serviços e dados essenciais. | Testes com volume excessivo de requisições deverão demonstrar a aplicação de limites sem indisponibilizar os serviços principais. Os registros de monitoramento deverão indicar falhas e alertas. Um teste de restauração deverá recuperar os dados do MongoDB a partir de uma cópia de segurança, verificando a integridade dos dados recuperados e a retomada das funcionalidades essenciais. |

### Justificativa

O requisito foi derivado do R09, classificado como crítico e priorizado em primeiro lugar na Etapa 2. A API REST, o MongoDB e o serviço de e-mail são componentes relevantes para inscrições, check-ins, avaliações e operações administrativas. Limitação de requisições, monitoramento e cópias de segurança reduzem a probabilidade de indisponibilidade e apoiam a recuperação caso ocorra uma falha ou ataque.

Além disso, a verificação da restauração das cópias de segurança é importante para garantir que o mecanismo de recuperação não exista apenas de forma teórica, mas seja capaz de recuperar os dados necessários em uma situação de falha. Da mesma forma, o monitoramento permite identificar situações anormais e apoiar uma resposta mais rápida a problemas de disponibilidade. Dessa forma, o requisito contribui tanto para a prevenção quanto para a recuperação diante de situações que possam comprometer a continuidade dos serviços da plataforma.