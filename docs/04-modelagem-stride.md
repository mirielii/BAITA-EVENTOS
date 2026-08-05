# 4. Modelagem de ameaças com STRIDE

A modelagem de ameaças da Plataforma para Gestão de Eventos foi realizada com o auxílio do Microsoft Threat Modeling Tool.

O diagrama elaborado na ferramenta representa os seguintes elementos:

- participante sem conta;
- organizador;
- avaliador;
- administrador;
- página pública do evento;
- área autenticada;
- API REST;
- banco de dados MongoDB;
- serviço externo de e-mail;
- fluxos de dados entre esses elementos;
- fronteiras de confiança da aplicação web, da API e do banco de dados.

A ferramenta aplicou automaticamente o modelo STRIDE aos elementos e fluxos representados, gerando inicialmente 111 ameaças potenciais.

Como as ameaças são produzidas automaticamente a partir dos tipos de elementos utilizados no diagrama, nem todas correspondem necessariamente a riscos aplicáveis à arquitetura da plataforma. Por esse motivo, foi realizada uma revisão de aplicabilidade antes da inclusão das ameaças no trabalho.

## 4.1 Categorias do STRIDE

O STRIDE organiza as ameaças em seis categorias:

| Categoria | Significado | Propriedade de segurança afetada |
|---|---|---|
| Spoofing | Falsificação de identidade | Autenticidade |
| Tampering | Alteração indevida de dados | Integridade |
| Repudiation | Negação de uma ação realizada | Rastreabilidade |
| Information Disclosure | Exposição indevida de informações | Confidencialidade |
| Denial of Service | Indisponibilidade ou degradação do serviço | Disponibilidade |
| Elevation of Privilege | Obtenção indevida de permissões | Autorização |

## 4.2 Critérios de seleção

Uma ameaça foi mantida quando atendia aos seguintes critérios:

- envolvia um usuário, componente ou fluxo existente na arquitetura;
- correspondia a uma operação realmente realizada pela plataforma;
- poderia afetar um ativo relevante;
- apresentava um impacto de segurança identificável;
- era tecnicamente compatível com uma aplicação web composta por React, API REST e MongoDB;
- não estava suficientemente representada por outra ameaça equivalente;
- contribuía para a análise de uma das categorias do STRIDE.

Foram considerados ativos relevantes:

- contas e credenciais dos usuários internos;
- tokens e sessões de autenticação;
- papéis e permissões;
- solicitações de associação de organizadores;
- dados pessoais fornecidos nas inscrições;
- inscrições e controle de vagas;
- registros e códigos de check-in;
- avaliações, notas e resultados;
- configurações dos eventos e atividades;
- registros de auditoria;
- banco de dados;
- disponibilidade da plataforma.

## 4.3 Critérios de exclusão

Uma ameaça foi retirada quando apresentava uma ou mais das seguintes características:

- descrevia um elemento que não existe na arquitetura;
- pressupunha compartilhamento de memória ou ponteiros entre a página web e a API;
- tratava a página React como um processo privilegiado do servidor;
- atribuía privilégios de conta ao participante, que utiliza a plataforma sem cadastro;
- representava apenas a direção inversa de um fluxo já analisado;
- repetia uma ameaça já representada por outra ocorrência mais abrangente;
- não apresentava impacto relevante para os ativos identificados;
- descrevia um comportamento genérico sem relação suficiente com as funcionalidades da plataforma.

As exclusões não significam que a ferramenta apresentou um resultado incorreto. Elas indicam que o Microsoft Threat Modeling Tool gera possibilidades com base nos elementos do diagrama, cabendo aos responsáveis pelo sistema avaliar a aplicabilidade de cada resultado.

## 4.4 Resultado da seleção

Das 111 ameaças geradas automaticamente, 30 foram selecionadas para a análise contextualizada.

| Categoria STRIDE | Ameaças selecionadas |
|---|---:|
| Spoofing | 7 |
| Tampering | 3 |
| Repudiation | 6 |
| Information Disclosure | 2 |
| Denial of Service | 6 |
| Elevation of Privilege | 6 |
| **Total** | **30** |

O campo de prioridade não foi utilizado como critério de seleção, pois todas as ameaças foram exportadas pela ferramenta com prioridade `High`. Essa classificação automática não representa, isoladamente, o risco real de cada ameaça.

