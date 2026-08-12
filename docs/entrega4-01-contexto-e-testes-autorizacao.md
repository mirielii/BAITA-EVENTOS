# 21. Práticas de código seguro

Foram selecionadas duas práticas de código seguro derivadas dos riscos e requisitos definidos nas etapas anteriores. A primeira prática trata da validação de autorização na API REST; a segunda trata da proteção de sessões autenticadas.

A definição dessas práticas busca transformar os requisitos de segurança em comportamentos concretos que possam ser incorporados ao desenvolvimento da plataforma. Dessa forma, os controles de segurança deixam de ser apenas especificações documentais e passam a ser associados a testes e critérios verificáveis durante a implementação.

## 21.1 Testes antes da implementação

Os testes foram definidos antes da implementação para orientar o comportamento seguro esperado da API REST. A prática a seguir está relacionada ao risco R02 e ao requisito RS03.

A definição antecipada dos testes permite estabelecer previamente quais situações devem ser permitidas e quais devem ser bloqueadas pela aplicação. Isso reduz a possibilidade de que a implementação considere apenas o fluxo normal de utilização e deixe de tratar cenários em que um usuário autenticado tenta acessar ou modificar recursos que não estão sob sua responsabilidade.

### 21.1.1 Prática 1 — Controle de autorização na API REST

| Item                     | Descrição |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Prática de código seguro | Controle de autorização no servidor para operações protegidas da API REST. |
| Risco relacionado        | R02 — Execução de operações sem autorização. |
| Requisito relacionado    | RS03 — Autorização de operações protegidas. |
| Objetivo                 | Garantir que a API REST permita uma operação somente quando o usuário estiver autenticado, possuir o perfil necessário e tiver vínculo com o evento ou atividade envolvidos. |

| Teste                          | Entrada ou ação                                                                                                    | Resultado seguro esperado |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| TS01 — Operação autorizada     | Um organizador autenticado, vinculado ao evento, solicita a atualização de uma atividade desse evento.             | A solicitação é permitida, a atividade é atualizada e a operação é registrada nos logs de auditoria. |
| TS02 — Operação não autorizada | Um organizador autenticado tenta atualizar uma atividade pertencente a outro evento com o qual não possui vínculo. | A solicitação é recusada com resposta `403 — Permissão insuficiente`; a atividade não é alterada e a tentativa é registrada nos logs de auditoria. |

Os testes demonstram que não é suficiente ocultar opções na interface web. A API REST deve validar as permissões em todas as requisições protegidas, inclusive quando uma chamada é feita diretamente a um endpoint.

O primeiro teste verifica o comportamento esperado para uma operação legítima, garantindo que um usuário que atende às condições de autenticação, perfil e vínculo com o recurso consiga realizar a ação. Já o segundo teste representa uma tentativa de acesso indevido, verificando se a API impede a operação mesmo quando o usuário possui uma conta válida e está autenticado.

Essa separação é importante porque autenticação e autorização são controles diferentes. O fato de um usuário estar autenticado não significa que ele tenha permissão para acessar ou modificar qualquer recurso da plataforma. A autorização deve considerar também o perfil do usuário e seu vínculo com o evento ou atividade envolvidos.

A implementação ou o pseudocódigo dessa prática será apresentado na sequência, após a definição dos testes. A implementação deverá atender aos comportamentos definidos nos testes, garantindo que as operações autorizadas sejam permitidas e que as tentativas sem autorização sejam bloqueadas e registradas adequadamente.