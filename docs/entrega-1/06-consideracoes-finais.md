# Considerações Finais

## 1 Síntese da análise

A Etapa 1 teve como objetivo iniciar a análise de segurança do **BAITA EVENTOS** antes da implementação, compreendendo o funcionamento da plataforma e identificando as formas pelas quais ela poderia ser explorada.

A análise partiu da descrição do sistema ([01-descricao-do-sistema](01-descricao-do-sistema.md)), dos usuários e ativos ([02-usuarios-ativos](02-usuarios-ativos.md)) e da visão da arquitetura ([03-arquitetura](03-arquitetura.md)). Sobre essa base, foi realizada a modelagem de ameaças com o **STRIDE** ([04-modelagem-stride](04-modelagem-stride.md)), que selecionou 30 ameaças contextualizadas a partir das 111 geradas automaticamente pelo Microsoft Threat Modeling Tool, e foram elaborados os casos de abuso ([05-casos-de-abuso](05-casos-de-abuso.md)), que representam 10 situações concretas de uso malicioso da plataforma.

## 2 Ameaças consideradas mais preocupantes

Entre as 30 ameaças selecionadas, destacam-se como mais preocupantes:

- **comprometimento da conta do administrador**, pois permite gerenciar usuários, permissões e associações de forma indevida, afetando toda a plataforma (categoria Spoofing);
- **falhas de autorização entre organizadores, avaliadores e administradores**, que permitem executar operações sem o perfil ou vínculo necessário com o evento (categoria Elevation of Privilege);
- **acesso indevido aos dados pessoais das inscrições**, por falha de autorização, expõe informações de todos os participantes de um evento (categoria Information Disclosure);
- **alteração de notas, avaliações e resultados**, que compromete a credibilidade do processo de avaliação e pode ser negada pela ausência de auditoria (categorias Tampering e Repudiation);
- **execução de código ou comandos indevidos na API**, que parte de dados maliciosos em requisições autenticadas e pode comprometer o servidor (categoria Elevation of Privilege);
- **indisponibilidade durante períodos de inscrição, check-in e avaliação**, momente críticos para o funcionamento do evento (categoria Denial of Service).

Essas ameaças merecem atenção prioritária porque podem afetar simultaneamente a confidencialidade, a integridade, a disponibilidade e a rastreabilidade das operações da plataforma, atingindo participantes e usuários internos ao mesmo tempo.

## 3 Ativos mais importantes

Os ativos cujo comprometimento causaria maior prejuízo são:

- **contas e credenciais dos usuários internos** (organizadores, avaliadores e administradores);
- **dados pessoais fornecidos nas inscrições**, por serem protegidos e não possuírem vínculo com conta permanente;
- **inscrições e controle de vagas**, cuja alteração afeta a capacidade do evento e a confiança dos participantes;
- **registros e códigos de check-in**, pois permitem comprovar (ou falsificar) a presença;
- **avaliações, notas e resultados**, por definirem a classificação de atividades competitivas;
- **configurações dos eventos e atividades**, que controlam períodos, vagas e regras de inscrição;
- **registros de auditoria**, necessários para responsabilização e investigação;
- **banco de dados MongoDB e a disponibilidade da plataforma**, que sustentam todas as operações.

A perda, alteração ou indisponibilização desses ativos gera danos diretos aos usuários, prejuízo à organização do evento e perda de confiança na plataforma.

## 4 Abusos com maior impacto potencial

Embora todos os casos de abuso documentados sejam relevantes, alguns possuem potencial de dano mais amplo:

- **CA01 — Comprometimento de conta interna (furto de credenciais de organizador e falsificação de identidade de administrador):** o atacante assume a identidade de um usuário interno legítimo, podendo alterar eventos, inscrições, configurações e permissões em nome dele;
- **CA02 — Acesso a operações sem autorização (vazamento de dados dos participantes e acesso a funções administrativas):** expõe dados pessoais em massa, afetando todos os inscritos de um evento, e permite a usuários de menor privilégio executar operações administrativas;
- **CA06 — Execução de operações indevidas por meio da API REST (alteração direta de dados no banco):** corrompe a base de inscrições, avaliações e resultados, comprometendo a integridade de todo o sistema;
- **CA09 — Indisponibilidade da plataforma (sobrecarga das rotas públicas e API):** impede inscrições, consultas e check-ins em períodos críticos do evento.

Esses abusos combinam acesso indevido com alteração ou exposição de dados, generalizando o prejuízo para todos os usuários da plataforma.

## 5 Dificuldades encontradas

As principais dificuldades durante a análise foram:

- **volume da saída automática da ferramenta:** o Microsoft Threat Modeling Tool gerou 111 ameaças; foi necessário revisar manualmente cada uma para selecionar as 30 aplicáveis, exigindo conhecimento da arquitetura para decidir a manutenção ou exclusão;
- **colaboração concorrente:** os integrantes trabalharam em branches paralelas editando os mesmos documentos, o que exigiu cuidado na integração e na organização dos arquivos;
- **escopo da descrição sem implementação:** descrever o funcionamento e os fluxos de um sistema ainda não construído exigiu decisões sobre o que detalhar, evitando especificar requisitos completos;
- **avaliação da prioridade fornecida pela ferramenta:** todas as ameaças foram exportadas com prioridade `High`, o que não reflete o risco real; a relevância precisou ser analisada pelo grupo com base nos ativos e na arquitetura.

## 6 Medidas de proteção indicativas

Embora a proposição de soluções completas não seja objetivo desta etapa, as medidas a seguir são indicadas como direções de proteção para as ameaças mais relevantes:

- **autenticação multifator e reautenticação em operações sensíveis**, para reduzir o impacto do furto de credenciais (Spoofing);
- **validação e sanitização de entradas em todos os formulários**, públicos e autenticados, para reduzir a execução de scripts (Tampering/XSS);
- **verificação de autorização no servidor (API) em todas as operações**, por perfil e vínculo com o evento, não apenas ocultando opções na interface (Elevation of Privilege);
- **registro de auditoria imutável e associado ao usuário responsável**, com data, hora e resultado, para apoiar a rastreabilidade (Repudiation);
- **criptografia em trânsito e restrição de acesso ao banco**, mantendo o acesso apenas pela API (Information Disclosure);
- **limitação de requisições (rate limiting), controle de vagas e planos de contingência**, para reduzir a indisponibilidade (Denial of Service);
- **autenticação mútua com o serviço de e-mail e validação do destinatário**, reduzindo o risco de envio de convites e tokens para destinos controlados pelo atacante (Spoofing).

Na Etapa 2, as ameaças identificadas nesta análise serão transformadas em eventos de risco, avaliados por probabilidade e impacto, priorizados e tratados conforme as funções do NIST Cybersecurity Framework 2.0. A coerência entre os documentos desta etapa — ameaças, casos de abuso, ativos e arquitetura — será o ponto de partida para a construção do plano de tratamento de riscos.
