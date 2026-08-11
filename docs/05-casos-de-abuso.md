# 5. Casos de Abuso

Os casos de abuso apresentados a seguir foram elaborados a partir das ameaças identificadas na modelagem STRIDE (seção 4.5), com o objetivo de representar situações concretas em que um agente malicioso — externo ou interno à plataforma — poderia explorar o BAITA Eventos para obter acesso indevido, adulterar informações, comprometer a disponibilidade do sistema ou dificultar a responsabilização por suas ações.

Cada caso de abuso está diretamente relacionado a uma ou mais ameaças da modelagem STRIDE e apresenta:

- o ator malicioso ou agente envolvido;
- o objetivo do abuso;
- as condições necessárias para que o abuso ocorra;
- a sequência de ações realizadas pelo agente;
- o impacto esperado sobre o sistema, os usuários ou os dados;
- a relação com a(s) categoria(s) do STRIDE correspondente(s).

Os casos foram organizados de acordo com as seis categorias do STRIDE, agrupando ameaças relacionadas sempre que representavam a mesma dinâmica de abuso, de modo a evitar repetição excessiva sem perder a rastreabilidade em relação às ameaças originais.

## 5.1 Casos de abuso — Spoofing

### CA-S01 — Furto de credenciais de organizador por phishing

- **Ameaças STRIDE relacionadas:** T10
- **Ator malicioso:** atacante externo, sem vínculo com a plataforma
- **Objetivo do abuso:** acessar a área autenticada assumindo a identidade de um organizador legítimo
- **Condições necessárias:** o organizador possui e-mail e senha que podem ser obtidos por phishing, vazamento em outro serviço ou reutilização de senha; a plataforma não exige verificação adicional além de e-mail e senha no login
- **Sequência de ações:**
  1. O atacante envia uma mensagem fraudulenta simulando uma comunicação oficial do BAITA Eventos.
  2. O organizador informa e-mail e senha em uma página falsa controlada pelo atacante.
  3. O atacante utiliza as credenciais obtidas para autenticar-se na plataforma real.
  4. O atacante acessa a área autenticada e realiza operações em nome do organizador.
- **Impacto esperado:** alteração indevida de eventos, atividades, inscrições e configurações; fraude realizada em nome do organizador legítimo.
- **Relação com STRIDE:** Spoofing (falsificação de identidade do organizador).

### CA-S02 — Uso indevido de conta de avaliador comprometida

- **Ameaças STRIDE relacionadas:** T13
- **Ator malicioso:** atacante externo ou pessoa com acesso não autorizado às credenciais
- **Objetivo do abuso:** consultar inscrições e registrar avaliações em nome de um avaliador legítimo
- **Condições necessárias:** as credenciais do avaliador são comprometidas (ex.: senha fraca, dispositivo compartilhado sem logout, credencial reaproveitada de outro sistema)
- **Sequência de ações:**
  1. O atacante obtém as credenciais de um avaliador.
  2. O atacante acessa a área autenticada como se fosse o avaliador.
  3. O atacante consulta inscrições e dados de participantes aos quais o avaliador tem acesso.
  4. O atacante registra ou altera notas e observações das atividades avaliadas.
- **Impacto esperado:** exposição de dados de participantes e alteração indevida de avaliações, prejudicando resultados e classificações.
- **Relação com STRIDE:** Spoofing (falsificação de identidade do avaliador).

### CA-S03 — Falsificação de identidade de administrador

- **Ameaças STRIDE relacionadas:** T16
- **Ator malicioso:** atacante externo com alto grau de motivação e conhecimento sobre a plataforma
- **Objetivo do abuso:** obter controle administrativo total sobre usuários, permissões e associações de organizadores
- **Condições necessárias:** comprometimento da conta de administrador (credenciais fracas, ausência de autenticação multifator, engenharia social direcionada)
- **Sequência de ações:**
  1. O atacante identifica ou engana um administrador para obter suas credenciais.
  2. O atacante autentica-se na plataforma como administrador.
  3. O atacante concede permissões administrativas a uma conta sob seu controle.
  4. O atacante aprova solicitações de associação de organizadores fraudulentos ou altera usuários existentes.
- **Impacto esperado:** comprometimento total da governança da plataforma, com possibilidade de manter acesso persistente mesmo após a detecção do incidente original.
- **Relação com STRIDE:** Spoofing (falsificação de identidade do administrador) combinado a Elevation of Privilege.

