## 21.3 Resultado esperado e referência — Proteção de sessões autenticadas

A prática de proteção de sessões autenticadas foi definida para reduzir o risco R01 — Comprometimento de contas autenticadas e atender ao requisito RS02 — Proteção de contas autenticadas.

### 21.3.1 Resultado esperado

Após a aplicação do controle proposto, a plataforma deverá criar sessões locais somente para organizadores, avaliadores e administradores que apresentem token Google/OIDC válido.

O token deverá possuir assinatura válida, emissor confiável, destinatário correspondente ao BAITA Eventos e prazo de validade vigente. Tokens expirados, adulterados ou emitidos para outra aplicação deverão ser recusados antes da criação da sessão.

A sessão local deverá possuir identificador aleatório, prazo de expiração e proteção por meio de cookie configurado com os atributos `HttpOnly`, `Secure` e `SameSite`. O identificador da sessão não deverá ser registrado em logs. Quando houver encerramento da sessão, bloqueio da conta ou indício de comprometimento, a sessão local deverá ser invalidada.

Com isso, espera-se reduzir o risco de uso indevido de sessões e impedir que tokens inválidos ou expirados forneçam acesso às funcionalidades autenticadas da plataforma.

### 21.3.2 Referência OWASP utilizada

A prática foi baseada na [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html).

A referência orienta que identificadores de sessão sejam protegidos, possuam expiração adequada e sejam transmitidos somente por conexões seguras. Também recomenda o uso de atributos de cookie como `Secure`, `HttpOnly` e `SameSite` para reduzir exposição e uso indevido de sessões.

No contexto do BAITA Eventos, essas recomendações são aplicadas pela criação de uma sessão local somente após a validação do token Google/OIDC, pela utilização de cookies protegidos, pela expiração das sessões inativas e pela possibilidade de invalidação em caso de suspeita de comprometimento.