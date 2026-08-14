# Análise e priorização dos riscos

## Critérios de probabilidade

A probabilidade representa a possibilidade de um evento de risco ocorrer na plataforma BAITA Eventos, considerando as condições necessárias para sua exploração, a frequência de exposição e o nível de acesso exigido.

| Nível | Classificação | Critério contextualizado |
|---:|---|---|
| 1 | Baixa | O evento depende de condições incomuns, acesso muito específico ou elevada capacidade técnica. |
| 2 | Média-baixa | O evento é possível, mas depende de uma vulnerabilidade ou condição específica. |
| 3 | Média-alta | O evento é plausível em situações comuns de uso ou ataque à plataforma. |
| 4 | Alta | O evento pode ocorrer com facilidade, frequência ou em condições previsíveis. |

## Critérios de impacto

O impacto representa a gravidade das consequências para os usuários, para os dados, para as funcionalidades e para a administração da plataforma caso o evento de risco se concretize.

| Nível | Classificação | Critério contextualizado |
|---:|---|---|
| 1 | Baixo | Provoca pequeno inconveniente e pode ser corrigido rapidamente, sem prejuízo relevante. |
| 2 | Moderado | Provoca interrupção ou inconsistência limitada, com possibilidade de recuperação. |
| 3 | Alto | Causa prejuízo relevante aos usuários, ao negócio, à administração ou à privacidade. |
| 4 | Muito alto | Afeta muitos usuários, operações críticas ou provoca danos graves à plataforma e aos dados. |

##  Cálculo e classificação

A pontuação de cada risco é calculada pela multiplicação da probabilidade pelo impacto:

**Pontuação = Probabilidade × Impacto**

| Pontuação | Classificação |
|---:|---|
| 1 a 3 | Baixo |
| 4 a 7 | Médio |
| 8 a 11 | Alto |
| 12 a 16 | Crítico |

## Registro de riscos

Os riscos foram derivados dos casos de abuso. Como cada caso de abuso agrupa ameaças relacionadas, um único risco pode ter origem em mais de uma categoria STRIDE. Os identificadores das ameaças foram mantidos para garantir a rastreabilidade com a modelagem da Etapa 1.

| ID | Caso de abuso | Origem STRIDE | Evento de risco | Vulnerabilidade ou condição | P | I | Pontuação | Classificação |
|---|---|---|---|---|---:|---:|---:|---|
| R01 | CA01 | Spoofing e Repudiation — T10, T13, T16 e T54 | Um atacante compromete uma conta autenticada e passa a executar ações em nome de um usuário legítimo. | Falhas na autenticação, na proteção de credenciais ou no gerenciamento de sessão. | 3 | 4 | 12 | Crítico |
| R02 | CA02 | Repudiation, Information Disclosure e Elevation of Privilege — T12, T15, T18, T60, T66, T72 e T123 | Um usuário executa operações administrativas ou de gestão sem possuir autorização adequada. | Controles insuficientes de autorização e validação de permissões. | 3 | 4 | 12 | Crítico |
| R03 | CA03 | Spoofing, Tampering, Repudiation e Elevation of Privilege — T13, T15, T66 e T114 | Um usuário mal-intencionado altera notas, comentários, pareceres ou classificações de avaliações. | Falhas na autenticação, autorização e rastreabilidade das operações de avaliação. | 3 | 3 | 9 | Alto |
| R04 | CA04 | Repudiation e Denial of Service — T38, T40, T54 e T115 | Um atacante realiza inscrições ou registros de presença fraudulentos. | Ausência ou insuficiência de validações nas operações públicas de inscrição e check-in. | 4 | 3 | 12 | Crítico |
| R05 | CA05 | Spoofing e Tampering — T01, T10, T11, T13 e T16 | Um atacante insere conteúdo malicioso em campos processados ou apresentados pela plataforma. | Validação, sanitização ou codificação insuficiente das entradas e saídas. | 3 | 4 | 12 | Crítico |
| R06 | CA06 | Spoofing, Tampering e Elevation of Privilege — T57, T58, T112 e T114 | Um atacante explora a API REST para executar operações indevidas e comprometer regras de negócio. | Entradas processadas de forma insegura na API REST e validações insuficientes no servidor. | 2 | 4 | 8 | Alto |
| R07 | CA07 | Spoofing e Information Disclosure — T113, T116, T121 e T123 | Um atacante intercepta, altera ou redireciona dados trocados entre a API REST e o MongoDB. | Comunicação desprotegida entre a API REST e o MongoDB e validação insuficiente da conexão. | 2 | 4 | 8 | Alto |
| R08 | CA08 | Spoofing e Denial of Service — T98 e T100 | Um atacante redireciona, intercepta ou falsifica mensagens e tokens enviados por e-mail. | Confiança inadequada no serviço de e-mail e proteção insuficiente dos dados enviados. | 2 | 4 | 8 | Alto |
| R09 | CA09 | Denial of Service — T40, T56, T100, T117, T119 e T124 | Um atacante ou uma falha provoca a indisponibilidade de funcionalidades essenciais da plataforma. | Ausência de proteção adequada contra sobrecarga e dependência da API REST, do MongoDB e do serviço de e-mail. | 4 | 4 | 16 | Crítico |
| R10 | CA10 | Repudiation e Elevation of Privilege — T12, T18, T59 e T72 | Um atacante induz um usuário autenticado a executar uma operação não intencional por meio de CSRF. | Ausência ou insuficiência de mecanismos de proteção contra requisições forjadas. | 2 | 4 | 8 | Alto |