### CA-S04 — Serviço de e-mail falso recebendo notificações da API

- **Ameaças STRIDE relacionadas:** T98
- **Ator malicioso:** atacante capaz de interceptar ou redirecionar tráfego de rede/DNS entre a API e o serviço de e-mail
- **Objetivo do abuso:** capturar convites, confirmações e notificações destinados a participantes e usuários internos
- **Condições necessárias:** a integração entre a API REST e o serviço de e-mail não valida adequadamente a identidade do destinatário do lado do serviço externo
- **Sequência de ações:**
  1. O atacante posiciona um serviço falso que se apresenta como o serviço de e-mail legítimo.
  2. A API REST envia convites, confirmações e notificações para o serviço falso.
  3. O atacante captura tokens de confirmação de inscrição, convites de organizadores e demais informações sensíveis.
  4. O atacante utiliza os dados capturados para se inscrever em nome de terceiros ou obter acesso indevido.
- **Impacto esperado:** exposição de dados pessoais e tokens; possibilidade de uso fraudulento de convites de associação de organizadores.
- **Relação com STRIDE:** Spoofing (falsificação do serviço externo de e-mail).

### CA-S05 — Componente falso entre a API REST e o MongoDB

- **Ameaças STRIDE relacionadas:** T112, T113, T121
- **Ator malicioso:** atacante com acesso à infraestrutura ou à rede interna da plataforma
- **Objetivo do abuso:** interpor-se na comunicação entre a API REST e o banco de dados para ler ou adulterar informações
- **Condições necessárias:** ausência de autenticação mútua entre a API e o banco de dados, permitindo que um componente não autorizado se apresente como um dos dois lados legítimos
- **Sequência de ações:**
  1. O atacante posiciona um componente controlado por ele na comunicação entre a API REST e o MongoDB.
  2. A API REST envia gravações para o componente falso, acreditando se comunicar com o banco legítimo (T113).
  3. Alternativamente, o componente falso se apresenta ao banco como a API legítima e realiza operações não autorizadas (T112).
  4. O componente falso também pode devolver à API dados adulterados, fazendo-se passar pelo MongoDB legítimo (T121).
- **Impacto esperado:** exposição de dados enviados para infraestrutura controlada pelo atacante e alteração indevida das informações exibidas aos usuários (inscrições, avaliações, resultados).
- **Relação com STRIDE:** Spoofing (falsificação mútua entre componentes de backend).

## 5.2 Casos de abuso — Tampering

### CA-T01 — Injeção de conteúdo malicioso via formulário público

- **Ameaças STRIDE relacionadas:** T01
- **Ator malicioso:** atacante externo, sem necessidade de conta na plataforma
- **Objetivo do abuso:** executar scripts maliciosos no navegador de outros usuários que visualizam os dados enviados
- **Condições necessárias:** o formulário de inscrição da página pública não trata ou não sanitiza adequadamente o conteúdo enviado pelo participante antes de exibi-lo (ex.: para organizadores e avaliadores)
- **Sequência de ações:**
  1. O atacante preenche o formulário de inscrição com conteúdo malicioso em campos de texto livre (ex.: nome ou respostas personalizadas).
  2. A inscrição é registrada normalmente pela plataforma.
  3. Um organizador ou avaliador acessa a lista de inscrições na área autenticada.
  4. O conteúdo malicioso é exibido sem tratamento e é executado no navegador da vítima.
- **Impacto esperado:** comprometimento da sessão do organizador ou avaliador, podendo levar ao furto de credenciais de sessão ou à execução de ações não autorizadas em nome da vítima.
- **Relação com STRIDE:** Tampering (alteração indevida do conteúdo exibido).

### CA-T02 — Injeção de conteúdo malicioso na área autenticada

- **Ameaças STRIDE relacionadas:** T11
- **Ator malicioso:** usuário interno mal-intencionado (organizador ou avaliador) ou atacante que já comprometeu uma conta interna
- **Objetivo do abuso:** comprometer as sessões de outros usuários internos que acessam a mesma área autenticada
- **Condições necessárias:** campos preenchidos por organizadores ou avaliadores (ex.: descrições de eventos, comentários de avaliação) não são tratados antes de serem exibidos a outros usuários internos
- **Sequência de ações:**
  1. O atacante, já autenticado como organizador ou avaliador, insere conteúdo malicioso em um campo da área autenticada.
  2. O conteúdo é armazenado sem tratamento adequado.
  3. Outro usuário interno (ex.: administrador) acessa a tela onde o conteúdo é exibido.
  4. O script malicioso é executado no navegador da vítima.
