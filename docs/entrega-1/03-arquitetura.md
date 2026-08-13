# Visão Geral da Arquitetura

## 1. Visão geral

O **BAITA EVENTOS** utilizará uma arquitetura cliente-servidor dividida em três camadas:

1. camada de apresentação;
2. camada de aplicação;
3. camada de dados.

Essa divisão permite separar a interface utilizada pelas pessoas, as regras de funcionamento da plataforma e o armazenamento das informações.

Cada evento possuirá sua própria página pública. Os participantes utilizarão essa página para consultar informações, realizar inscrições e confirmar presença.

Organizadores, avaliadores e administradores utilizarão uma área autenticada da plataforma.

## 2. Camada de apresentação

A camada de apresentação será composta por uma aplicação web desenvolvida com React.

Ela será responsável por exibir as páginas e receber as informações fornecidas pelas pessoas que utilizam a plataforma.

A aplicação será dividida em dois tipos de acesso.

### 2.1 Página pública do evento

Cada evento possuirá uma página pública específica.

Nessa página, o participante poderá:

- consultar informações do evento e de suas atividades;
- visualizar datas, horários e locais;
- preencher o formulário de inscrição;
- realizar a inscrição no evento ou em uma atividade;
- confirmar presença por meio de check-in, quando essa funcionalidade estiver habilitada.

O participante não precisará criar uma conta para acessar a página pública ou realizar uma inscrição.

Seus dados serão fornecidos e armazenados somente durante o processo de inscrição.

### 2.2 Área autenticada

A área autenticada será utilizada por organizadores, avaliadores e administradores.

As funcionalidades disponíveis dependerão do perfil e das permissões de cada usuário interno.

Por meio dessa área será possível:

- criar e configurar eventos;
- cadastrar atividades;
- consultar inscrições;
- configurar o check-in;
- registrar avaliações;
- gerenciar usuários internos;
- consultar registros de auditoria.

## 3. Camada de aplicação

A camada de aplicação será composta por uma API REST desenvolvida com Node.js e Express.

A API será responsável por receber as solicitações da aplicação web, validar os dados, verificar as permissões dos usuários internos, executar as regras de negócio e acessar o banco de dados.

Entre suas responsabilidades estão:

- autenticar organizadores, avaliadores e administradores;
- verificar papéis e permissões;
- validar dados recebidos;
- processar inscrições;
- controlar vagas;
- registrar check-ins;
- processar avaliações;
- calcular resultados e classificações;
- enviar notificações;
- registrar operações importantes para auditoria.

As decisões de acesso deverão ser realizadas pela API. Não será suficiente ocultar funcionalidades apenas na interface da aplicação.

## 4. Camada de dados

A camada de dados utilizará o MongoDB para armazenar as informações da plataforma.

O banco de dados poderá armazenar:

- usuários internos;
- papéis e permissões;
- eventos;
- atividades;
- inscrições;
- dados fornecidos pelos participantes durante a inscrição;
- registros de check-in;
- avaliações;
- resultados;
- configurações;
- registros de auditoria.

Os participantes não possuirão conta ou cadastro permanente como usuários da plataforma. Seus dados estarão associados às inscrições realizadas.

O acesso ao banco de dados deverá ocorrer por meio da API. Os navegadores não deverão acessar o banco diretamente.

## 5. Serviços externos

A plataforma poderá utilizar um serviço externo de e-mail para enviar:

- convites para usuários internos;
- confirmações de inscrição;
- avisos relacionados ao evento;
- notificações relacionadas às atividades.

A API será responsável por solicitar o envio dessas mensagens. Somente as informações necessárias deverão ser encaminhadas ao serviço de e-mail.

## 6. Fluxo geral do sistema

O funcionamento geral da plataforma seguirá este fluxo:

1. A pessoa acessa a aplicação web pelo navegador.
2. A aplicação envia uma solicitação para a API.
3. A API valida os dados recebidos.
4. Nas operações autenticadas, a API verifica a identidade, o perfil e as permissões do usuário interno.
5. Nas operações públicas, a API verifica as regras configuradas para o evento ou para a atividade.
6. A API executa a operação solicitada.
7. Quando necessário, a API consulta ou altera informações no banco de dados.
8. A API devolve o resultado para a aplicação web.
9. A aplicação apresenta o resultado.

## 7. Fluxos principais

### 7.1 Inscrição

1. O participante acessa a página pública específica do evento.
2. Consulta as informações do evento e de suas atividades.
3. Seleciona uma atividade, quando necessário.
4. Preenche o formulário de inscrição.
5. A aplicação envia os dados para a API.
6. A API valida o período de inscrição, as regras e a disponibilidade de vagas.
7. A inscrição e os dados fornecidos pelo participante são registrados no banco de dados.
8. O participante recebe a confirmação da inscrição.

### 7.2 Check-in

1. O participante acessa a página pública do evento ou apresenta seu código de identificação.
2. A aplicação envia a solicitação para a API.
3. A API verifica a inscrição, o período permitido e a validade do código.
4. A presença é registrada no banco de dados.
5. O sistema informa que o check-in foi concluído.

### 7.3 Avaliação

1. O avaliador entra na área autenticada.
2. A API verifica sua identidade e confirma se ele está associado à atividade.
3. O avaliador informa notas e observações.
4. A API valida os valores e o período de avaliação.
5. A avaliação é armazenada no banco de dados.
6. O sistema utiliza as avaliações para calcular os resultados.

## 8. Fronteiras de confiança

A arquitetura possui pontos nos quais os dados passam entre ambientes com diferentes níveis de confiança.

Os principais pontos são:

- comunicação entre o navegador e a aplicação;
- comunicação entre a aplicação web e a API;
- comunicação entre a API e o banco de dados;
- comunicação entre a API e o serviço de e-mail;
- separação entre a página pública e a área autenticada.

Esses pontos precisam de mecanismos de proteção, como conexão segura, autenticação dos usuários internos, autorização e validação dos dados recebidos.