## Justificativas

As pontuações foram atribuídas considerando as condições necessárias para a ocorrência de cada evento, a exposição dos componentes, a abrangência dos efeitos e a possibilidade de recuperação. As justificativas a seguir complementam os valores do registro de riscos.

### R01 — Comprometimento de contas autenticadas

- **Probabilidade — 3 (média-alta):** tentativas de obtenção ou reutilização de credenciais são plausíveis em sistemas com autenticação. O risco pode atingir contas de organizadores, avaliadores e administradores.
- **Impacto — 4 (muito alto):** uma conta comprometida permite executar operações em nome do usuário legítimo, inclusive ações de gestão, avaliação ou administração.
- **Ativos afetados:** credenciais, sessões autenticadas, contas de usuários internos, eventos, avaliações e configurações administrativas.
- **Consequências:** acesso indevido, alteração de informações, exposição de dados, perda de rastreabilidade e uso da conta para explorar outros riscos.

### R02 — Execução de operações sem autorização

- **Probabilidade — 3 (média-alta):** falhas de autorização podem ser exploradas por usuários autenticados que tentem acessar funcionalidades ou dados destinados a outros perfis ou eventos.
- **Impacto — 4 (muito alto):** operações sem autorização podem afetar eventos, atividades, avaliações, usuários, permissões e solicitações administrativas.
- **Ativos afetados:** regras de autorização, perfis de acesso, dados dos eventos, avaliações, usuários e configurações administrativas.
- **Consequências:** alteração ou consulta indevida de informações, elevação de privilégios, perda de rastreabilidade e comprometimento da separação entre os perfis.

### R03 — Manipulação de avaliações

- **Probabilidade — 3 (média-alta):** o processo depende de contas autenticadas e de verificações corretas sobre quais atividades cada avaliador pode acessar. Falhas nessas verificações tornam o cenário plausível.
- **Impacto — 3 (alto):** a alteração de notas, comentários, pareceres ou classificações compromete a integridade e a credibilidade do processo de avaliação.
- **Ativos afetados:** avaliações, notas, comentários, pareceres, classificações e histórico das operações.
- **Consequências:** resultados incorretos, favorecimento ou prejuízo de participantes, contestação das avaliações e perda de confiança no evento.

### R04 — Fraudes em inscrições e registros de presença

- **Probabilidade — 4 (alta):** inscrições e check-ins são realizados por participantes sem conta e ficam expostos por meio da página pública, facilitando tentativas repetidas ou automatizadas.
- **Impacto — 3 (alto):** fraudes podem comprometer a confiabilidade das listas de inscritos e dos registros de presença, embora seus efeitos normalmente permaneçam concentrados no evento afetado.
- **Ativos afetados:** inscrições, registros de presença, vagas das atividades, dados dos participantes inscritos e relatórios do evento.
- **Consequências:** inscrições duplicadas ou falsas, ocupação indevida de vagas, registros incorretos de presença e necessidade de correção manual.

