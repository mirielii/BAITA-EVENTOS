# Descrição do Sistema

## 1. Visão geral

O **BAITA EVENTOS** é uma plataforma para gestão de eventos acadêmicos, institucionais, culturais e corporativos.

A plataforma pretende reunir, em um único ambiente, diferentes tarefas relacionadas à organização de eventos, como:

- criação de eventos;
- configuração de atividades;
- inscrição de participantes;
- controle de vagas;
- confirmação de presença;
- avaliação de atividades;
- acompanhamento dos resultados.

Um evento poderá possuir diferentes atividades, como palestras, oficinas, minicursos, competições e apresentações.

Cada atividade poderá ter suas próprias regras de inscrição, quantidade de vagas, forma de check-in e critérios de avaliação.

## 2. Problema que o sistema resolve

A organização de eventos pode exigir o uso de várias ferramentas, como planilhas, formulários e aplicativos de mensagens.

A utilização de ferramentas separadas pode causar:

- perda ou duplicidade de informações;
- dificuldade para controlar as inscrições;
- registros de presença incorretos;
- retrabalho para os organizadores;
- dificuldade para acompanhar as avaliações;
- falta de controle sobre o acesso às informações.

O BAITA EVENTOS busca resolver esse problema centralizando as principais operações em uma única plataforma.

## 3. Usuários do sistema

A plataforma será utilizada por quatro tipos principais de usuários:

- participantes;
- organizadores;
- avaliadores;
- administradores.

Os participantes utilizarão as páginas públicas para consultar eventos, realizar inscrições e confirmar presença.

Os organizadores, avaliadores e administradores utilizarão uma área autenticada, com funcionalidades diferentes de acordo com suas responsabilidades.

Os perfis, permissões e responsabilidades de cada usuário serão detalhados no documento [Usuários, ativos e pontos de interação](02-usuarios-ativos.md).

## 4. Principais funcionalidades

A plataforma terá as seguintes funcionalidades:

- cadastrar e editar eventos;
- criar atividades dentro de um evento;
- publicar informações sobre eventos;
- configurar formulários de inscrição;
- definir períodos de inscrição;
- controlar vagas disponíveis;
- registrar inscrições;
- realizar o check-in dos participantes;
- registrar avaliações;
- calcular resultados e classificações;
- enviar confirmações e notificações por e-mail;
- gerenciar usuários e permissões;
- registrar operações importantes para auditoria.

## 5. Informações tratadas pelo sistema

A plataforma poderá armazenar ou transmitir:

- informações dos eventos;
- informações das atividades;
- nomes e e-mails dos usuários;
- dados preenchidos nos formulários;
- inscrições dos participantes;
- quantidade de vagas;
- registros de presença;
- códigos de check-in;
- notas e comentários de avaliação;
- resultados e classificações;
- convites e notificações por e-mail;
- registros de acesso e operações administrativas.

Os dados solicitados poderão variar de acordo com o evento e com a configuração de cada atividade.

## 6. Recursos que precisam ser protegidos

Os principais recursos que precisam ser protegidos são:

- contas e credenciais dos usuários;
- dados pessoais dos participantes;
- inscrições;
- registros de presença;
- códigos de check-in;
- avaliações e resultados;
- configurações dos eventos;
- permissões dos usuários;
- banco de dados;
- disponibilidade da plataforma.

O acesso ou a alteração indevida desses recursos pode causar fraude, exposição de dados pessoais, resultados incorretos e interrupção das atividades.

A identificação detalhada dos ativos e pontos de interação será apresentada no documento [Usuários, ativos e pontos de interação](02-usuarios-ativos.md).
