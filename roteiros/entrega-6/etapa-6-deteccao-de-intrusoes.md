# Etapa 6 — Monitoramento e Detecção de Intrusões

## O que é detecção de intrusões

Detecção de intrusões é o processo de observar o comportamento de um sistema em operação para identificar sinais de que algo suspeito ou malicioso está acontecendo — por exemplo, tentativas de acesso indevido, uso anormal de funcionalidades ou volumes incomuns de requisições. Diferente de um controle preventivo, a detecção não impede o ataque por si só: ela permite que a equipe perceba que ele está ocorrendo (ou já ocorreu) e reaja a tempo de reduzir o impacto.

## Diferença entre prevenir e detectar

**Prevenir** significa impedir que o abuso aconteça, por meio de controles aplicados antes ou durante a ação — validação de entrada, autenticação multifator, verificação de permissões, limites de requisição. Um controle preventivo bem implementado bloqueia a tentativa antes que ela produza efeito.

**Detectar** significa perceber que uma tentativa de abuso ocorreu ou está ocorrendo, mesmo que a prevenção falhe ou não exista para aquele caso específico. A detecção depende de registrar eventos relevantes (logs) e de regras que reconheçam padrões suspeitos nesses registros.

As duas abordagens são complementares: a prevenção reduz a chance de sucesso do ataque, e a detecção garante que, se algo passar despercebido pelos controles preventivos, a equipe ainda tenha a chance de identificar e responder ao incidente.

## Eventos que deveriam ser registrados

Com base nos riscos priorizados na Etapa 2, os seguintes eventos deveriam gerar registros (logs) no BAITA Eventos:

- tentativas de autenticação, bem-sucedidas e malsucedidas, com data, hora, conta e origem (IP);
- alterações em eventos, atividades, permissões e associações de organizadores;
- registros e alterações de avaliações, notas e pareceres;
- inscrições e confirmações de check-in, incluindo a origem da requisição;
- requisições rejeitadas por falha de autorização (usuário tentando acessar recurso sem vínculo);
- volume de requisições por conta, por IP e por rota, em intervalos de tempo curtos;
- conteúdo enviado em campos de texto livre (nome, descrição, comentários) antes de ser armazenado;
- tempo de resposta e taxa de erro da API REST e do MongoDB.

## Regras de detecção

As três regras a seguir foram definidas com base nos riscos de maior prioridade identificados na Etapa 2 (seção 8.6.2): R09, R01 e R05.

### Regra 1 — Indisponibilidade da plataforma por sobrecarga

| Campo | Descrição |
|---|---|
| Risco observado | R09 — Indisponibilidade da plataforma (prioridade 1 da Etapa 2), relacionado ao caso de abuso CA09 |
| Fonte de dados | Logs de requisições da API REST (volume por IP, por conta e por rota) e métricas de tempo de resposta da API e do MongoDB |
| Condição de alerta | Mais de 50 requisições por minuto originadas do mesmo IP para uma mesma rota, ou aumento súbito no tempo de resposta médio da API/MongoDB acima de um limite definido (ex.: 3x a média do dia) |
| Resposta inicial | Limitar temporariamente novas requisições da origem identificada, alertar a equipe técnica responsável pela infraestrutura, e verificar se o MongoDB ou a comunicação com o serviço de e-mail estão sendo afetados |

### Regra 2 — Comprometimento de conta autenticada

| Campo | Descrição |
|---|---|
| Risco observado | R01 — Comprometimento de contas autenticadas (prioridade 2 da Etapa 2), relacionado ao caso de abuso CA01 |
| Fonte de dados | Logs de autenticação (conta, resultado, origem, dispositivo) |
| Condição de alerta | Mais de 5 tentativas de login malsucedidas para a mesma conta em menos de 5 minutos, ou login bem-sucedido a partir de um IP ou dispositivo nunca utilizado antes por aquela conta |
| Resposta inicial | Bloquear temporariamente novas tentativas para a conta, notificar o usuário por e-mail sobre o acesso ou tentativa incomum, e registrar o evento para análise da equipe responsável |

### Regra 3 — Inserção de conteúdo malicioso em campos públicos ou autenticados

| Campo | Descrição |
|---|---|
| Risco observado | R05 — Inserção de conteúdo malicioso (prioridade 4 da Etapa 2), relacionado ao caso de abuso CA05 |
| Fonte de dados | Logs de requisições da API (conteúdo enviado nos campos de inscrição, descrição de evento e comentários de avaliação) |
| Condição de alerta | Campos de texto livre contendo padrões típicos de script (ex.: `<script`, `javascript:`, `onerror=`) enviados em requisições de inscrição, criação de evento ou avaliação |
| Resposta inicial | Rejeitar a requisição, registrar o conteúdo suspeito e a origem para análise, e alertar a equipe responsável caso o padrão se repita a partir da mesma origem |

## O que deve acontecer depois de um alerta

Após um alerta ser gerado, a equipe responsável deve:

1. verificar o registro do evento para confirmar se representa uma ameaça real ou um falso positivo;
2. avaliar o impacto já causado (ex.: quantas tentativas ocorreram, se houve sucesso, quantos usuários foram afetados);
3. aplicar a resposta inicial correspondente à regra (bloqueio temporário, rejeição da requisição, limitação de taxa);
4. comunicar o incidente à equipe técnica responsável quando o alerta indicar um caso confirmado, e não apenas um comportamento isolado;
5. registrar o incidente e a resposta aplicada, para consulta em análises futuras e para retroalimentar os critérios de detecção, caso as regras precisem ser ajustadas.
