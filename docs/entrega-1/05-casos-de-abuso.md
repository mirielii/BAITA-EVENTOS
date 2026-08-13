#  Casos de abuso

## 1 Processo de elaboração

Os casos de abuso da plataforma BAITA Eventos foram elaborados com base nas ameaças selecionadas na modelagem STRIDE, realizada na Etapa 1 deste trabalho. A modelagem STRIDE fornece uma visão fragmentada das fragilidades da plataforma, na medida em que cada ameaça é analisada isoladamente em relação a um componente, um fluxo de dados ou um ativo específico. Os casos de abuso cumprem um papel complementar a essa análise: eles reconstroem essas ameaças isoladas em cenários de ataque coerentes, descrevendo como um atacante ou um usuário mal-intencionado poderia combinar uma ou mais fragilidades para alcançar um objetivo concreto dentro do sistema.

Enquanto a modelagem STRIDE identifica quais ameaças podem afetar os usuários, os componentes e os fluxos da plataforma, os casos de abuso descrevem como esse potencial de ameaça se traduziria em uma ação real, do ponto de vista de quem tenta explorá-la. Essa mudança de perspectiva — da ameaça técnica para o comportamento do atacante — é o que permite avaliar, na Etapa 2, a probabilidade e o impacto de cada risco de forma mais realista, já que se passa a analisar um cenário de exploração completo, e não apenas um evento isolado.

Para a construção dos casos, optou-se por não elaborar um caso de abuso para cada ameaça individualmente. Essa escolha metodológica se justifica porque diversas ameaças da modelagem STRIDE, embora tecnicamente distintas, convergem para o mesmo objetivo final de um atacante. Por exemplo, ameaças de spoofing de credenciais e ameaças de repúdio de ações podem, juntas, descrever um único cenário de comprometimento de conta. Dessa forma, as ameaças que representam um mesmo objetivo foram agrupadas em cenários concretos, e cada caso de abuso passou a representar um objetivo de ataque, e não uma técnica isolada. Como consequência direta dessa abordagem, um mesmo caso pode estar relacionado a várias ameaças da modelagem STRIDE, e uma mesma ameaça pode participar de mais de um caso, caso ela seja relevante para mais de um objetivo de ataque.

Os identificadores iniciados por `T` correspondem às ameaças documentadas no arquivo de modelagem STRIDE da Etapa 1. Essa relação mantém a rastreabilidade entre os dois documentos, permitindo verificar, a qualquer momento, qual ameaça técnica originou determinado cenário de abuso e evitando que ameaças identificadas anteriormente sejam perdidas ou ignoradas ao longo do trabalho. Essa rastreabilidade servirá como base para a análise de riscos da Etapa 2, na qual cada caso de abuso é convertido em um risco a ser avaliado, priorizado e tratado.

## 2 Estrutura dos casos

Para garantir uniformidade na análise e facilitar a comparação entre os diferentes cenários, todos os casos de abuso identificados foram documentados seguindo a mesma estrutura. Cada caso de abuso contém:

- **identificador**, utilizado para referenciar o caso de forma única ao longo do trabalho, inclusive nas etapas seguintes de análise de riscos e definição de controles;
- **título**, que resume de forma objetiva o objetivo central do cenário de abuso;
- **ameaças STRIDE relacionadas**, que indicam quais ameaças da Etapa 1 fundamentam o cenário e garantem a rastreabilidade entre os documentos;
- **ator responsável pelo abuso**, descrevendo o perfil de quem executaria a ação, podendo se tratar de um atacante externo sem vínculo com a plataforma, de um usuário interno mal-intencionado ou de um usuário legítimo cuja conta foi comprometida;
- **objetivo do atacante**, isto é, o resultado que o ator pretende alcançar ao explorar as fragilidades descritas;
- **condições necessárias**, que descrevem os pré-requisitos hipotéticos para que o abuso pudesse ocorrer;
- **fluxo de abuso**, apresentado como uma sequência lógica e numerada de passos, descrevendo como o cenário se desenvolveria do início até a concretização do objetivo do atacante;
- **impacto esperado**, relacionando as consequências do abuso para a plataforma, para os usuários e para os processos de negócio envolvidos;
- **categorias STRIDE relacionadas**, que classificam o caso segundo as categorias do próprio método STRIDE, reforçando a coerência entre a modelagem de ameaças e os cenários descritos.

