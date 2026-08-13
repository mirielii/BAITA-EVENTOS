# 21.3 Resultado esperado e referência — Controle de autorização

A prática de controle de autorização na API REST foi definida para reduzir o risco R02 — Execução de operações sem autorização e atender ao requisito RS03 — Autorização de operações protegidas.

## 21.3.1 Resultado esperado

Após a aplicação do controle proposto, a API REST deverá permitir operações protegidas somente quando o usuário:

- possuir uma sessão autenticada válida;
- tiver o perfil necessário para a operação;
- estiver vinculado ao evento ou à atividade relacionados à solicitação.

Chamadas diretas à API REST realizadas por usuários sem permissão deverão ser recusadas antes de qualquer alteração ou consulta indevida. A recusa deverá utilizar uma resposta adequada, como `403 — Permissão insuficiente`, e registrar o evento de forma auditável, sem expor tokens, credenciais ou dados pessoais desnecessários.

Com isso, espera-se reduzir a possibilidade de acesso indevido a eventos, atividades, avaliações, inscrições e funcionalidades administrativas.

## 21.3.2 Referência OWASP utilizada

A prática foi baseada na [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html).

A referência orienta que as permissões sejam validadas em todas as requisições e que a aplicação utilize uma postura de negação por padrão. Também recomenda criar testes unitários e de integração para verificar a lógica de autorização.

No contexto do BAITA Eventos, essas recomendações são aplicadas pela validação da sessão, do perfil do usuário e de seu vínculo com o evento ou atividade antes da execução de cada operação protegida na API REST.
