## 25.5 Análise dos alertas e achados

### 25.5.1 A01 — Content Security Policy (CSP) Header Not Set

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