É importante destacar que as condições necessárias representam situações hipotéticas que poderiam permitir a realização do abuso, descritas com a finalidade de orientar a análise de riscos e a posterior definição de controles. A presença dessas condições no caso não significa que a vulnerabilidade correspondente já exista de fato na plataforma, tampouco que qualquer uma delas tenha sido observada ou testada durante o desenvolvimento do trabalho até este ponto. Da mesma forma, os fluxos de abuso descritos representam sequências plausíveis de exploração sob a ótica do atacante, e não roteiros de teste ou evidências de exploração já realizada, servindo como insumo para a modelagem de riscos e para a definição das práticas de segurança nas etapas seguintes do projeto.

## 3 Casos de abuso identificados

### CA01 — Comprometimento de conta interna

**Ameaças STRIDE relacionadas:** T10, T13, T16 e T54.

**Ator:** atacante externo.

**Objetivo:** assumir a identidade de um organizador, avaliador ou administrador para acessar a área autenticada e executar operações em nome da vítima.

**Condições necessárias:**

- existência de uma conta interna ativa;
- obtenção ou descoberta das credenciais da vítima;
- ausência ou insuficiência de mecanismos adicionais de proteção da autenticação;
- ausência de bloqueio ou detecção de tentativas suspeitas de acesso.

**Fluxo de abuso:**

1. O atacante identifica o endereço de e-mail de um usuário interno.
2. As credenciais são obtidas por engenharia social, vazamento, reutilização de senha ou tentativa automatizada.
3. O atacante acessa a tela de autenticação do BAITA Eventos.
4. As credenciais comprometidas são informadas.
5. A plataforma reconhece as credenciais como válidas.
6. O atacante acessa a área autenticada com as permissões da vítima.
7. Informações são consultadas ou operações são realizadas em nome do usuário comprometido.

**Impacto esperado:**

- acesso indevido à área autenticada;
- exposição de inscrições, avaliações ou configurações;
- alteração indevida de eventos, atividades, usuários ou permissões;
- perda de rastreabilidade das operações;
- comprometimento da confiança na plataforma.

**Categorias STRIDE relacionadas:** Spoofing e Repudiation.

### CA02 — Acesso a operações sem autorização

**Ameaças STRIDE relacionadas:** T12, T15, T18, T60, T66, T72 e T123.

**Ator:** usuário interno autenticado.

**Objetivo:** executar operações de outro perfil ou acessar recursos de eventos aos quais não está vinculado.

**Condições necessárias:**

- o ator possui uma conta válida;
- os identificadores dos recursos podem ser observados ou modificados;
- a API REST não verifica corretamente o perfil, a permissão ou o vínculo com o evento;
- a interface oculta a funcionalidade, mas a API ainda aceita a requisição.

**Fluxo de abuso:**

1. O usuário entra normalmente na área autenticada.
2. Uma requisição utilizada para consultar ou alterar um recurso é identificada.
3. O usuário modifica o identificador do evento, da atividade, da inscrição ou do usuário.
4. A requisição alterada é enviada diretamente à API REST.
5. A API processa a solicitação sem verificar adequadamente o perfil ou o vínculo do usuário.
6. O usuário consulta informações ou executa uma operação para a qual não possui autorização.
7. A operação pode ser negada posteriormente caso não exista uma trilha de auditoria suficiente.

**Impacto esperado:**

- acesso indevido a informações de outros eventos;
- exposição de dados pessoais ou avaliações;
- alteração indevida de eventos, atividades e permissões;
- obtenção indevida de privilégios;
- perda de rastreabilidade das operações.

**Categorias STRIDE relacionadas:** Elevation of Privilege, Information Disclosure e Repudiation.

### CA03 — Manipulação de avaliações e resultados

**Ameaças STRIDE relacionadas:** T13, T15, T66 e T114.

**Ator:** avaliador, usuário interno mal-intencionado ou atacante com uma conta comprometida.

**Objetivo:** alterar notas, comentários, pareceres, resultados ou classificações para beneficiar ou prejudicar participantes.

**Condições necessárias:**

- acesso à área autenticada;
- ausência de verificação adequada da designação do avaliador;
- possibilidade de modificar os identificadores das avaliações;
- ausência de controles de integridade ou registros de auditoria suficientes.

**Fluxo de abuso:**

