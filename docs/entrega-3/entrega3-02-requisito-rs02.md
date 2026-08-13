## 18.2 RS02 — Proteção de contas autenticadas

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
|---|---|---|---|
| RS02 | R01 — Comprometimento de contas autenticadas | A plataforma deverá utilizar autenticação federada com Google por meio de OpenID Connect (OIDC) para organizadores, avaliadores e administradores. A aplicação deverá validar os tokens de identidade recebidos, criar sessões locais protegidas, expirar sessões inativas e invalidar sessões locais quando houver indício de comprometimento. | Testes deverão confirmar que somente tokens válidos, emitidos para a aplicação e não expirados permitem a criação de uma sessão. Sessões inativas deverão expirar e sessões locais deverão ser invalidadas quando a conta for bloqueada ou houver indício de comprometimento. |

### Justificativa

O requisito foi derivado do R01, classificado como crítico e priorizado em segundo lugar na Etapa 2. O comprometimento de contas pode permitir que um atacante atue em nome de organizadores, avaliadores ou administradores, comprometendo eventos, avaliações, permissões e configurações da plataforma.

A autenticação será delegada ao Google, reduzindo a necessidade de a plataforma BAITA Eventos armazenar senhas próprias. Entretanto, a plataforma continuará responsável por validar os tokens, proteger e controlar as sessões locais, associar a conta autenticada ao perfil interno adequado e registrar eventos de autenticação para auditoria.