## 4.5 Ameaças selecionadas

### 4.5.1 Spoofing — falsificação de identidade

| ID | ID da ferramenta | Componente ou ativo | Ameaça identificada | Possível impacto |
|---|---:|---|---|---|
| T01 | 10 | Conta do organizador | Um atacante utiliza credenciais roubadas ou falsificadas para acessar a plataforma como organizador. | Alteração de eventos, atividades, inscrições, critérios de avaliação ou configurações. |
| T02 | 13 | Conta do avaliador | Um atacante obtém acesso indevido à conta de um avaliador. | Consulta de inscrições e registro ou alteração indevida de avaliações. |
| T03 | 16 | Conta do administrador | Um atacante se passa por um administrador da plataforma. | Gerenciamento indevido de usuários, permissões e solicitações de associação. |
| T04 | 98 | Serviço de e-mail | Um serviço falso se apresenta como o serviço de e-mail utilizado pela API. | Envio de informações, convites ou tokens para um destino controlado pelo atacante. |
| T05 | 112 | API REST | Um componente falso se apresenta como a API legítima durante a comunicação com o banco de dados. | Acesso indevido, manipulação de dados ou captura de credenciais do banco. |
| T06 | 113 | MongoDB como destino | Um banco falso se apresenta como o MongoDB utilizado pela plataforma. | Gravação de dados pessoais, inscrições ou avaliações em um destino controlado pelo atacante. |
| T07 | 121 | MongoDB como origem | Uma fonte falsa se apresenta como o MongoDB e envia dados incorretos para a API. | Exibição de informações falsas e tomada de decisões com base em dados adulterados. |

### 4.5.2 Tampering — alteração indevida

| ID | ID da ferramenta | Componente ou ativo | Ameaça identificada | Possível impacto |
|---|---:|---|---|---|
| T08 | 1 | Página pública | Dados maliciosos enviados nos formulários públicos são exibidos sem sanitização adequada, possibilitando XSS. | Execução de scripts no navegador, alteração da página e captura de informações. |
| T09 | 11 | Área autenticada | Dados maliciosos são armazenados ou exibidos na área autenticada sem tratamento adequado. | Execução de scripts na sessão de organizadores, avaliadores ou administradores. |
| T10 | 114 | MongoDB | Um atacante altera ou corrompe informações armazenadas no banco de dados. | Modificação de inscrições, check-ins, avaliações, resultados ou configurações. |

### 4.5.3 Repudiation — negação de ações

| ID | ID da ferramenta | Componente ou ativo | Ameaça identificada | Possível impacto |
|---|---:|---|---|---|
| T11 | 38 | Inscrições e check-ins | Uma operação pública é processada sem registros suficientes para comprovar sua origem, data e resultado. | Dificuldade para investigar inscrições falsas, duplicadas ou confirmações indevidas de presença. |
| T12 | 54 | Operações autenticadas | A API processa uma operação autenticada sem registrar adequadamente o usuário responsável. | Um usuário pode negar uma alteração realizada na plataforma. |
| T13 | 60 | Operações do organizador | Um organizador modifica um evento ou atividade e posteriormente nega a realização da operação. | Dificuldade para responsabilizar o usuário e recuperar a configuração correta. |
| T14 | 66 | Avaliações | Um avaliador registra ou modifica uma nota e posteriormente nega a autoria da ação. | Contestação dos resultados e perda de confiança no processo de avaliação. |
| T15 | 72 | Operações administrativas | Um administrador modifica usuários, permissões ou associações sem que exista uma trilha adequada de auditoria. | Impossibilidade de determinar o responsável por uma concessão indevida de acesso. |
| T16 | 115 | Gravações no MongoDB | Não existem evidências suficientes para confirmar se determinada informação foi gravada ou modificada no banco. | Dificuldade para investigar perda, inconsistência ou alteração de dados. |

### 4.5.4 Information Disclosure — exposição de informações

| ID | ID da ferramenta | Componente ou ativo | Ameaça identificada | Possível impacto |
|---|---:|---|---|---|
| T17 | 116 | Comunicação entre API e MongoDB | Um atacante intercepta os dados transmitidos entre a API e o banco de dados. | Exposição de dados pessoais, inscrições, avaliações, tokens ou configurações. |
| T18 | 123 | Controle de acesso aos dados | Uma falha de autorização permite que informações armazenadas sejam consultadas por um usuário sem permissão. | Exposição de inscrições, dados pessoais, avaliações ou informações de outros eventos. |

