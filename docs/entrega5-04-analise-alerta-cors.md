### 25.5.2 A02 — Cross-Domain Misconfiguration

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
