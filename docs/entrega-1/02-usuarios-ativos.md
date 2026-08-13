# 3. Usuários, Ativos e Pontos de Interação

## 1. Usuários do sistema

O **BAITA EVENTOS** possui quatro perfis principais de interação:

- participante;
- organizador;
- avaliador;
- administrador.

O participante utiliza a página pública específica de um evento e não possui uma conta na plataforma.

Organizadores, avaliadores e administradores são usuários internos. Eles possuem conta e acessam uma área autenticada.

## 2. Perfis de acesso

### 2.1 Participante

O participante acessa a página pública específica de um evento.

Ele poderá:

- consultar informações do evento e de suas atividades;
- preencher formulários de inscrição;
- realizar inscrições;
- receber confirmações e notificações;
- confirmar presença por meio de check-in.

O participante não possui conta ou acesso à área interna. Seus dados são fornecidos quando ele realiza uma inscrição.

### 2.2 Organizador

O organizador possui uma conta e utiliza a área autenticada.

Ele poderá:

- criar eventos;
- editar informações dos eventos;
- cadastrar e configurar atividades;
- definir períodos de inscrição;
- criar formulários personalizados;
- controlar a quantidade de vagas;
- configurar o check-in;
- definir critérios de avaliação;
- solicitar a associação de novos organizadores ao evento;
- associar avaliadores;
- consultar inscrições e participantes;
- acompanhar avaliações e resultados.

A associação de um novo organizador não será realizada imediatamente. A solicitação deverá ser analisada e aceita por um administrador.

O organizador deve acessar somente os eventos aos quais estiver vinculado.

### 2.3 Avaliador

O avaliador possui uma conta e utiliza a área autenticada.

Ele poderá:

- consultar as atividades para as quais foi designado;
- visualizar as inscrições que deverá avaliar;
- registrar notas;
- adicionar observações;
- atualizar avaliações dentro do período permitido.

O avaliador não deve acessar ou alterar avaliações de atividades para as quais não foi designado.

### 2.4 Administrador

O administrador possui uma conta com acesso ampliado.

Ele poderá:

- gerenciar usuários internos;
- enviar convites para novos usuários;
- atribuir papéis e permissões;
- analisar e aceitar solicitações de associação de novos organizadores aos eventos;
- consultar eventos cadastrados;
- acompanhar a operação da plataforma;
- consultar registros de auditoria.

Por possuir permissões ampliadas, a conta de administrador precisa de proteção adicional.

## 3. Resumo dos perfis

| Perfil | Possui conta? | Forma de acesso | Principais operações |
|---|---|---|---|
| Participante | Não | Página pública específica do evento | Consultar informações, realizar inscrição e confirmar presença |
| Organizador | Sim | Área autenticada | Criar e configurar eventos e solicitar a associação de organizadores |
| Avaliador | Sim | Área autenticada | Registrar notas e observações |
| Administrador | Sim | Área autenticada | Gerenciar usuários e permissões, aprovar associações e consultar registros de auditoria |

## 4. Ativos do sistema

Ativos são dados, recursos ou componentes que podem causar prejuízos caso sejam acessados, alterados, destruídos ou indisponibilizados indevidamente.

### 4.1 Contas e credenciais

As contas pertencem aos organizadores, avaliadores e administradores.

Esse ativo inclui:

- endereço de e-mail;
- senha;
- tokens de autenticação;
- sessões de acesso;
- convites para criação de conta.

O comprometimento desses dados pode permitir que uma pessoa se passe por um usuário interno.

### 4.2 Papéis e permissões

Os papéis determinam quais funcionalidades cada usuário interno pode acessar.

Uma alteração indevida pode permitir que um avaliador ou organizador obtenha permissões administrativas.

### 4.3 Solicitações de associação de organizadores

As solicitações representam pedidos para associar novos organizadores a um evento.

Elas devem permanecer pendentes até a análise de um administrador. Uma aprovação indevida pode conceder acesso às informações e configurações do evento a uma pessoa não autorizada.

### 4.4 Dados fornecidos nas inscrições

Os participantes fornecem dados por meio dos formulários de inscrição.

Dependendo da configuração do evento, esses dados podem incluir:

- nome;
- e-mail;
- telefone;
- instituição;
- curso;
- respostas a campos personalizados;
- informações necessárias para participação.

Esses dados devem ser utilizados apenas para as finalidades relacionadas ao evento.

### 4.5 Inscrições

Os registros de inscrição representam o vínculo do participante com um evento ou uma atividade.

Eles podem conter:

- dados fornecidos pelo participante;
- evento ou atividade selecionada;
- data e horário da inscrição;
- situação da inscrição;
- confirmação ou cancelamento;
- ocupação de uma vaga.

Alterações indevidas podem provocar perda de vagas, inscrições falsas ou cancelamentos não autorizados.

### 4.6 Registros de check-in

Os registros de check-in comprovam a presença de um participante.

Esse ativo pode incluir:

- código ou identificador de check-in;
- evento ou atividade;
- data e horário;
- situação da confirmação de presença.

A alteração desses registros pode produzir presenças falsas ou impedir a comprovação da participação.

### 4.7 Avaliações e resultados

Esse ativo inclui:

- critérios de avaliação;
- pesos;
- notas;
- comentários;
- resultados;
- classificações.