1. O ator acessa a área autenticada.
2. O identificador de uma atividade, inscrição ou avaliação é obtido.
3. O ator modifica a requisição para acessar uma avaliação para a qual não foi designado.
4. A API REST aceita a solicitação sem verificar corretamente a designação.
5. Notas, comentários ou pareceres são registrados ou modificados.
6. Os resultados e as classificações são calculados utilizando as informações alteradas.
7. O ator pode negar posteriormente a autoria da alteração.

**Impacto esperado:**

- alteração indevida de notas, comentários e pareceres;
- alteração indevida de resultados e classificações;
- prejuízo aos participantes;
- perda de rastreabilidade das avaliações;
- comprometimento da confiança no processo de avaliação.

**Categorias STRIDE relacionadas:** Spoofing, Tampering, Repudiation e Elevation of Privilege.

### CA04 — Fraudes em inscrições e registros de presença

**Ameaças STRIDE relacionadas:** T38, T40, T54 e T115.

**Ator:** participante mal-intencionado ou atacante externo.

**Objetivo:** criar inscrições falsas, ocupar vagas indevidamente ou registrar presenças que não ocorreram.

**Condições necessárias:**

- acesso à página pública do evento;
- ausência de mecanismos para limitar solicitações automatizadas;
- códigos de check-in previsíveis, reutilizáveis ou compartilháveis;
- ausência de registros suficientes sobre inscrições e confirmações de presença.

**Fluxo de abuso:**

1. O atacante acessa a página pública de um evento.
2. Requisições de inscrição ou check-in são observadas.
3. O atacante automatiza o envio de formulários ou modifica os dados enviados.
4. Diversas inscrições são realizadas para ocupar as vagas disponíveis.
5. Códigos de check-in são testados, reutilizados ou compartilhados.
6. A API REST registra inscrições ou presenças indevidas.
7. A ausência de registros suficientes dificulta a identificação da origem das operações.

**Impacto esperado:**

- criação de inscrições falsas ou duplicadas;
- ocupação indevida de vagas;
- registro indevido de presença;
- indisponibilidade das inscrições para participantes legítimos;
- perda de rastreabilidade dos registros.

**Categorias STRIDE relacionadas:** Repudiation e Denial of Service.

### CA05 — Injeção de conteúdo malicioso

**Ameaças STRIDE relacionadas:** T01, T11, T10, T13 e T16.

**Ator:** participante mal-intencionado ou atacante externo.

**Objetivo:** inserir scripts maliciosos para comprometer a página pública ou a sessão de um usuário interno.

**Condições necessárias:**

- existência de formulários ou campos que aceitam conteúdo fornecido pelos usuários;
- ausência de validação, sanitização ou codificação adequada;
- exibição posterior do conteúdo armazenado;
- acesso de um usuário interno à página que apresenta o conteúdo malicioso.

**Fluxo de abuso:**

1. O atacante identifica um formulário público ou campo de entrada.
2. Um script malicioso é inserido em um dos campos.
3. A plataforma armazena ou processa o conteúdo sem tratamento adequado.
4. O conteúdo é exibido na página pública ou na área autenticada.
5. O navegador executa o script.
6. Informações da página ou da sessão podem ser capturadas.
7. Caso a sessão de um usuário interno seja comprometida, o atacante poderá executar operações em seu nome.

**Impacto esperado:**

- comprometimento da página pública;
- comprometimento das sessões dos usuários internos;
- exposição de informações;
- acesso indevido à área autenticada;
- alteração indevida de dados e configurações.

**Categorias STRIDE relacionadas:** Tampering e Spoofing.

### CA06 — Execução de operações indevidas por meio da API REST

**Ameaças STRIDE relacionadas:** T57, T58, T112 e T114.

**Ator:** atacante externo ou usuário autenticado mal-intencionado.

**Objetivo:** explorar a API REST para executar operações não autorizadas, desviar regras de negócio ou alterar informações processadas pela plataforma.

**Condições necessárias:**

- existência de entradas processadas sem validação suficiente;
- utilização insegura de dados recebidos em comandos ou operações internas;
- falhas na autenticação entre a API REST e o MongoDB;
- permissões excessivas concedidas ao processo da API REST.

**Fluxo de abuso:**

