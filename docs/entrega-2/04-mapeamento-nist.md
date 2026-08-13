## 9.3 Mapeamento dos riscos para o NIST CSF 2.0

O mapeamento foi realizado de acordo com os resultados de segurança necessários para tratar cada risco. As funções não foram marcadas automaticamente: cada associação considera as características do evento, os componentes afetados e as medidas necessárias para prevenção, detecção, resposta ou recuperação.

Nem todos os riscos exigem restauração de dados ou serviços. Por isso, a função **Recover** foi associada apenas aos cenários em que essa necessidade foi identificada. Da mesma forma, a função **Govern** foi relacionada somente aos riscos que dependem diretamente de políticas, responsabilidades, requisitos institucionais ou gestão de fornecedores.

### 9.3.1 Tabela de mapeamento

| Risco | Govern | Identify | Protect | Detect | Respond | Recover |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| R01 | X | X | X | X | X |  |
| R02 | X | X | X | X | X |  |
| R03 |  | X | X | X | X | X |
| R04 |  | X | X | X | X |  |
| R05 |  | X | X | X | X | X |
| R06 | X | X | X | X | X |  |
| R07 |  | X | X | X | X | X |
| R08 | X | X | X | X | X |  |
| R09 | X | X | X | X | X | X |
| R10 |  | X | X | X | X |  |

### 9.3.2 Justificativas do mapeamento

#### R01 — Comprometimento de contas autenticadas

- **Govern:** definição de políticas de autenticação e responsabilidades sobre contas privilegiadas;
- **Identify:** identificação das contas, permissões e pontos de autenticação expostos;
- **Protect:** proteção de credenciais, sessões e tentativas de acesso;
- **Detect:** monitoramento de autenticações e comportamentos anormais;
- **Respond:** bloqueio de contas ou sessões comprometidas e investigação do incidente.

A função Recover não foi associada diretamente porque o tratamento principal está concentrado na prevenção, na detecção e na contenção do acesso indevido.

#### R02 — Execução de operações sem autorização

- **Govern:** definição de papéis e regras de autorização;
- **Identify:** identificação das operações e permissões associadas a cada perfil;
- **Protect:** validação das permissões no servidor;
- **Detect:** registro e monitoramento das operações executadas;
- **Respond:** revogação de acessos e contenção de operações indevidas.

A função Recover não foi associada porque a resposta principal consiste em interromper o acesso indevido e corrigir as permissões. Caso uma operação provoque alteração de dados, a recuperação deverá ser considerada no tratamento do evento específico.

#### R03 — Manipulação de avaliações

- **Identify:** identificação das avaliações, dos usuários autorizados e das operações sensíveis;
- **Protect:** controle de acesso e validação das alterações;
- **Detect:** registros de auditoria e identificação de mudanças anormais;
- **Respond:** interrupção do acesso indevido e investigação das alterações;
- **Recover:** restauração das notas, comentários, pareceres ou classificações legítimas.

A função Govern não foi marcada porque o tratamento está concentrado na proteção técnica e operacional do processo de avaliação, sem exigir uma decisão adicional de governança para este cenário.

#### R04 — Fraudes em inscrições e registros de presença

- **Identify:** identificação das funcionalidades públicas e dos padrões esperados de utilização;
- **Protect:** validação das solicitações e adoção de mecanismos contra automação;
- **Detect:** monitoramento de inscrições e check-ins repetidos ou anormais;
- **Respond:** bloqueio das solicitações fraudulentas e correção dos registros afetados.

A função Recover não foi associada diretamente porque os registros fraudulentos podem ser tratados durante a resposta, por meio de sua identificação e correção. A função Govern também não foi considerada essencial para o tratamento inicial.

#### R05 — Inserção de conteúdo malicioso

- **Identify:** identificação dos campos de entrada e dos locais em que o conteúdo é apresentado;
- **Protect:** validação, sanitização e codificação segura das entradas e saídas;
- **Detect:** registro e detecção de entradas ou comportamentos suspeitos;
- **Respond:** remoção do conteúdo malicioso e contenção de sua origem;
- **Recover:** restauração das informações, páginas ou sessões afetadas.

A função Recover foi incluída porque a exploração pode alterar conteúdo apresentado pela plataforma ou comprometer sessões e informações, exigindo restauração após a contenção.

