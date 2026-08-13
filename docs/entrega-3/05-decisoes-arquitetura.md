# 18.4 Decisões de arquitetura

As decisões de arquitetura foram definidas a partir dos riscos prioritários e dos requisitos de segurança RS01, RS02 e RS03. Elas descrevem como a arquitetura da plataforma BAITA Eventos deverá ser organizada para reduzir os riscos identificados.

## 18.4.1 Decisão 1 — Utilizar autenticação federada com Google

| Elemento | Descrição |
|---|---|
| Problema ou risco tratado | R01 — Comprometimento de contas autenticadas |
| Decisão tomada | Utilizar autenticação federada com Google por meio de OpenID Connect (OIDC) para organizadores, avaliadores e administradores. A plataforma deverá validar os tokens recebidos, criar uma sessão local protegida e associar a conta autenticada ao perfil interno correspondente. |
| Motivo | A autenticação será delegada a um provedor consolidado, reduzindo a necessidade de a plataforma armazenar senhas próprias. A autenticação federada também permite aplicar controles locais de sessão, auditoria e bloqueio de acessos suspeitos. |
| Componentes afetados | Interface web, serviço Google Identity, serviço de autenticação, API REST, banco de dados e logs de auditoria. |
| Resultado esperado | Redução da exposição relacionada ao armazenamento de senhas, autenticação mais confiável e possibilidade de controlar sessões locais, validar tokens e registrar acessos suspeitos. |

A adoção do Google não substitui os controles internos. A API REST continuará responsável por validar a identidade autenticada, controlar a sessão local e aplicar as regras de autorização da plataforma. Participantes continuarão utilizando a página pública e realizando inscrições sem conta.

## 18.4.2 Decisão 2 — Validar autorização na API REST

| Elemento | Descrição |
|---|---|
| Problema ou risco tratado | R02 — Execução de operações sem autorização |
| Decisão tomada | Validar, na API REST e antes de cada operação protegida, a autenticação do usuário, seu perfil e seu vínculo com o evento ou atividade relacionados à solicitação. |
| Motivo | Ocultar opções na interface não impede chamadas diretas aos endpoints. A autorização precisa ser aplicada no servidor para garantir que participantes, organizadores, avaliadores e administradores acessem somente as operações permitidas. |
| Componentes afetados | API REST, regras de autorização, dados de usuários, eventos, atividades e avaliações. |
| Resultado esperado | Recusa de solicitações realizadas por perfis sem permissão, proteção contra elevação de privilégios e preservação da separação entre as responsabilidades dos usuários. |