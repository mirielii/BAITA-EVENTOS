# Diagrama da arquitetura segura

O diagrama a seguir apresenta a arquitetura segura proposta para a plataforma BAITA Eventos. Ele relaciona os usuários, a página pública, a API REST, a autenticação federada com Google/OIDC, as regras de autorização, o MongoDB, o serviço de e-mail, os registros de logs, o monitoramento e os controles de disponibilidade e recuperação.

![Diagrama da arquitetura segura da plataforma BAITA Eventos](../../diagramas/etapa-3/arquitetura-de-software-seguro.drawio.png)

*Figura 1 — Diagrama da arquitetura segura da plataforma BAITA Eventos.*

# Decisões de arquitetura

As decisões de arquitetura foram definidas a partir dos riscos prioritários e dos requisitos de segurança RS01, RS02 e RS03. Elas descrevem como a arquitetura da plataforma BAITA Eventos deverá ser organizada para reduzir os riscos identificados.

## Decisão 1 — Utilizar autenticação federada com Google

| Elemento | Descrição |
|---|---|
| Problema ou risco tratado | R01 — Comprometimento de contas autenticadas |
| Decisão tomada | Utilizar autenticação federada com Google por meio de OpenID Connect (OIDC) para organizadores, avaliadores e administradores. A plataforma deverá validar os tokens recebidos, criar uma sessão local protegida e associar a conta autenticada ao perfil interno correspondente. |
| Motivo | A autenticação será delegada a um provedor consolidado, reduzindo a necessidade de a plataforma armazenar senhas próprias. A autenticação federada também permite aplicar controles locais de sessão, auditoria e bloqueio de acessos suspeitos. |
| Componentes afetados | Interface web, serviço Google Identity, serviço de autenticação, API REST, banco de dados e logs de auditoria. |
| Resultado esperado | Redução da exposição relacionada ao armazenamento de senhas, autenticação mais confiável e possibilidade de controlar sessões locais, validar tokens e registrar acessos suspeitos. |

A adoção do Google não substitui os controles internos. A API REST continuará responsável por validar a identidade autenticada, controlar a sessão local e aplicar as regras de autorização da plataforma. Participantes continuarão utilizando a página pública e realizando inscrições sem conta.

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
| Decisão tomada | Implementar limitação de requisições na API REST, monitoramento de disponibilidade e consumo de recursos, registros de eventos, cópias de segurança do MongoDB e procedimentos testados de restauração. Mecanismos de redundância deverão ser adotados quando forem viáveis para os componentes e serviços essenciais. |
| Motivo | A API REST e o MongoDB sustentam inscrições, check-ins, avaliações e operações administrativas. Uma sobrecarga, falha técnica ou indisponibilidade desses componentes pode interromper várias funcionalidades ao mesmo tempo. |
| Componentes afetados | API REST, MongoDB, serviço de logs e monitoramento, infraestrutura de cópias de segurança e serviços externos relevantes. |
| Resultado esperado | Redução da probabilidade de indisponibilidade, identificação mais rápida de falhas, preservação dos dados e capacidade de restaurar os serviços essenciais após um incidente. |

# Rastreabilidade da arquitetura segura

A proposta da Etapa 3 mantém a rastreabilidade com as etapas anteriores. Os riscos prioritários da Etapa 2 foram utilizados como origem dos requisitos de segurança, das decisões de arquitetura e dos controles posicionados no diagrama da arquitetura segura.

| Risco prioritário | Requisito de segurança | Decisão de arquitetura | Controles principais |
|---|---|---|---|
| R09 — Indisponibilidade da plataforma | RS01 — Disponibilidade da plataforma | Decisão 3 — Implementar controles de disponibilidade e recuperação | Limitação de requisições, monitoramento, logs, cópias de segurança, restauração e redundância quando viável. |
| R01 — Comprometimento de contas autenticadas | RS02 — Proteção de contas autenticadas | Decisão 1 — Utilizar autenticação federada com Google | Autenticação federada com Google/OIDC, validação de tokens, sessões locais protegidas, expiração e invalidação de sessões e registros de autenticação. |
| R02 — Execução de operações sem autorização | RS03 — Autorização de operações protegidas | Decisão 2 — Validar autorização na API REST | Controle de acesso por perfil, validação no servidor e verificação do vínculo com evento ou atividade. |

Essa rastreabilidade demonstra que a arquitetura segura não foi definida de forma isolada. As decisões adotadas decorrem dos riscos prioritários identificados, dos requisitos de segurança derivados e dos controles propostos na Etapa 2.