### 4.5.5 Denial of Service — indisponibilidade

| ID | ID da ferramenta | Componente ou ativo | Ameaça identificada | Possível impacto |
|---|---:|---|---|---|
| T19 | 40 | Requisições públicas | Um atacante envia grande quantidade de requisições à página pública ou interrompe o fluxo para a API. | Indisponibilidade de inscrições, consultas e check-ins. |
| T20 | 56 | Requisições autenticadas | Um atacante provoca a interrupção ou degradação das operações da área autenticada. | Organizadores, avaliadores e administradores ficam impedidos de trabalhar. |
| T21 | 100 | Serviço de e-mail | A comunicação com o serviço externo de e-mail é interrompida. | Convites, confirmações e notificações deixam de ser enviados. |
| T22 | 117 | API REST e MongoDB | Requisições abusivas provocam consumo excessivo de processamento, memória ou conexões. | Lentidão ou indisponibilidade da plataforma. |
| T23 | 119 | MongoDB | O banco de dados fica indisponível durante operações de leitura ou gravação. | Impossibilidade de consultar eventos, registrar inscrições, check-ins ou avaliações. |
| T24 | 124 | API REST | A API falha, para ou passa a responder lentamente. | Interrupção das principais funcionalidades da plataforma. |

### 4.5.6 Elevation of Privilege — elevação de privilégio

| ID | ID da ferramenta | Componente ou ativo | Ameaça identificada | Possível impacto |
|---|---:|---|---|---|
| T25 | 12 | Operações do organizador | Um usuário executa operações de organizador sem possuir o perfil ou o vínculo necessário com o evento. | Alteração de eventos, atividades, inscrições e configurações sem autorização. |
| T26 | 15 | Operações do avaliador | Um usuário acessa ou altera avaliações de atividades para as quais não foi designado. | Manipulação de notas, comentários e resultados. |
| T27 | 18 | Operações administrativas | Um organizador ou avaliador consegue executar uma funcionalidade exclusiva do administrador. | Alteração de permissões, usuários e aprovações administrativas. |
| T28 | 57 | API REST | Dados maliciosos enviados em uma requisição autenticada permitem a execução de código na API. | Comprometimento do servidor e acesso amplo aos dados da plataforma. |
| T29 | 58 | API REST | Um atacante manipula parâmetros para alterar o fluxo de execução da API. | Desvio das regras de negócio, alteração indevida de dados ou execução de operações não previstas. |
| T30 | 59 | Área autenticada | Um atacante induz o navegador de um usuário autenticado a enviar uma requisição que altera o estado da plataforma, caso a autenticação utilize credenciais, como cookies, enviadas automaticamente pelo navegador. | Execução de operações em nome do usuário por meio de CSRF. |

## 4.6 Ameaças consideradas mais relevantes

Entre as ameaças selecionadas, destacam-se como mais preocupantes:

- comprometimento da conta do administrador;
- falhas de autorização entre organizadores, avaliadores e administradores;
- acesso indevido aos dados pessoais das inscrições;
- alteração de notas, avaliações e resultados;
- manipulação de inscrições e registros de check-in;
- execução de código ou comandos indevidos na API;
- indisponibilidade durante períodos de inscrição, check-in e avaliação;
- falsificação do serviço de e-mail e exposição de convites ou tokens;
- ausência de registros de auditoria para operações importantes.

Essas ameaças possuem maior relevância porque podem afetar simultaneamente a confidencialidade, a integridade, a disponibilidade e a rastreabilidade das operações da plataforma.

## 4.7 Arquivos complementares

Além da análise apresentada nesta seção, estarão disponíveis no repositório os seguintes arquivos complementares:

- arquivo-fonte do modelo criado no Microsoft Threat Modeling Tool;
- imagem do diagrama de fluxo de dados;
- arquivo CSV original contendo as 111 ameaças geradas pela ferramenta;
- planilha estruturada com a saída original;
- planilha contendo as ameaças selecionadas para análise.

Esses arquivos registram as diferentes etapas da modelagem e permitem consultar tanto a saída original da ferramenta quanto o resultado da revisão realizada pelo grupo.
