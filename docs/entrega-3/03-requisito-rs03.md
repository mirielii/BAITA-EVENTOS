# 18.3 RS03 — Autorização de operações protegidas

| ID | Risco de origem | Requisito de segurança | Critério de verificação |
|---|---|---|---|
| RS03 | R02 — Execução de operações sem autorização | A API REST deverá validar, no servidor e antes de cada operação protegida, a autenticação, o perfil do usuário e seu vínculo com o evento ou atividade envolvidos na solicitação. | Testes com participantes, organizadores, avaliadores e administradores deverão confirmar que cada perfil acessa somente as operações e os dados autorizados. Chamadas diretas à API REST com perfil sem permissão deverão ser recusadas. |

### Justificativa

O requisito foi derivado do R02, classificado como crítico e priorizado em terceiro lugar na Etapa 2. Ocultar funcionalidades na interface não impede que um usuário tente acessar diretamente os endpoints da API REST. Por isso, a validação de autorização deve ocorrer no servidor, considerando tanto o perfil quanto o vínculo do usuário com o evento ou atividade solicitados.