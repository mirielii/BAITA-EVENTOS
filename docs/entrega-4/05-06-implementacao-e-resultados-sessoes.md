# 21.3 Prática 2 — Proteção de sessões autenticadas

## 21.3.1 Implementação em pseudocódigo

A implementação abaixo apresenta, em pseudocódigo, como a plataforma pode validar o token fornecido pelo Google e criar uma sessão local protegida. A solução é baseada nos testes TS03 e TS04 definidos anteriormente.

```text
FUNÇÃO autenticarComGoogle(tokenGoogle):

    dadosToken ← validarTokenComGoogle(tokenGoogle)

    SE dadosToken.assinatura não for válida:
        registrarLog("Autenticação recusada: assinatura inválida")
        retornar resposta 401 "Token inválido"

    SE dadosToken.emissor não for Google:
        registrarLog("Autenticação recusada: emissor inválido")
        retornar resposta 401 "Token inválido"

    SE dadosToken.destinatario não for o identificador da plataforma:
        registrarLog("Autenticação recusada: token destinado a outra aplicação")
        retornar resposta 401 "Token inválido"

    SE dadosToken estiver expirado:
        registrarLog("Autenticação recusada: token expirado")
        retornar resposta 401 "Token expirado"

    usuario ← buscarUsuarioPorEmail(dadosToken.email)

    SE usuario não existir OU usuario.perfil não for autorizado:
        registrarLog("Autenticação recusada: usuário sem perfil interno autorizado")
        retornar resposta 403 "Acesso não autorizado"

    sessao ← criarSessaoLocal(
        usuario.id,
        expiração,
        identificadorSeguroAleatório
    )

    armazenarSessaoNoServidor(sessao)

    configurarCookie(
        nome = "sessao",
        valor = sessao.identificador,
        HttpOnly = verdadeiro,
        Secure = verdadeiro,
        SameSite = "Lax"
    )

    registrarLog("Autenticação realizada", usuario.id)

    retornar resposta 200 "Autenticação realizada com sucesso"
```

A plataforma não deve utilizar diretamente o token Google como sessão da aplicação. Primeiro, ela valida o token de identidade e confirma que ele foi emitido pelo Google para o BAITA Eventos. Depois, cria uma sessão local com identificador aleatório, expiração definida e cookie protegido.

A sessão local poderá ser invalidada quando houver indício de comprometimento, bloqueio da conta ou encerramento explícito da sessão.

## 21.3.2 Resultado esperado

A prática de proteção de sessões autenticadas foi definida para reduzir o risco R01 — Comprometimento de contas autenticadas e atender ao requisito RS02 — Proteção de contas autenticadas.

Após a aplicação do controle proposto, a plataforma deverá criar sessões locais somente para organizadores, avaliadores e administradores que apresentem token Google/OIDC válido.

O token deverá possuir assinatura válida, emissor confiável, destinatário correspondente ao BAITA Eventos e prazo de validade vigente. Tokens expirados, adulterados ou emitidos para outra aplicação deverão ser recusados antes da criação da sessão.

A sessão local deverá possuir identificador aleatório, prazo de expiração e proteção por meio de cookie configurado com os atributos `HttpOnly`, `Secure` e `SameSite`. O identificador da sessão não deverá ser registrado em logs. Quando houver encerramento da sessão, bloqueio da conta ou indício de comprometimento, a sessão local deverá ser invalidada.

Com isso, espera-se reduzir o risco de uso indevido de sessões e impedir que tokens inválidos ou expirados forneçam acesso às funcionalidades autenticadas da plataforma.

## 21.3.3 Referência OWASP utilizada

A prática foi baseada na [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html).

A referência orienta que identificadores de sessão sejam protegidos, possuam expiração adequada e sejam transmitidos somente por conexões seguras. Também recomenda o uso de atributos de cookie como `Secure`, `HttpOnly` e `SameSite` para reduzir exposição e uso indevido de sessões.

No contexto do BAITA Eventos, essas recomendações são aplicadas pela criação de uma sessão local somente após a validação do token Google/OIDC, pela utilização de cookies protegidos, pela expiração das sessões inativas e pela possibilidade de invalidação em caso de suspeita de comprometimento.
