## 21.2 Implementação da prática 2 — Proteção de sessões autenticadas

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