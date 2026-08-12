## 25.4 Evidências da execução

A execução da sessão de verificação foi registrada por meio do relatório HTML exportado pelo OWASP ZAP e de capturas de tela dos alertas selecionados.

Os arquivos estão armazenados na pasta `evidencias/etapa-5/` do repositório.

### Relatório completo

O relatório da sessão está disponível em:

[relatorio-zap-juice-shop.html](../../evidencias/etapa-5/relatorio-zap-juice-shop.html)

### Resumo dos alertas

A sessão identificou dois alertas de risco médio, um alerta de risco baixo e três alertas informativos. Os três achados analisados foram escolhidos entre os alertas de risco médio e baixo.

![Resumo dos alertas identificados pelo OWASP ZAP](../../evidencias/etapa-5/01-resumo-alertas-zap.png)

### Evidências dos achados selecionados

**A01 — Content Security Policy (CSP) Header Not Set**

![Alerta sobre ausência do cabeçalho Content Security Policy](../../evidencias/etapa-5/02-alerta-csp-ausente.png)

**A02 — Cross-Domain Misconfiguration**

![Alerta sobre configuração CORS ampla](../../evidencias/etapa-5/03-alerta-cors.png)

**A03 — Timestamp Disclosure - Unix**

![Alerta sobre divulgação de timestamp](../../evidencias/etapa-5/04-alerta-timestamp.png)

As evidências demonstram que a varredura foi realizada exclusivamente em `http://127.0.0.1:3000`, correspondente ao OWASP Juice Shop executado localmente.