### R05 — Inserção de conteúdo malicioso

- **Probabilidade — 3 (média-alta):** a plataforma recebe dados por formulários públicos e autenticados. Caso as entradas e saídas não sejam tratadas adequadamente, o envio de conteúdo malicioso torna-se plausível.
- **Impacto — 4 (muito alto):** a execução de conteúdo malicioso pode comprometer páginas, sessões autenticadas e informações acessadas por diferentes usuários.
- **Ativos afetados:** página pública, área autenticada, sessões, dados apresentados e navegadores dos usuários.
- **Consequências:** execução de scripts, comprometimento de sessões, alteração da apresentação das páginas, exposição de informações e realização de ações em nome de usuários autenticados.

### R06 — Operações indevidas por meio da API REST

- **Probabilidade — 2 (média-baixa):** a exploração depende de entradas processadas de forma insegura na API REST ou de falhas específicas nas validações das regras de negócio.
- **Impacto — 4 (muito alto):** como a API REST concentra o processamento das requisições, sua exploração pode permitir operações não autorizadas e afetar diferentes funcionalidades.
- **Ativos afetados:** API REST, regras de negócio, endpoints, dados processados e informações armazenadas no MongoDB.
- **Consequências:** execução de código ou operações indevidas, alteração de informações, acesso não autorizado e comprometimento da aplicação.

### R07 — Comprometimento da comunicação entre a API REST e o MongoDB

- **Probabilidade — 2 (média-baixa):** o cenário exige acesso à comunicação interna ou capacidade de se apresentar como um dos componentes legítimos, o que depende de condições específicas.
- **Impacto — 4 (muito alto):** a interceptação ou falsificação da comunicação pode expor ou adulterar dados utilizados pelas principais funcionalidades da plataforma.
- **Ativos afetados:** comunicação entre a API REST e o MongoDB, credenciais de conexão, inscrições, avaliações, configurações e demais dados armazenados.
- **Consequências:** exposição de dados, respostas adulteradas, gravações indevidas e comprometimento da integridade das informações.

### R08 — Comprometimento de mensagens e tokens de e-mail

- **Probabilidade — 2 (média-baixa):** a ocorrência depende do comprometimento, da falsificação ou da indisponibilidade do serviço externo de e-mail ou de sua integração com a API REST.
- **Impacto — 4 (muito alto):** mensagens ou tokens redirecionados podem comprometer contas e informações sensíveis por meio do comprometimento do serviço de e-mail, enquanto a interrupção do serviço pode impedir comunicações necessárias.
- **Ativos afetados:** mensagens, convites, confirmações, notificações, tokens e integração com o serviço de e-mail.
- **Consequências:** exposição ou uso indevido de tokens, envio de mensagens a destinatários incorretos, comprometimento de contas e indisponibilidade das comunicações.

### R09 — Indisponibilidade da plataforma

- **Probabilidade — 4 (alta):** a API REST é o componente central das requisições, e a plataforma também depende do MongoDB e do serviço de e-mail. Sobrecarga ou falha nesses componentes pode interromper diferentes funcionalidades.
- **Impacto — 4 (muito alto):** a indisponibilidade pode afetar simultaneamente participantes, organizadores, avaliadores e administradores, impedindo operações essenciais durante os eventos.
- **Ativos afetados:** API REST, MongoDB, serviço de e-mail, página pública, área autenticada e funcionalidades de inscrição, check-in e avaliação.
- **Consequências:** interrupção das atividades, impossibilidade de realizar inscrições ou check-ins, atraso nas avaliações, falhas administrativas e perda de confiança na plataforma.

### R10 — Execução de operações por CSRF

- **Probabilidade — 2 (média-baixa):** a exploração exige que um usuário esteja autenticado e seja induzido a acessar conteúdo preparado pelo atacante, além da ausência de proteção adequada contra CSRF.
- **Impacto — 4 (muito alto):** uma requisição forjada pode executar operações com as permissões do usuário autenticado, inclusive ações de gestão ou administração.
- **Ativos afetados:** sessões autenticadas, operações de organizadores, avaliadores e administradores, eventos, avaliações e configurações.
- **Consequências:** alterações não intencionais, execução de operações privilegiadas, perda de rastreabilidade e comprometimento da integridade das informações.
