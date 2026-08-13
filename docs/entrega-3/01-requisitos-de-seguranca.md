# 18. Requisitos de segurança

Os requisitos de segurança foram derivados dos riscos prioritários identificados na Etapa 2. Cada requisito descreve uma condição verificável que a arquitetura da plataforma BAITA Eventos deve atender para reduzir a probabilidade ou o impacto do risco correspondente.

## 18.1 RS01 — Disponibilidade da plataforma

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
| ---- | --------------- | ---------------------- | ----------------------- |
| RS01 | R09 — Indisponibilidade da plataforma | A plataforma deverá limitar requisições abusivas à API REST, registrar eventos de disponibilidade e manter cópias de segurança testadas do MongoDB para permitir a recuperação dos serviços e dados essenciais. | Testes com volume excessivo de requisições deverão demonstrar a aplicação de limites sem indisponibilizar os serviços principais. Os registros de monitoramento deverão indicar falhas e alertas. Um teste de restauração deverá recuperar os dados do MongoDB a partir de uma cópia de segurança, verificando a integridade dos dados recuperados e a retomada das funcionalidades essenciais. |

### Justificativa

O requisito foi derivado do R09, classificado como crítico e priorizado em primeiro lugar na Etapa 2. A API REST, o MongoDB e o serviço de e-mail são componentes relevantes para inscrições, check-ins, avaliações e operações administrativas. Limitação de requisições, monitoramento e cópias de segurança reduzem a probabilidade de indisponibilidade e apoiam a recuperação caso ocorra uma falha ou ataque.

Além disso, a verificação da restauração das cópias de segurança é importante para garantir que o mecanismo de recuperação não exista apenas de forma teórica, mas seja capaz de recuperar os dados necessários em uma situação de falha. Da mesma forma, o monitoramento permite identificar situações anormais e apoiar uma resposta mais rápida a problemas de disponibilidade. Dessa forma, o requisito contribui tanto para a prevenção quanto para a recuperação diante de situações que possam comprometer a continuidade dos serviços da plataforma.

## 18.2 RS02 — Proteção de contas autenticadas

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
|---|---|---|---|
| RS02 | R01 — Comprometimento de contas autenticadas | A plataforma deverá utilizar autenticação federada com Google por meio de OpenID Connect (OIDC) para organizadores, avaliadores e administradores. A aplicação deverá validar os tokens de identidade recebidos, criar sessões locais protegidas, expirar sessões inativas e invalidar sessões locais quando houver indício de comprometimento. | Testes deverão confirmar que somente tokens válidos, emitidos para a aplicação e não expirados permitem a criação de uma sessão. Sessões inativas deverão expirar e sessões locais deverão ser invalidadas quando a conta for bloqueada ou houver indício de comprometimento. |

### Justificativa

O requisito foi derivado do R01, classificado como crítico e priorizado em segundo lugar na Etapa 2. O comprometimento de contas pode permitir que um atacante atue em nome de organizadores, avaliadores ou administradores, comprometendo eventos, avaliações, permissões e configurações da plataforma.

A autenticação será delegada ao Google, reduzindo a necessidade de a plataforma BAITA Eventos armazenar senhas próprias. Entretanto, a plataforma continuará responsável por validar os tokens, proteger e controlar as sessões locais, associar a conta autenticada ao perfil interno adequado e registrar eventos de autenticação para auditoria.

## 18.3 RS03 — Autorização de operações protegidas

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
|---|---|---|---|
| RS03 | R02 — Execução de operações sem autorização | A API REST deverá validar, no servidor e antes de cada operação protegida, a autenticação, o perfil do usuário e seu vínculo com o evento ou atividade envolvidos na solicitação. | Testes com participantes, organizadores, avaliadores e administradores deverão confirmar que cada perfil acessa somente as operações e os dados autorizados. Chamadas diretas à API REST com perfil sem permissão deverão ser recusadas. |

### Justificativa

O requisito foi derivado do R02, classificado como crítico e priorizado em terceiro lugar na Etapa 2. Ocultar funcionalidades na interface não impede que um usuário tente acessar diretamente os endpoints da API REST. Por isso, a validação de autorização deve ocorrer no servidor, considerando tanto o perfil quanto o vínculo do usuário com o evento ou atividade solicitados.
