# 1. Tratamento dos riscos com o NIST CSF

## 1.1 Estratégias de tratamento

Após a priorização dos riscos, foram definidas as estratégias mais adequadas para seu tratamento.

As estratégias foram selecionadas considerando a classificação dos riscos, os componentes afetados, as consequências possíveis e a viabilidade de atuação sobre cada cenário. Para esta análise, foram consideradas quatro possibilidades:

- **Evitar:** eliminar a atividade ou condição que origina o risco;
- **Reduzir:** implementar medidas para diminuir a probabilidade ou o impacto;
- **Compartilhar:** dividir parte da responsabilidade com terceiros;
- **Aceitar:** manter o risco de maneira consciente, desde que esteja dentro dos critérios estabelecidos.

A estratégia predominante foi a redução, pois a maioria dos riscos está relacionada a funcionalidades essenciais da plataforma BAITA Eventos. Essas funcionalidades não podem ser eliminadas, mas podem receber medidas de proteção, monitoramento e resposta.

### 1.1.1 Estratégias selecionadas

| Risco | Estratégia | Justificativa |
|---|---|---|
| R01 — Comprometimento de contas autenticadas | Reduzir | A autenticação é necessária para organizadores, avaliadores e administradores. Como não é possível eliminar essa funcionalidade, devem ser adotadas medidas para proteger credenciais, sessões e tentativas de acesso. |
| R02 — Execução de operações sem autorização | Reduzir | As operações de gestão e administração fazem parte do funcionamento da plataforma. O risco pode ser reduzido com verificações de autorização no servidor, separação de permissões e validação do perfil antes de cada operação. |
| R03 — Manipulação de avaliações | Reduzir | O processo de avaliação é uma funcionalidade necessária. A aplicação de controle de acesso, registros de auditoria e validações de integridade pode diminuir a possibilidade de alterações indevidas. |
| R04 — Fraudes em inscrições e registros de presença | Reduzir | As inscrições e os registros de presença são funcionalidades públicas ou acessíveis ao participante sem conta. Validações, códigos de confirmação, limites de requisições e mecanismos contra automação podem reduzir as fraudes. |
| R05 — Inserção de conteúdo malicioso | Reduzir | A plataforma precisa receber e apresentar dados informados pelos usuários. A validação das entradas, a sanitização do conteúdo e a codificação segura das saídas reduzem a possibilidade de execução de conteúdo malicioso. |
| R06 — Operações indevidas por meio da API REST | Reduzir | A API REST é essencial para o funcionamento da plataforma. Validações no servidor, autenticação, autorização e tratamento seguro das entradas podem diminuir a possibilidade de exploração das regras de negócio. |
| R07 — Comprometimento da comunicação entre a API REST e o MongoDB | Reduzir | A comunicação entre a API REST e o banco de dados é necessária. O uso de conexão protegida, autenticação entre componentes, restrição de rede e validação de certificados pode reduzir a possibilidade de interceptação ou alteração dos dados. |
| R08 — Comprometimento de mensagens e tokens de e-mail | Compartilhar | Parte do risco depende do provedor utilizado para o envio das mensagens. A responsabilidade pode ser compartilhada com um serviço de e-mail que ofereça proteção da comunicação, autenticação do domínio, monitoramento e garantias de disponibilidade. Mesmo com o compartilhamento, permanecem controles internos sob responsabilidade da plataforma. |
| R09 — Indisponibilidade da plataforma | Reduzir | A interrupção dos serviços não pode ser completamente evitada, mas sua probabilidade e seu impacto podem ser reduzidos com limitação de requisições, monitoramento, cópias de segurança, redundância e procedimentos de recuperação. O tratamento também pode contar com o apoio de serviços terceirizados de infraestrutura, sem retirar da plataforma a responsabilidade pela continuidade e pela recuperação. |
| R10 — Execução de operações por CSRF | Reduzir | As operações autenticadas são necessárias para a plataforma. Tokens ant-CSRF, configuração segura de cookies e validação da origem das requisições podem reduzir a possibilidade de operações forjadas. |

### 1.1.2 Observações sobre o compartilhamento de riscos

No R08, o compartilhamento não transfere integralmente a responsabilidade para o provedor de e-mail. A plataforma BAITA Eventos continua responsável por selecionar e configurar adequadamente o serviço, limitar a validade dos tokens, impedir sua reutilização e evitar a inclusão de informações sensíveis desnecessárias nas mensagens.

No R09, a estratégia principal permanece sendo a redução. Entretanto, alguns mecanismos de disponibilidade podem contar com serviços terceirizados, como infraestrutura em nuvem, distribuição de conteúdo, balanceamento de carga e serviços gerenciados. Esse apoio representa um compartilhamento parcial da operação, mas não elimina a responsabilidade da plataforma pelo planejamento da continuidade e pela recuperação dos serviços.

## 1.2 Funções do NIST CSF 2.0

O NIST Cybersecurity Framework 2.0 organiza os resultados esperados de segurança cibernética em seis funções: **Govern**, **Identify**, **Protect**, **Detect**, **Respond** e **Recover**.

As funções não representam controles específicos. Elas fornecem uma estrutura para organizar os resultados de segurança esperados e orientar a definição das medidas de tratamento. Um mesmo risco pode estar relacionado a mais de uma função, desde que essa relação seja justificada.

### 1.2.1 Govern — Governar

A função **Govern** está relacionada ao estabelecimento de políticas, responsabilidades, critérios de decisão e supervisão da segurança. No BAITA Eventos, inclui a definição de responsabilidades administrativas, regras de acesso, requisitos para fornecedores e critérios de aceitação dos riscos.

### 1.2.2 Identify — Identificar

A função **Identify** envolve a compreensão dos ativos, dependências, ameaças, vulnerabilidades e riscos. Na plataforma, inclui a identificação das contas privilegiadas, funcionalidades críticas, dados tratados, integrações externas e dependências entre a aplicação web, a API REST, o MongoDB e o serviço de e-mail.

### 1.2.3 Protect — Proteger

A função **Protect** reúne resultados destinados a reduzir a probabilidade ou o impacto dos riscos. Relaciona-se à autenticação, autorização, proteção de sessões, validação de entradas, segurança das comunicações, cópias de segurança e proteção contra requisições maliciosas.

### 1.2.4 Detect — Detectar

A função **Detect** envolve a identificação de atividades suspeitas e eventos de segurança. No BAITA Eventos, pode incluir registros de auditoria, monitoramento de autenticações, detecção de alterações indevidas, alertas de excesso de requisições e acompanhamento das integrações.

### 1.2.5 Respond — Responder

A função **Respond** está relacionada às ações executadas após a identificação de um incidente. Inclui bloquear contas ou sessões, interromper operações maliciosas, investigar registros, comunicar os responsáveis e conter os efeitos do evento.

### 1.2.6 Recover — Recuperar

A função **Recover** envolve a restauração dos serviços e dados afetados. Para a plataforma, pode incluir a recuperação de informações a partir de cópias de segurança, a reversão de alterações indevidas e o restabelecimento das funcionalidades após uma indisponibilidade.