- **Impacto esperado:** comprometimento de sessões de organizadores, avaliadores ou administradores, possibilitando escalonamento de acesso dentro da plataforma.
- **Relação com STRIDE:** Tampering (alteração indevida do conteúdo exibido na área interna).

### CA-T03 — Alteração direta de dados no banco de dados

- **Ameaças STRIDE relacionadas:** T114
- **Ator malicioso:** atacante que obtém acesso não autorizado à infraestrutura do banco de dados
- **Objetivo do abuso:** corromper ou adulterar diretamente informações armazenadas, contornando as regras de negócio aplicadas pela API
- **Condições necessárias:** o banco de dados está acessível por algum caminho fora da API REST (ex.: credenciais de infraestrutura mal protegidas, backup exposto, porta de acesso indevidamente aberta)
- **Sequência de ações:**
  1. O atacante obtém acesso não autorizado ao MongoDB (ex.: credenciais vazadas, configuração insegura).
  2. O atacante altera diretamente registros de inscrições, check-ins, avaliações ou configurações.
  3. A alteração não passa pelas validações normalmente aplicadas pela API REST.
  4. Usuários e organizadores passam a operar sobre dados incorretos sem perceber a adulteração.
- **Impacto esperado:** inscrições, check-ins ou resultados incorretos; perda de integridade dos dados de todo um evento, com difícil identificação da origem do problema.
- **Relação com STRIDE:** Tampering (alteração indevida de dados armazenados).

## 5.3 Casos de abuso — Repudiation

### CA-R01 — Inscrições e check-ins realizados sem rastreabilidade suficiente

- **Ameaças STRIDE relacionadas:** T38, T54
- **Ator malicioso:** participante mal-intencionado ou atacante automatizado
- **Objetivo do abuso:** realizar inscrições, check-ins ou outras operações e, posteriormente, negar tê-las realizado, ou impedir que a origem da operação seja identificada
- **Condições necessárias:** a API REST não registra de forma suficiente a origem, o momento e o resultado de operações públicas e autenticadas
- **Sequência de ações:**
  1. O agente realiza uma inscrição, um check-in ou outra operação suportada pela API.
  2. A operação é processada, mas os registros mantidos não permitem identificar com segurança quem a realizou ou quando.
  3. Um problema é identificado posteriormente (ex.: vaga ocupada indevidamente, check-in duplicado).
  4. Não é possível comprovar com certeza a autoria da operação, dificultando a investigação.
- **Impacto esperado:** perda de rastreabilidade de inscrições e confirmações de presença; dificuldade para investigar fraudes ou erros e para responsabilizar o agente correto.
- **Relação com STRIDE:** Repudiation (negação de ações por ausência de registro adequado).

### CA-R02 — Organizador nega alteração realizada em um evento

- **Ameaças STRIDE relacionadas:** T60
- **Ator malicioso:** organizador com acesso legítimo à plataforma
- **Objetivo do abuso:** realizar uma alteração indevida ou prejudicial em um evento e, posteriormente, negar ter sido o responsável
- **Condições necessárias:** o sistema não mantém um histórico de alterações suficientemente detalhado (quem alterou, o quê, e quando) associado às operações sobre eventos e atividades
- **Sequência de ações:**
  1. O organizador altera informações de um evento ou de uma atividade (ex.: reduz vagas, altera período de inscrição).
  2. A alteração causa prejuízo a participantes ou a outros organizadores.
  3. Quando questionado, o organizador nega ter realizado a alteração.
  4. Sem um registro de auditoria confiável, não é possível comprovar a autoria da mudança.
- **Impacto esperado:** perda de rastreabilidade das alterações realizadas nos eventos; dificuldade para responsabilizar o organizador e resolver o conflito com participantes prejudicados.
- **Relação com STRIDE:** Repudiation (negação de autoria de uma alteração).

### CA-R03 — Avaliador nega ter registrado ou alterado uma nota

