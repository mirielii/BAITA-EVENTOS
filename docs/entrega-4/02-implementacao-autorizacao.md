# 21.2 Implementação da prática 1 — Controle de autorização na API REST

A implementação abaixo apresenta, em pseudocódigo, como a API REST pode validar a autorização antes de atualizar uma atividade. A solução é baseada nos testes TS01 e TS02 definidos anteriormente.

```text
FUNÇÃO atualizarAtividade(solicitacao):

    tokenSessao ← obterTokenDaSessao(solicitacao)

    SE tokenSessao não for válido:
        registrarLog("Acesso negado: sessão inválida")
        retornar resposta 401 "Autenticação necessária"

    usuario ← obterUsuarioDaSessao(tokenSessao)
    atividadeId ← solicitacao.parametro("atividadeId")
    dadosAtualizacao ← solicitacao.corpo

    atividade ← buscarAtividade(atividadeId)

    SE atividade não existir:
        retornar resposta 404 "Atividade não encontrada"

    SE usuario.perfil não for "organizador":
        registrarLog("Acesso negado: perfil sem permissão", usuario.id, atividadeId)
        retornar resposta 403 "Permissão insuficiente"

    SE usuario não estiver vinculado ao evento da atividade:
        registrarLog("Acesso negado: organizador não vinculado ao evento", usuario.id, atividadeId)
        retornar resposta 403 "Permissão insuficiente"

    validarDadosDaAtividade(dadosAtualizacao)

    atualizarAtividadeNoBanco(atividadeId, dadosAtualizacao)

    registrarLog("Atividade atualizada", usuario.id, atividadeId)

    retornar resposta 200 "Atividade atualizada com sucesso"
```

A validação é executada no servidor, antes da alteração dos dados. O controle considera três condições:

- a sessão deve ser válida;
- o usuário deve possuir perfil de organizador;
- o organizador deve estar vinculado ao evento ao qual a atividade pertence.

Dessa forma, uma solicitação direta à API REST não será aceita somente porque o usuário está autenticado. A operação também dependerá das permissões e do vínculo com o recurso solicitado.
