# Prática 1 — Controle de autorização na API REST

## Implementação em pseudocódigo

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

## Resultado esperado

A prática de controle de autorização na API REST foi definida para reduzir o risco R02 — Execução de operações sem autorização e atender ao requisito RS03 — Autorização de operações protegidas.

Após a aplicação do controle proposto, a API REST deverá permitir operações protegidas somente quando o usuário:

- possuir uma sessão autenticada válida;
- tiver o perfil necessário para a operação;
- estiver vinculado ao evento ou à atividade relacionados à solicitação.

Chamadas diretas à API REST realizadas por usuários sem permissão deverão ser recusadas antes de qualquer alteração ou consulta indevida. A recusa deverá utilizar uma resposta adequada, como `403 — Permissão insuficiente`, e registrar o evento de forma auditável, sem expor tokens, credenciais ou dados pessoais desnecessários.

Com isso, espera-se reduzir a possibilidade de acesso indevido a eventos, atividades, avaliações, inscrições e funcionalidades administrativas.

## Referência OWASP utilizada

A prática foi baseada na [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html).

A referência orienta que as permissões sejam validadas em todas as requisições e que a aplicação utilize uma postura de negação por padrão. Também recomenda criar testes unitários e de integração para verificar a lógica de autorização.

No contexto do BAITA Eventos, essas recomendações são aplicadas pela validação da sessão, do perfil do usuário e de seu vínculo com o evento ou atividade antes da execução de cada operação protegida na API REST.