#### R06 — Operações indevidas por meio da API REST

- **Govern:** definição de requisitos de segurança para o desenvolvimento e a manutenção da API REST;
- **Identify:** identificação dos endpoints, entradas e regras de negócio críticas;
- **Protect:** aplicação de autenticação, autorização e validação no servidor;
- **Detect:** monitoramento de chamadas, erros e respostas anormais;
- **Respond:** bloqueio das requisições, correção da vulnerabilidade e análise dos registros.

A função Recover não foi associada diretamente porque os resultados principais esperados estão relacionados à prevenção, à identificação e à interrupção das operações indevidas.

#### R07 — Comprometimento da comunicação entre a API REST e o MongoDB

- **Identify:** identificação dos componentes, das conexões e dos dados transmitidos;
- **Protect:** proteção da comunicação e restrição do acesso de rede;
- **Detect:** monitoramento de falhas de conexão e atividades anormais;
- **Respond:** isolamento da conexão comprometida e investigação do incidente;
- **Recover:** restauração dos dados que tenham sido alterados ou corrompidos.

A função Recover foi incluída porque a alteração das informações transmitidas ou armazenadas pode exigir restauração. A função Govern não foi marcada porque as medidas principais são técnicas e operacionais.

#### R08 — Comprometimento de mensagens e tokens de e-mail

- **Govern:** definição de requisitos de segurança e responsabilidades para o provedor de e-mail;
- **Identify:** identificação dos dados, tokens e dependências envolvidos no envio;
- **Protect:** proteção dos tokens e configuração segura da integração;
- **Detect:** monitoramento de falhas, redirecionamentos e uso anormal dos tokens;
- **Respond:** invalidação dos tokens e contenção das contas afetadas.

A função Govern foi incluída porque o risco envolve um serviço externo e exige a definição das responsabilidades da plataforma e do provedor. Recover não foi marcada porque o tratamento principal consiste em invalidar tokens, conter o incidente e restabelecer a comunicação.

#### R09 — Indisponibilidade da plataforma

- **Govern:** definição de responsabilidades, critérios de disponibilidade e requisitos de continuidade;
- **Identify:** identificação dos componentes críticos e de suas dependências;
- **Protect:** limitação de requisições, redundância e cópias de segurança;
- **Detect:** monitoramento de disponibilidade, desempenho e consumo de recursos;
- **Respond:** contenção do evento e ativação dos procedimentos de resposta;
- **Recover:** restauração dos dados e dos serviços afetados.

O R09 está relacionado às seis funções porque envolve gestão da continuidade, identificação das dependências, medidas preventivas, monitoramento, resposta ao incidente e restauração dos serviços. Essa associação resulta das características específicas do risco, e não de uma marcação automática.

#### R10 — Execução de operações por CSRF

- **Identify:** identificação das operações autenticadas que alteram o estado da plataforma;
- **Protect:** utilização de tokens ant-CSRF, cookies seguros e validação da origem;
- **Detect:** monitoramento de requisições e operações anormais;
- **Respond:** invalidação da sessão e contenção das operações indevidas.

A função Govern não foi considerada necessária para o tratamento inicial, pois existem mecanismos técnicos bem definidos para reduzir o risco. Recover também não foi marcada porque a resposta principal está concentrada na contenção da sessão e das operações forjadas.

### 9.3.3 Síntese do mapeamento

As funções **Identify**, **Protect**, **Detect** e **Respond** aparecem com maior frequência porque os riscos analisados exigem conhecimento dos componentes envolvidos, aplicação de salvaguardas, monitoramento de atividades suspeitas e capacidade de contenção.

A função **Govern** foi associada aos riscos que dependem diretamente de políticas, responsabilidades, requisitos de desenvolvimento, gestão de contas privilegiadas, fornecedores ou continuidade dos serviços. Por esse motivo, foi relacionada a R01, R02, R06, R08 e R09.

A função **Recover** foi relacionada somente aos riscos que podem exigir restauração de dados ou serviços. Ela foi associada a R03, R05, R07 e R09.

Essa diferenciação evita tratar o NIST CSF 2.0 como uma lista automática de controles e mantém o mapeamento vinculado ao contexto da plataforma BAITA Eventos.
