## 9.5 Ordem inicial de implementação

A ordem inicial de implementação foi definida considerando a classificação dos riscos, a importância dos componentes afetados, as dependências técnicas, o custo, o impacto esperado e a facilidade de implementação.

Alguns riscos demandam mais de uma etapa. No caso do R09, as medidas iniciais de monitoramento, limitação de requisições e cópias de segurança devem ser implementadas cedo, enquanto mecanismos mais complexos de redundância podem ser adotados posteriormente.

| Ordem | Ação inicial | Riscos tratados | Justificativa |
| ------ | ------------ | --------------- | ------------- |
| 1 | Implantar infraestrutura básica de auditoria e monitoramento | R01, R02, R03, R04, R06 e R09 | A infraestrutura de auditoria e monitoramento fornece visibilidade sobre autenticações, operações administrativas, avaliações, inscrições e falhas. Também produz evidências para a detecção e a resposta aos incidentes. |
| 2 | Aplicar limitação de requisições, cópias de segurança e testes de restauração | R04 e R09 | O R09 possui a maior prioridade da análise. A limitação de requisições e as cópias de segurança são medidas iniciais importantes para reduzir a indisponibilidade e apoiar a recuperação. |
| 3 | Fortalecer autenticação, sessões e controle de acesso | R01, R02, R03 e R10 | Esses controles protegem contas autenticadas e operações privilegiadas. Também servem como base para outras medidas aplicadas à página autenticada e à API REST. |
| 4 | Implementar validação de entradas e proteção dos endpoints da API REST | R05 e R06 | A API REST é um componente central. A validação no servidor reduz a inserção de conteúdo malicioso e a execução de operações indevidas. |
| 5 | Implementar proteção contra CSRF e revisar os cookies de sessão | R01 e R10 | São medidas de aplicação direta que reduzem requisições forjadas e fortalecem a proteção das sessões autenticadas. |
| 6 | Reforçar inscrições e registros de presença | R04 | As funcionalidades públicas possuem elevada exposição. Validação, confirmação, limitação e detecção de duplicidades reduzem fraudes e automação. |
| 7 | Proteger a comunicação entre a API REST e o MongoDB | R07 | A medida depende da configuração da aplicação, do banco e da infraestrutura. Sua implementação protege os dados armazenados e as comunicações internas. |
| 8 | Revisar a integração com o serviço de e-mail | R08 | A ação envolve controles internos e requisitos do provedor, incluindo proteção dos tokens, autenticação do domínio e monitoramento das entregas. |
| 9 | Ampliar mecanismos de disponibilidade e continuidade | R09 | Após os controles iniciais, devem ser avaliadas medidas de maior custo ou complexidade, como redundância, serviços gerenciados e procedimentos completos de continuidade. |
| 10 | Executar revisão integrada e testes de segurança | Todos | A revisão final verifica a interação entre os controles e identifica falhas que não seriam percebidas em testes isolados. |

### 9.5.1 Justificativa da sequência

O R09 recebeu medidas tanto no início quanto nas etapas posteriores. Essa divisão ocorre porque determinados controles, como monitoramento, limitação de requisições e cópias de segurança, podem ser aplicados inicialmente, enquanto redundância e continuidade exigem maior planejamento e investimento.

Os controles de autenticação, autorização e sessão foram posicionados antes das medidas específicas de algumas funcionalidades porque servem como base para a proteção das operações autenticadas. Em seguida, foram priorizadas a validação da API REST, a proteção contra CSRF e as funcionalidades públicas.

Os controles que dependem de infraestrutura ou de fornecedores externos aparecem posteriormente por exigirem coordenação adicional. A sequência não impede a realização paralela de atividades quando houver responsáveis e recursos disponíveis.

## 9.6 Estimativa do risco residual

O risco residual representa o nível esperado após a implementação dos controles propostos. Como os controles ainda não foram implementados nem testados, os valores desta seção são estimativas e não representam uma comprovação de eficácia.

O impacto foi mantido em nível elevado nos cenários em que os controles atuam principalmente sobre a probabilidade. Ele somente foi reduzido quando as medidas propostas também favorecem a contenção, a continuidade ou a recuperação.

