# Priorização dos riscos

A priorização foi realizada considerando a pontuação obtida pela multiplicação entre probabilidade e impacto, em conjunto com o contexto da plataforma BAITA Eventos. Também foram analisados os ativos afetados, a quantidade de usuários potencialmente atingidos, a possibilidade de recuperação, a urgência do tratamento e as dependências entre os componentes.

A pontuação foi utilizada como referência inicial, mas não como único critério. Por esse motivo, a ordem de prioridade não segue necessariamente uma organização estritamente decrescente das pontuações. Um risco com pontuação menor pode receber prioridade superior quando afetar componentes centrais, possuir maior abrangência ou facilitar a ocorrência de outros riscos.

### Critérios complementares de priorização

Foram adotados os seguintes critérios complementares:

- **Ativos afetados:** importância das contas, dados, funcionalidades e componentes envolvidos;
- **Abrangência:** quantidade e tipos de usuários que podem ser afetados;
- **Recuperação:** dificuldade de restaurar dados, serviços ou a confiança nas informações;
- **Urgência:** necessidade de tratamento antes que o risco seja explorado ou provoque consequências relevantes;
- **Dependências:** influência do componente afetado sobre outras partes da plataforma;
- **Efeito acumulado:** possibilidade de o risco facilitar ou ampliar a exploração de outros riscos.

###  Ordem de prioridade

| Prioridade | Risco | Pontuação | Classificação | Justificativa da posição |
|---:|---|---:|---|---|
| 1 | R09 — Indisponibilidade da plataforma | 16 | Crítico | Possui a maior pontuação e pode afetar simultaneamente participantes, organizadores, avaliadores e administradores. A API REST e o MongoDB são componentes centrais, e sua indisponibilidade pode interromper inscrições, check-ins, avaliações e operações administrativas. |
| 2 | R01 — Comprometimento de contas autenticadas | 12 | Crítico | O comprometimento de uma conta pode permitir que o atacante execute ações em nome de um usuário legítimo. Contas de organizadores e administradores possuem acesso a operações relevantes e podem facilitar a exploração de outros riscos. |
| 3 | R02 — Execução de operações sem autorização | 12 | Crítico | Pode resultar em alterações administrativas e de gestão realizadas por usuários sem permissão. O risco afeta diretamente o controle de acesso e pode comprometer diversas funcionalidades da plataforma. |
| 4 | R05 — Inserção de conteúdo malicioso | 12 | Crítico | Conteúdo malicioso pode atingir diferentes usuários e comprometer sessões, informações e funcionalidades. O risco pode estar presente em diferentes campos de entrada e produzir efeitos além da operação inicialmente explorada. |
| 5 | R04 — Fraudes em inscrições e registros de presença | 12 | Crítico | Apresenta alta probabilidade porque envolve funcionalidades públicas que não exigem uma conta do participante. Pode comprometer a integridade das inscrições e dos registros de presença, embora seus efeitos normalmente permaneçam mais limitados ao evento afetado. |
| 6 | R06 — Operações indevidas por meio da API REST | 8 | Alto | A API REST concentra o processamento das requisições e das regras de negócio. Sua exploração pode afetar diferentes funcionalidades, embora dependa de entradas inseguras ou condições específicas. |
| 7 | R07 — Comprometimento da comunicação entre a API REST e o MongoDB | 8 | Alto | Pode causar exposição, alteração ou redirecionamento dos dados armazenados. O impacto é elevado devido à importância do banco de dados, mas a exploração depende de acesso ou posicionamento específico na comunicação interna. |
| 8 | R08 — Comprometimento de mensagens e tokens de e-mail | 8 | Alto | Pode permitir o redirecionamento de mensagens, a exposição de informações sensíveis, o comprometimento de contas ou a interrupção das comunicações. Sua exploração depende do serviço de e-mail ou do fluxo de envio. |
| 9 | R03 — Manipulação de avaliações | 9 | Alto | Embora possua pontuação superior à de R06, R07 e R08, seus efeitos estão principalmente concentrados no processo de avaliação. A alteração de notas, comentários, pareceres ou classificações compromete a integridade e a confiança nos resultados, mas apresenta abrangência menor que os riscos posicionados anteriormente. |
| 10 | R10 — Execução de operações por CSRF | 8 | Alto | Depende de um usuário autenticado acessar ou interagir com conteúdo preparado pelo atacante. Pode causar operações indevidas, mas exige condições específicas e pode ser reduzido com mecanismos conhecidos de proteção de sessão e validação de requisições. |

### Justificativa geral da ordem

O R09 ocupa a primeira posição porque combina probabilidade alta, impacto muito alto e possibilidade de interrupção generalizada da plataforma. Em seguida aparecem R01 e R02, pois o comprometimento de contas e as falhas de autorização podem permitir diferentes operações indevidas e facilitar outros ataques.

R05 e R04 completam o grupo de riscos críticos. O R05 possui maior potencial de propagação entre usuários e funcionalidades, enquanto o R04 apresenta alta probabilidade devido à exposição pública das funcionalidades de inscrição e check-in, mas possui consequências mais delimitadas.

Entre os riscos classificados como altos, R06 e R07 recebem maior prioridade por envolverem a API REST e o MongoDB, componentes dos quais dependem várias funcionalidades. O R08 aparece em seguida devido à possibilidade de comprometimento de mensagens, tokens e contas por meio do serviço de e-mail.

O R03 possui pontuação 9, enquanto R06, R07 e R08 possuem pontuação 8. Mesmo assim, R03 foi posicionado depois desses riscos porque seu impacto está mais concentrado no processo de avaliação. R06, R07 e R08 envolvem componentes ou integrações que sustentam diferentes funcionalidades e podem afetar uma quantidade maior de operações.

Essa diferença foi definida de forma intencional e demonstra que a pontuação foi utilizada como apoio à decisão, sem substituir a análise do contexto, dos ativos afetados e das dependências da plataforma.

O R10 encerra a ordem porque sua exploração depende de condições mais específicas: o usuário precisa estar autenticado, ser induzido a interagir com conteúdo preparado pelo atacante e a plataforma precisa não possuir proteção adequada contra CSRF.

A ordem apresentada representa uma prioridade inicial e poderá ser revista caso sejam identificadas novas vulnerabilidades, mudanças na arquitetura ou evidências sobre a frequência e a gravidade dos eventos.
