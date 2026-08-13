# 19. Vulnerabilidades catalogadas

Foram selecionadas três vulnerabilidades catalogadas, uma para cada risco prioritário e requisito de segurança definido. O objetivo do mapeamento é relacionar os riscos da plataforma BAITA Eventos a referências reconhecidas, orientando a definição dos controles arquiteturais.

| Risco | Vulnerabilidade ou categoria | Referência utilizada | Relação com o sistema |
|---|---|---|---|
| R09 — Indisponibilidade da plataforma | CWE-770 — Allocation of Resources Without Limits or Throttling | [CWE-770](https://cwe.mitre.org/data/definitions/770.html) | A ausência de limites ou controle de requisições pode permitir que chamadas excessivas consumam recursos da API REST, do MongoDB ou da infraestrutura, prejudicando inscrições, check-ins, avaliações e operações administrativas. |
| R01 — Comprometimento de contas autenticadas | CWE-287 — Improper Authentication | [CWE-287](https://cwe.mitre.org/data/definitions/287.html) | A validação insuficiente da identidade de um usuário ou de tokens de autenticação pode permitir que um atacante utilize uma conta de organizador, avaliador ou administrador. A autenticação federada com Google/OIDC, a validação de tokens e o controle de sessões reduzem esse risco. |
| R02 — Execução de operações sem autorização | CWE-862 — Missing Authorization | [CWE-862](https://cwe.mitre.org/data/definitions/862.html) | A ausência de verificações de autorização na API REST pode permitir que um usuário autenticado consulte ou execute operações fora de seu perfil ou vínculo com determinado evento ou atividade. |

## 19.1 Relação com os requisitos de segurança

O mapeamento reforça a ligação entre as vulnerabilidades catalogadas e os requisitos definidos:

- **CWE-770** está relacionada ao RS01, pois justifica a limitação de requisições e o monitoramento da disponibilidade.
- **CWE-287** está relacionada ao RS02, pois orienta a validação de tokens de identidade, a autenticação federada e o gerenciamento de sessões.
- **CWE-862** está relacionada ao RS03, pois reforça a necessidade de validar permissões na API REST antes de cada operação protegida.

Essas referências foram utilizadas como apoio à arquitetura proposta e não como indicação de que a plataforma possui vulnerabilidades comprovadas. Elas representam categorias que devem ser consideradas para reduzir os riscos prioritários identificados na Etapa 2.