| Risco | Nível inicial | Probabilidade residual | Impacto residual | Pontuação residual | Nível residual esperado | Condição para aceitação |
| ----- | ------------- | ---------------------- | ----------------- | ------------------- | ----------------------- | ----------------------- |
| R01 | Crítico — 12 | 2 | 4 | 8 | Alto | Os controles de autenticação e sessão devem ser testados, as tentativas de acesso devem ser monitoradas e deve existir procedimento para bloquear contas comprometidas. |
| R02 | Crítico — 12 | 1 | 4 | 4 | Médio | Todas as operações protegidas devem validar permissões no servidor, e os testes não devem permitir acesso por perfis sem autorização. |
| R03 | Alto — 9 | 1 | 3 | 3 | Baixo | As alterações devem possuir registro de auditoria, identificação do responsável e possibilidade de restauração das informações legítimas. |
| R04 | Crítico — 12 | 2 | 3 | 6 | Médio | Duplicidades e comportamentos automatizados devem ser limitados e monitorados, com possibilidade de correção dos registros fraudulentos. |
| R05 | Crítico — 12 | 1 | 4 | 4 | Médio | Os campos devem passar por validação e sanitização, e os testes não devem resultar na execução de conteúdo malicioso. |
| R06 | Alto — 8 | 1 | 4 | 4 | Médio | Os endpoints devem validar autenticação, autorização e entradas no servidor, com testes de operações inválidas e não autorizadas. |
| R07 | Alto — 8 | 1 | 4 | 4 | Médio | A conexão deve estar protegida, restrita aos componentes necessários e utilizar credenciais com permissões mínimas. |
| R08 | Alto — 8 | 1 | 4 | 4 | Médio | O provedor e a integração devem atender aos requisitos definidos, e os tokens devem possuir validade limitada e uso controlado. |
| R09 | Crítico — 16 | 2 | 3 | 6 | Médio | Devem existir monitoramento, limitação de requisições, cópias de segurança testadas e procedimentos capazes de restaurar os serviços dentro do período definido. |
| R10 | Alto — 8 | 1 | 4 | 4 | Médio | As operações autenticadas que alteram dados devem possuir proteção contra CSRF, e os testes de requisições forjadas não devem ser bem-sucedidos. |

### 9.6.1 Condições gerais de aceitação

A aceitação do risco residual depende da implementação e da verificação dos controles propostos. Um risco não deve ser considerado aceitável apenas porque sua pontuação estimada foi reduzida.

Antes da aceitação, devem ser observadas as seguintes condições:

- os controles previstos devem estar implementados;
- as evidências de verificação devem estar disponíveis;
- os testes devem confirmar o funcionamento esperado;
- falhas relevantes devem ser corrigidas;
- os responsáveis devem conhecer as ações necessárias em caso de incidente;
- os riscos devem permanecer sujeitos a monitoramento e revisão.

Se as evidências demonstrarem que os controles não produziram a redução esperada, a probabilidade, o impacto e a estratégia de tratamento deverão ser reavaliados.

## 9.7 Considerações finais

A análise demonstrou que os riscos mais relevantes da plataforma BAITA Eventos estão relacionados à indisponibilidade dos serviços, ao comprometimento de contas, à execução de operações sem autorização, à inserção de conteúdo malicioso e às fraudes em funcionalidades públicas.

A priorização não foi determinada apenas pela pontuação. Também foram considerados os ativos afetados, a quantidade de usuários expostos, as dependências entre os componentes, a possibilidade de recuperação e a capacidade de um risco facilitar outros eventos.

A estratégia predominante foi **Reduzir**, pois a maior parte das funcionalidades associadas aos riscos é necessária para o funcionamento da plataforma. O R08 recebeu a estratégia **Compartilhar** devido à participação do provedor de e-mail, sem eliminar a responsabilidade da plataforma pelos controles internos. No R09, serviços terceirizados podem apoiar a disponibilidade, mas a estratégia principal permanece sendo a redução.

No mapeamento para o NIST CSF 2.0, as funções **Identify**, **Protect**, **Detect** e **Respond** foram as mais recorrentes. A função **Govern** foi relacionada aos riscos que exigem políticas, definição de responsabilidades, gestão de fornecedores ou requisitos de continuidade. A função **Recover** foi utilizada somente nos cenários em que existe necessidade de restaurar dados ou serviços.

Entre os controles considerados essenciais estão:

- proteção de contas e sessões;
- validação de autorização na API REST;
- registros de auditoria e monitoramento;
- validação e sanitização das entradas;
- proteção das funcionalidades públicas;
- segurança da comunicação com o MongoDB;
- proteção dos tokens enviados por e-mail;
- limitação de requisições;
- cópias de segurança e testes de restauração;
- proteção contra CSRF.

Uma das principais dificuldades encontradas foi diferenciar ameaças, casos de abuso, vulnerabilidades e eventos de risco, mantendo a rastreabilidade entre esses elementos. Também foi necessário justificar prioridades que não seguem estritamente a ordem numérica das pontuações e estimar níveis residuais sem possuir evidências de controles já implementados.

A principal limitação desta análise é o caráter estimado dos resultados residuais. Os controles propostos ainda precisam ser implementados e testados para que sua eficácia seja comprovada. Mudanças na arquitetura, nas funcionalidades, na infraestrutura ou nos serviços externos também podem alterar a probabilidade e o impacto dos riscos.

Como próximos passos, recomenda-se implementar os controles seguindo a ordem inicial estabelecida, produzir as evidências de verificação, executar testes de segurança e revisar periodicamente o registro de riscos. Após essas atividades, os níveis residuais deverão ser recalculados com base nos resultados observados.

A metodologia adotada permitiu manter a rastreabilidade entre a arquitetura, a modelagem STRIDE, os casos de abuso, os riscos identificados e o plano de tratamento, contribuindo para a consistência da análise.
