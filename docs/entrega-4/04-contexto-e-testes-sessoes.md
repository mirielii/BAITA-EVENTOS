# 21.1 Testes antes da implementação

## 21.1.2 Prática 2 — Proteção de sessões autenticadas

| Item | Descrição |
|---|---|
| Prática de código seguro | Validação de tokens Google/OIDC e criação de sessões locais protegidas. |
| Risco relacionado | R01 — Comprometimento de contas autenticadas. |
| Requisito relacionado | RS02 — Proteção de contas autenticadas. |
| Objetivo | Garantir que somente usuários internos com token de identidade válido possam criar sessões locais e acessar operações protegidas da plataforma. |

| Teste | Entrada ou ação | Resultado seguro esperado |
|---|---|---|
| TS03 — Autenticação válida | Um organizador utiliza um token Google/OIDC válido, não expirado e emitido para a plataforma BAITA Eventos. | A identidade é validada, uma sessão local protegida é criada e o acesso às funcionalidades permitidas para o perfil é autorizado. O evento de autenticação é registrado. |
| TS04 — Token inválido ou expirado | Um usuário tenta criar uma sessão local utilizando token expirado, com assinatura inválida ou emitido para outra aplicação. | A autenticação é recusada com resposta `401 — Token inválido ou expirado`; nenhuma sessão local é criada e a tentativa é registrada sem armazenar o token nos logs. |

Os testes foram definidos antes da implementação para assegurar que a plataforma aceite somente tokens confiáveis e rejeite credenciais inválidas antes de criar uma sessão local.