# Decisões de arquitetura — Plataforma BAITA Eventos

As decisões de arquitetura foram definidas a partir dos riscos prioritários identificados na Etapa 2 e dos requisitos de segurança correspondentes (RS01, RS02). Elas descrevem como a arquitetura da plataforma BAITA Eventos deve ser organizada para reduzir esses riscos, mantendo rastreabilidade entre risco, requisito, decisão e controles.

## Decisão 1 — Utilizar autenticação federada com Google

| Elemento | Descrição |
|---|---|
| Problema ou risco tratado | R01 — Comprometimento de contas autenticadas |
| Decisão tomada | Utilizar autenticação federada com Google por meio de OpenID Connect (OIDC) para organizadores, avaliadores e administradores. A plataforma valida os tokens recebidos, cria uma sessão local protegida e associa a conta autenticada ao perfil interno correspondente. |
| Motivo | A autenticação é delegada a um provedor consolidado, reduzindo a necessidade de a plataforma armazenar senhas próprias. A autenticação federada também permite aplicar controles locais de sessão, auditoria e bloqueio de acessos suspeitos. |
| Componentes afetados | Interface web, serviço Google Identity, serviço de autenticação, API REST, banco de dados e logs de auditoria. |
| Resultado esperado | Redução da exposição relacionada ao armazenamento de senhas, autenticação mais confiável e possibilidade de controlar sessões locais, validar tokens e registrar acessos suspeitos. |

A adoção do Google não substitui os controles internos: a API REST continua responsável por validar a identidade autenticada, controlar a sessão local e aplicar as regras de autorização da plataforma. Participantes continuam utilizando a página pública e realizando inscrições sem conta, sem passar por esse fluxo.

## Decisão 2 — Validar autorização na API REST

| Elemento | Descrição |
|---|---|
| Problema ou risco tratado | R02 — Execução de operações sem autorização |
| Decisão tomada | Validar, na API REST e antes de cada operação protegida, a autenticação do usuário, seu perfil e seu vínculo com o evento ou atividade relacionados à solicitação. |
| Motivo | Ocultar opções na interface não impede chamadas diretas aos endpoints. A autorização precisa ser aplicada no servidor para garantir que participantes, organizadores, avaliadores e administradores acessem somente as operações permitidas. |
| Componentes afetados | API REST, regras de autorização, dados de usuários, eventos, atividades e avaliações. |
| Resultado esperado | Recusa de solicitações realizadas por perfis sem permissão, proteção contra elevação de privilégios e preservação da separação entre as responsabilidades dos usuários. |

## Decisão 3 — Implementar controles de disponibilidade e recuperação

| Elemento | Descrição |
|---|---|
| Problema ou risco tratado | R09 — Indisponibilidade da plataforma |
| Decisão tomada | Implementar limitação de requisições na API REST, monitoramento de disponibilidade e consumo de recursos, registros de eventos, cópias de segurança do MongoDB e procedimentos testados de restauração. Mecanismos de redundância devem ser adotados quando viáveis para os componentes e serviços essenciais. |
| Motivo | A API REST e o MongoDB sustentam inscrições, check-ins, avaliações e operações administrativas. Uma sobrecarga, falha técnica ou indisponibilidade desses componentes pode interromper várias funcionalidades ao mesmo tempo. |
| Componentes afetados | API REST, MongoDB, serviço de logs e monitoramento, infraestrutura de cópias de segurança e serviços externos relevantes (como o serviço de e-mail). |
| Resultado esperado | Redução da probabilidade de indisponibilidade, identificação mais rápida de falhas, preservação dos dados e capacidade de restaurar os serviços essenciais após um incidente. |

## Rastreabilidade

| Risco prioritário | Requisito de segurança | Decisão de arquitetura | Controles principais |
|---|---|---|---|
| R01 — Comprometimento de contas autenticadas | RS02 — Proteção de contas autenticadas | Decisão 1 — Autenticação federada com Google | OIDC, validação de tokens, sessões locais protegidas, expiração e invalidação de sessões. |
| R02 — Execução de operações sem autorização | — | Decisão 2 — Validar autorização na API REST | Validação de autenticação, perfil e vínculo com o evento antes de cada operação protegida. |
| R09 — Indisponibilidade da plataforma | RS01 — Disponibilidade da plataforma | Decisão 3 — Controles de disponibilidade e recuperação | Limitação de requisições, monitoramento, logs, cópias de segurança, restauração e redundância quando viável. |

## Síntese
As três decisões cobrem, em conjunto, as três frentes de risco priorizadas na Etapa 2: quem pode entrar no sistema (Decisão 1), o que cada perfil pode fazer depois de autenticado (Decisão 2) e se o sistema continua disponível e recuperável sob falha ou ataque (Decisão 3). Nenhuma decisão substitui as demais — a autenticação federada não dispensa a validação de autorização na API, e os controles de disponibilidade não dependem da identidade do usuário. Elas atuam em camadas complementares da arquitetura.