1. O atacante identifica uma rota que recebe parâmetros ou dados controláveis.
2. Uma entrada especialmente preparada é enviada à API REST.
3. A API processa o conteúdo sem validação suficiente.
4. O fluxo normal das regras de negócio é desviado ou um código não autorizado é executado.
5. O atacante utiliza as permissões da API REST para acessar ou alterar informações.
6. Dados armazenados no MongoDB podem ser consultados, modificados ou corrompidos.

**Impacto esperado:**

- execução indevida de operações;
- desvio das regras de negócio;
- acesso indevido às informações armazenadas;
- alteração indevida de inscrições, avaliações ou configurações;
- comprometimento da integridade do banco de dados.

**Categorias STRIDE relacionadas:** Elevation of Privilege, Spoofing e Tampering.

### CA07 — Comprometimento da comunicação entre a API REST e o MongoDB

**Ameaças STRIDE relacionadas:** T113, T116, T121 e T123.

**Ator:** atacante com acesso à rede, à infraestrutura ou a uma conta interna.

**Objetivo:** interceptar, redirecionar ou adulterar a comunicação entre a API REST e o MongoDB para obter informações ou fornecer dados falsos à plataforma.

**Condições necessárias:**

- comunicação desprotegida ou configurada incorretamente;
- ausência de autenticação adequada entre a API REST e o MongoDB;
- controle de acesso insuficiente;
- possibilidade de redirecionar a conexão para um serviço controlado pelo atacante.

**Fluxo de abuso:**

1. O atacante identifica o fluxo de comunicação entre a API REST e o MongoDB.
2. A comunicação é interceptada ou redirecionada.
3. Informações transmitidas são capturadas.
4. Um banco falso pode se apresentar como o MongoDB legítimo.
5. Dados incorretos ou adulterados são enviados à API REST.
6. A plataforma utiliza ou apresenta as informações falsas.
7. Uma falha de autorização também pode permitir a consulta de dados de outros eventos.

**Impacto esperado:**

- exposição de dados pessoais, inscrições e avaliações;
- alteração indevida das informações processadas;
- apresentação de informações falsas;
- comprometimento dos resultados;
- violação da privacidade dos participantes.

**Categorias STRIDE relacionadas:** Spoofing e Information Disclosure.

### CA08 — Redirecionamento de e-mails e tokens

**Ameaças STRIDE relacionadas:** T98 e T100.

**Ator:** atacante externo com capacidade de interferir na integração com o serviço de e-mail.

**Objetivo:** receber informações, convites ou tokens destinados ao serviço legítimo ou impedir o envio das mensagens.

**Condições necessárias:**

- configuração incorreta do serviço externo;
- ausência de autenticação adequada do serviço de e-mail;
- comprometimento das credenciais utilizadas pela API REST;
- possibilidade de redirecionar ou interromper a comunicação.

**Fluxo de abuso:**

1. O atacante obtém ou modifica informações de configuração da integração.
2. Um serviço controlado pelo atacante se apresenta como o serviço de e-mail legítimo.
3. A API REST envia convites, notificações ou tokens ao serviço falso.
4. O atacante captura as informações transmitidas.
5. Alternativamente, o atacante interrompe o fluxo para impedir o envio das mensagens.
6. Convites ou confirmações podem deixar de chegar aos destinatários legítimos.

**Impacto esperado:**

- exposição de convites, notificações ou tokens;
- acesso indevido a contas internas;
- indisponibilidade do envio de mensagens;
- comprometimento do processo de criação ou recuperação de contas.

**Categorias STRIDE relacionadas:** Spoofing e Denial of Service.

### CA09 — Indisponibilidade da plataforma

**Ameaças STRIDE relacionadas:** T40, T56, T100, T117, T119 e T124.

**Ator:** atacante externo ou usuário mal-intencionado.

**Objetivo:** impedir ou degradar o acesso às funcionalidades da plataforma.

**Condições necessárias:**

- ausência ou insuficiência de limites de requisição;
- recursos computacionais limitados;
- ausência de mecanismos adequados de monitoramento e recuperação;
- dependência da API REST, do MongoDB ou do serviço externo de e-mail.

**Fluxo de abuso:**

1. O atacante identifica rotas públicas ou autenticadas que consomem recursos relevantes.
2. Uma grande quantidade de requisições é enviada simultaneamente.
3. A API REST passa a consumir processamento, memória ou conexões em excesso.
4. O MongoDB recebe um volume elevado de operações.
5. A API REST ou o banco passa a responder lentamente ou fica indisponível.
6. A comunicação com o serviço de e-mail também pode ser interrompida.
7. Usuários legítimos deixam de acessar as funcionalidades da plataforma.