- **Ameaças STRIDE relacionadas:** T66
- **Ator malicioso:** avaliador com acesso legítimo às atividades sob sua responsabilidade
- **Objetivo do abuso:** registrar ou alterar uma nota de forma indevida (ex.: favorecendo ou prejudicando um participante) e negar posteriormente a autoria
- **Condições necessárias:** o sistema não associa de forma confiável cada registro ou alteração de nota ao avaliador responsável, nem mantém histórico das alterações
- **Sequência de ações:**
  1. O avaliador registra ou altera uma nota de forma indevida.
  2. O resultado da avaliação é utilizado para gerar classificações ou premiações.
  3. Um participante contesta o resultado.
  4. O avaliador nega ter realizado o registro ou a alteração, e não há evidência suficiente para confirmar ou refutar a negativa.
- **Impacto esperado:** perda de rastreabilidade das avaliações; contestação de resultados sem possibilidade de resolução; perda de confiança no processo avaliativo.
- **Relação com STRIDE:** Repudiation (negação de autoria de um registro de avaliação).

### CA-R04 — Operação administrativa ou de banco de dados sem trilha de auditoria

- **Ameaças STRIDE relacionadas:** T72, T115
- **Ator malicioso:** administrador com acesso legítimo, ou atacante que já obteve acesso administrativo/de infraestrutura (ver CA-S03)
- **Objetivo do abuso:** realizar uma operação sensível (alteração de usuários, permissões, associações ou gravações diretas no banco) e negar posteriormente sua realização, ou impedir que ela seja atribuída a um responsável
- **Condições necessárias:** operações administrativas e gravações no banco de dados não geram registros de auditoria completos e protegidos contra alteração
- **Sequência de ações:**
  1. O agente realiza uma operação sensível (ex.: concede permissões, aprova uma associação de organizador, altera diretamente um registro no MongoDB).
  2. A operação não é registrada de forma completa ou os registros gerados podem ser alterados posteriormente.
  3. A operação causa um problema identificado por outro usuário ou pela equipe responsável.
  4. O agente nega ter realizado a operação, e a ausência de uma trilha de auditoria confiável impede a comprovação.
- **Impacto esperado:** perda de rastreabilidade de alterações administrativas críticas; dificuldade para investigar incidentes de segurança e responsabilizar o agente correto.
- **Relação com STRIDE:** Repudiation (negação de operações administrativas e de banco de dados).

## 5.4 Casos de abuso — Information Disclosure

### CA-I01 - Vazamento de dados dos participantes
- **Ameaças STRIDE relacionadas:** T10
- **Ator:** participante mal-intencionado ou usuário autenticado sem autorização.
- **Objetivo:** obter dados pessoais de participantes de eventos aos quais não possui permissão de acesso.
- **Condições necessárias:** falha de autorização permite consultar inscrições ou enumerar participantes de outro evento.
- **Sequência de ações:**
  1. O atacante acessa o sistema utilizando uma conta válida.
  2. Identifica uma funcionalidade de consulta de inscrições ou participantes.
  3. Altera ou utiliza um identificador de evento diferente daquele ao qual possui acesso.
  4. Envia a requisição para a API.
  5. O sistema não verifica adequadamente o vínculo do usuário com o evento.
  6. A API retorna dados de participantes do evento consultado.
- **Impacto esperado:** exposição de dados pessoais, violação de privacidade e possibilidade de utilização indevida das informações obtidas.
- **Relação com STRIDE:** Information Disclosure (Mostrar informações para quem não deve ver.)

### CA-I02 - Exposição de informações em logs e mensagens de erro
-** Ameaças STRIDE relacionadas:** T11.
- **Ator:** usuário mal-intencionado ou pessoa que obtém acesso aos logs da aplicação.
- **Objetivo:** obter informações sensíveis ou detalhes internos do sistema por meio de mensagens de erro e registros da aplicação.
- **Condições necessárias:** logs ou mensagens de erro podem registrar tokens, credenciais, respostas de formulários ou detalhes internos sem mascaramento ou minimização adequada.
- **Sequência de ações:**
1. O atacante realiza uma operação que provoca um erro na aplicação.
2. O sistema apresenta uma mensagem de erro contendo informações internas ou dados sensíveis.
3. As informações também podem ser registradas nos logs da aplicação.
4. O atacante obtém acesso às informações expostas.
5. O atacante utiliza os dados obtidos para facilitar novos ataques ou acessar informações protegidas.
6. **Impacto esperado:** exposição de dados pessoais, credenciais, tokens ou informações internas da aplicação, facilitando ataques posteriores e comprometendo a segurança do sistema.
- **Relação com STRIDE:** Information Disclosure (Mostrar informações para quem não deve ver.)
