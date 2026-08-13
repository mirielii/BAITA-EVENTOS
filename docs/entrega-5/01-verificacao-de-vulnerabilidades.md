# Verificação de vulnerabilidades

Este documento registra uma única sessão de verificação de vulnerabilidades realizada em ambiente autorizado, as evidências obtidas, a análise de três achados e as respectivas propostas de correção.

## 1 Sistema ou ambiente testado

A verificação foi realizada exclusivamente no **OWASP Juice Shop**, uma aplicação deliberadamente vulnerável criada para fins educacionais e de treinamento em segurança.

A aplicação foi executada localmente por meio de um contêiner Docker e disponibilizada somente no endereço:

```text
http://127.0.0.1:3000
```

O endereço `127.0.0.1` corresponde ao próprio computador utilizado no teste. Portanto, a sessão não envolveu sistemas de terceiros nem aplicações sem autorização.

## 2 Ferramenta e configuração básica da sessão

A ferramenta utilizada foi o **OWASP ZAP 2.17.0** (Zed Attack Proxy), destinada a testes de segurança em aplicações web. Ela foi usada para identificar alertas relacionados a configurações e comportamentos potencialmente inseguros da aplicação executada localmente.

| Item | Configuração |
|---|---|
| Alvo da verificação | `http://127.0.0.1:3000` |
| Aplicação analisada | OWASP Juice Shop |
| Ferramenta | OWASP ZAP 2.17.0 |
| Tipo de verificação | Varredura automatizada com descoberta de recursos e verificação ativa |
| Escopo permitido | Somente recursos acessíveis a partir da aplicação local |
| Saída gerada | Relatório HTML e alertas da sessão |

A varredura foi limitada ao ambiente local autorizado. O ZAP realizou a descoberta de recursos acessíveis a partir da página inicial e executou verificações ativas nesse escopo.

## 3 Evidências da execução

A execução foi registrada por meio do relatório HTML exportado pelo OWASP ZAP e de capturas de tela dos alertas selecionados. Todos os arquivos estão em `evidencias/etapa-5/`.

O relatório completo está disponível em [relatorio-zap-juice-shop.html](../../evidencias/etapa-5/relatorio-zap-juice-shop.html).

A sessão identificou dois alertas de risco médio, um de risco baixo e três informativos. Os três achados analisados foram escolhidos entre os alertas de risco médio e baixo.

![Resumo dos alertas identificados pelo OWASP ZAP](../../evidencias/etapa-5/01-resumo-alertas-zap.png)

As evidências abaixo demonstram que a varredura foi realizada exclusivamente em `http://127.0.0.1:3000`, correspondente ao OWASP Juice Shop executado localmente.

### Evidências dos achados selecionados

**A01 — Content Security Policy (CSP) Header Not Set**

![Alerta sobre ausência do cabeçalho Content Security Policy](../../evidencias/etapa-5/02-alerta-csp-ausente.png)

**A02 — Cross-Domain Misconfiguration**

![Alerta sobre configuração CORS ampla](../../evidencias/etapa-5/03-alerta-cors.png)

**A03 — Timestamp Disclosure - Unix**

![Alerta sobre divulgação de timestamp](../../evidencias/etapa-5/04-alerta-timestamp.png)

## 4 Análise dos alertas e achados

### 4.1 A01 — Content Security Policy (CSP) Header Not Set