Alterações indevidas podem modificar os resultados de uma atividade e prejudicar participantes.

### 4.8 Configurações dos eventos e atividades

As configurações determinam o funcionamento de cada evento e atividade.

Elas podem incluir:

- período de inscrição;
- quantidade de vagas;
- campos do formulário;
- regras de check-in;
- critérios de avaliação;
- datas e horários;
- organizadores e avaliadores associados.

Uma alteração indevida pode afetar todo o funcionamento do evento.

### 4.9 Registros de auditoria

Os registros de auditoria permitem acompanhar operações importantes realizadas na plataforma.

Eles podem registrar:

- autenticações;
- falhas de acesso;
- alterações de usuários e permissões;
- solicitações e aprovações de associação de organizadores;
- mudanças nas configurações;
- operações administrativas;
- registro ou alteração de avaliações.

Esses registros precisam ser protegidos contra alteração e exclusão indevidas.

### 4.10 Banco de dados

O banco de dados armazena as informações operacionais da plataforma.

Seu comprometimento pode causar:

- exposição de dados;
- alteração de inscrições;
- perda de avaliações;
- exclusão de eventos;
- indisponibilidade do sistema.

### 4.11 Disponibilidade da plataforma

A plataforma precisa permanecer disponível, principalmente durante:

- períodos de inscrição;
- realização de eventos;
- horários de check-in;
- períodos de avaliação.

Uma interrupção nesses momentos pode impedir inscrições, confirmações de presença e avaliações.

## 5. Resumo dos ativos

| Ativo | Possível prejuízo |
|---|---|
| Contas e credenciais | Acesso indevido à área interna |
| Papéis e permissões | Obtenção de privilégios não autorizados |
| Solicitações de associação | Inclusão de organizadores não autorizados |
| Dados das inscrições | Exposição de informações pessoais |
| Inscrições | Fraude, cancelamento ou ocupação indevida de vagas |
| Registros de check-in | Confirmação falsa ou perda do registro de presença |
| Avaliações e resultados | Alteração indevida das classificações |
| Configurações | Funcionamento incorreto do evento |
| Registros de auditoria | Dificuldade para investigar incidentes |
| Banco de dados | Exposição, alteração ou perda de informações |
| Disponibilidade | Interrupção das operações da plataforma |

## 6. Pontos de interação

Os pontos de interação são locais pelos quais pessoas ou serviços enviam e recebem informações.

### 6.1 Página pública do evento

A página pública permite:

- consultar informações;
- enviar formulários de inscrição;
- realizar check-in.

Por ser acessível sem autenticação, esse ponto pode receber solicitações automáticas, dados inválidos ou tentativas de manipulação.

### 6.2 Tela de autenticação

A tela de autenticação é utilizada por organizadores, avaliadores e administradores.

Nesse ponto são informadas as credenciais de acesso. Tentativas repetidas de login ou uso de credenciais roubadas podem comprometer contas internas.

### 6.3 Área autenticada

A área autenticada permite realizar operações de organização, avaliação e administração.

O sistema deve verificar o perfil e o vínculo do usuário antes de permitir qualquer operação.

### 6.4 Solicitação e aprovação de organizadores

O organizador poderá solicitar a associação de outro organizador ao evento. A solicitação deverá permanecer pendente até ser analisada por um administrador.

O sistema somente deverá conceder o acesso após a aprovação administrativa.

### 6.5 API REST

A API recebe as solicitações da aplicação web e executa as regras de negócio.

Ela deve:

- validar os dados recebidos;
- autenticar usuários internos;
- verificar permissões;
- verificar o vínculo com o evento;
- impedir acesso a recursos de outros eventos;
- limitar solicitações abusivas.

### 6.6 Banco de dados

O banco de dados recebe operações de leitura e gravação realizadas pela API.

Ele não deve ser acessado diretamente pelo navegador ou pelos usuários.

### 6.7 Códigos de check-in

Os códigos ou identificadores são utilizados para confirmar a presença.

Eles precisam ser protegidos contra:

- cópia;
- compartilhamento;
- reutilização;
- adivinhação;
- uso fora do período permitido.

### 6.8 Serviço de e-mail

O serviço de e-mail poderá receber informações necessárias para enviar:

- convites;
- confirmações de inscrição;
- avisos;
- notificações.

Somente os dados necessários devem ser encaminhados ao serviço.

## 7. Resumo dos pontos de interação

| Ponto de interação | Informações envolvidas | Principal preocupação |
|---|---|---|
| Página pública | Informações do evento, inscrições e check-in | Envio de dados inválidos ou solicitações automatizadas |
| Autenticação | E-mail, senha, token e sessão | Roubo ou tentativa de descoberta de credenciais |
| Área autenticada | Eventos, inscrições, avaliações e usuários | Acesso a operações sem permissão |
| Associação de organizadores | Solicitação, aprovação e vínculo com o evento | Aprovação indevida ou concessão de acesso sem autorização |
| API REST | Solicitações e respostas da aplicação | Manipulação de parâmetros e falhas de autorização |
| Banco de dados | Dados operacionais e configurações | Exposição, alteração ou exclusão de informações |
| Código de check-in | Identificador e registro de presença | Cópia, reutilização ou compartilhamento |
| Serviço de e-mail | Convites e notificações | Exposição de informações ou tokens |