**Impacto esperado:**

- indisponibilidade da página pública;
- indisponibilidade da área autenticada;
- interrupção de inscrições e check-ins;
- interrupção das avaliações;
- indisponibilidade do envio de convites e notificações.

**Categorias STRIDE relacionadas:** Denial of Service.

### CA10 — Execução de operação por CSRF

**Ameaças STRIDE relacionadas:** T12, T18, T59 e T72.

**Ator:** atacante externo.

**Objetivo:** induzir o navegador de um usuário autenticado a executar uma operação sem seu consentimento.

**Condições necessárias:**

- autenticação baseada em cookies ou credenciais enviadas automaticamente pelo navegador;
- usuário autenticado na plataforma;
- ausência de proteção contra CSRF;
- existência de uma operação que altera o estado da plataforma.

**Fluxo de abuso:**

1. O usuário interno entra normalmente na área autenticada.
2. Durante a sessão, o usuário acessa um endereço ou conteúdo controlado pelo atacante.
3. O conteúdo malicioso envia uma requisição para a API REST.
4. O navegador inclui automaticamente as credenciais da sessão.
5. A API REST interpreta a requisição como uma operação legítima.
6. Uma configuração, permissão ou associação é alterada sem o consentimento do usuário.
7. A ausência de registros suficientes pode dificultar a investigação da operação.

**Impacto esperado:**

- execução indevida de operações em nome do usuário;
- alteração indevida de eventos, permissões ou associações;
- obtenção indevida de privilégios;
- perda de rastreabilidade das operações.

**Categorias STRIDE relacionadas:** Elevation of Privilege e Repudiation.

## 4 Matriz de rastreabilidade

A matriz relaciona cada ameaça selecionada na modelagem STRIDE aos casos de abuso em que ela está representada.

| Ameaça | Categoria STRIDE | Casos de abuso relacionados |
|---|---|---|
| T01 | Tampering | CA05 |
| T10 | Spoofing | CA01 e CA05 |
| T11 | Tampering | CA05 |
| T12 | Elevation of Privilege | CA02 e CA10 |
| T13 | Spoofing | CA01, CA03 e CA05 |
| T15 | Elevation of Privilege | CA02 e CA03 |
| T16 | Spoofing | CA01 e CA05 |
| T18 | Elevation of Privilege | CA02 e CA10 |
| T38 | Repudiation | CA04 |
| T40 | Denial of Service | CA04 e CA09 |
| T54 | Repudiation | CA01 e CA04 |
| T56 | Denial of Service | CA09 |
| T57 | Elevation of Privilege | CA06 |
| T58 | Elevation of Privilege | CA06 |
| T59 | Elevation of Privilege | CA10 |
| T60 | Repudiation | CA02 |
| T66 | Repudiation | CA02 e CA03 |
| T72 | Repudiation | CA02 e CA10 |
| T98 | Spoofing | CA08 |
| T100 | Denial of Service | CA08 e CA09 |
| T112 | Spoofing | CA06 |
| T113 | Spoofing | CA07 |
| T114 | Tampering | CA03 e CA06 |
| T115 | Repudiation | CA04 |
| T116 | Information Disclosure | CA07 |
| T117 | Denial of Service | CA09 |
| T119 | Denial of Service | CA09 |
| T121 | Spoofing | CA07 |
| T123 | Information Disclosure | CA02 e CA07 |
| T124 | Denial of Service | CA09 |

A matriz demonstra que todas as 30 ameaças selecionadas na modelagem STRIDE estão representadas em pelo menos um caso de abuso.

## 5 Síntese

Os casos de abuso demonstram que as ameaças identificadas podem ser combinadas em cenários com objetivos concretos, como comprometer contas, obter permissões indevidas, alterar avaliações, fraudar inscrições, expor dados ou interromper a plataforma.

Os cenários de maior impacto são aqueles que envolvem contas administrativas, dados pessoais dos participantes, avaliações, resultados, inscrições e disponibilidade durante períodos críticos.

Os casos de abuso servirão como base para a análise de riscos da Etapa 2. Cada caso poderá dar origem a um ou mais riscos, que posteriormente poderão ser avaliados e relacionados às estratégias de tratamento e às funções do NIST CSF.