| Campo | Análise |
|---|---|
| ID | A01 |
| Alerta ou achado | Content Security Policy (CSP) Header Not Set |
| Nível indicado pelo ZAP | Médio |
| Evidência | O relatório do ZAP indicou a ausência do cabeçalho `Content-Security-Policy` nas respostas da aplicação, incluindo `http://127.0.0.1:3000`. |
| Possível impacto | A ausência de uma CSP reduz uma camada de defesa no navegador contra a execução de scripts não autorizados e ataques de injeção, como Cross-Site Scripting (XSS). Caso uma falha de injeção exista, o navegador terá menos restrições sobre as origens de conteúdo que podem ser carregadas ou executadas. |
| Relação com OWASP ou CWE | [CWE-693 — Protection Mechanism Failure](https://cwe.mitre.org/data/definitions/693.html) e [OWASP Content Security Policy Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html). |
| Correção proposta | Configurar o servidor para enviar um cabeçalho `Content-Security-Policy` restritivo, permitindo somente as origens necessárias para scripts, estilos, imagens e conexões. A política deve ser testada gradualmente para evitar o bloqueio de recursos legítimos da aplicação. |

A CSP não substitui a validação de entradas e a codificação segura das saídas. Ela funciona como uma camada adicional de proteção, reduzindo o impacto de possíveis falhas de injeção que afetem o navegador.

### 4.2 A02 — Cross-Domain Misconfiguration

| Campo | Análise |
|---|---|
| ID | A02 |
| Alerta ou achado | Cross-Domain Misconfiguration |
| Nível indicado pelo ZAP | Médio |
| Evidência | O relatório identificou o cabeçalho `Access-Control-Allow-Origin: *` nas respostas analisadas em `http://127.0.0.1:3000`. |
| Possível impacto | A configuração permite que páginas hospedadas em domínios externos realizem leituras de recursos públicos da aplicação. Caso o mesmo padrão seja aplicado de forma indevida a endpoints que tratem dados sensíveis, pode ampliar a exposição de informações ou facilitar o uso indevido de APIs. |
| Relação com OWASP ou CWE | [CWE-264 — Permissions, Privileges, and Access Controls](https://cwe.mitre.org/data/definitions/264.html) e OWASP Top 10 — Security Misconfiguration. |
| Correção proposta | Substituir o caractere curinga `*` por uma lista explícita de origens autorizadas. Endpoints autenticados não devem aceitar origens arbitrárias e devem revisar cuidadosamente o uso de credenciais em CORS. |

O alerta deve ser interpretado conforme o contexto. O relatório indica que navegadores não permitem que domínios arbitrários leiam respostas de APIs autenticadas apenas com essa configuração, o que reduz parte do risco. Ainda assim, a política CORS ampla não deve ser adotada por padrão em serviços que possam expor informações sensíveis.

### 4.3 A03 — Timestamp Disclosure - Unix

| Campo | Análise |
|---|---|
| ID | A03 |
| Alerta ou achado | Timestamp Disclosure - Unix |
| Nível indicado pelo ZAP | Baixo |
| Evidência | O relatório identificou o valor `1666666667`, interpretado pelo ZAP como `2022-10-24 23:57:47`, em resposta da aplicação. |
| Possível impacto | A divulgação de informações técnicas pode auxiliar atividades de reconhecimento do ambiente. Isoladamente, esse alerta possui impacto limitado, mas pode se tornar mais relevante se os timestamps forem combinados com outras informações para revelar versões, períodos de implantação ou padrões exploráveis. |
| Relação com OWASP ou CWE | [CWE-200 — Exposure of Sensitive Information to an Unauthorized Actor](https://cwe.mitre.org/data/definitions/200.html). |
| Correção proposta | Avaliar se a data é necessária na resposta pública e se pode ser agregada a outras informações para revelar padrões exploráveis. Quando não for necessária, remover ou generalizar a informação. Caso seja necessária para a funcionalidade, registrar o achado como risco baixo aceito e manter revisão periódica. |

Esse alerta não comprova, por si só, uma vulnerabilidade crítica. A decisão de corrigir ou aceitar o risco depende da necessidade funcional da data exposta e da existência de outras informações que possam aumentar seu impacto.

## 5 Limitações e revisão dos resultados

A verificação foi realizada em uma única sessão automatizada do OWASP ZAP, limitada ao OWASP Juice Shop executado localmente. Portanto, os resultados representam os alertas identificados nesse cenário específico e não comprovam, por si só, a exploração completa de vulnerabilidades.

A análise foi direcionada aos dois alertas de risco médio e ao alerta de risco baixo apresentados no resumo do relatório. Os três alertas informativos não foram selecionados como achados principais porque possuem menor impacto isolado ou exigem investigação adicional para demonstrar uma consequência de segurança concreta.

Entre os alertas informativos encontrados, o aviso sobre comentários suspeitos foi tratado como informação complementar. Embora possa indicar exposição de detalhes de implementação, não houve evidência suficiente, nesta sessão, de que o comentário permitisse acesso indevido, alteração de dados ou interrupção do serviço. Dessa forma, poderá ser revisado posteriormente, mas não foi priorizado em relação aos alertas de CSP, CORS e divulgação de timestamp.

Também é importante considerar a possibilidade de falsos positivos ou alertas dependentes de contexto. O alerta de configuração CORS, por exemplo, precisa ser interpretado conforme os endpoints afetados e o tipo de informação exposta. Já o alerta de timestamp possui risco baixo e pode ser aceito caso a informação seja necessária para o funcionamento da aplicação e não revele padrões exploráveis.

A principal limitação da sessão é que ela foi executada em uma aplicação de treinamento deliberadamente vulnerável. Portanto, os achados não devem ser atribuídos automaticamente ao BAITA Eventos. O objetivo da atividade foi utilizar a ferramenta de forma autorizada, interpretar os resultados e relacionar os alertas às práticas de segurança estudadas nas etapas anteriores.

A revisão final confirmou que os três achados selecionados possuem evidências no relatório, impactos descritos, referências reconhecidas e propostas de correção compatíveis com os riscos analisados.
