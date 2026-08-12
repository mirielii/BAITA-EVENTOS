# 25. Verificação de vulnerabilidades

## 25.1 Sistema ou ambiente testado

A verificação foi realizada exclusivamente no **OWASP Juice Shop**, uma aplicação deliberadamente vulnerável criada para fins educacionais e de treinamento em segurança.

A aplicação foi executada localmente por meio de um contêiner Docker e disponibilizada somente no endereço:

```text
http://127.0.0.1:3000
```

O endereço `127.0.0.1` corresponde ao próprio computador utilizado no teste. Portanto, a sessão não envolveu sistemas de terceiros nem aplicações sem autorização.

## 25.2 Ferramenta utilizada

A ferramenta utilizada foi o **OWASP ZAP 2.17.0** (Zed Attack Proxy), ferramenta de teste de segurança para aplicações web.

O ZAP foi utilizado para identificar alertas relacionados a configurações e comportamentos potencialmente inseguros da aplicação executada localmente.

## 25.3 Configuração básica da sessão

A sessão foi configurada com o seguinte escopo:

| Item | Configuração |
|---|---|
| Alvo da verificação | `http://127.0.0.1:3000` |
| Aplicação analisada | OWASP Juice Shop |
| Ferramenta | OWASP ZAP 2.17.0 |
| Tipo de verificação | Varredura automatizada com descoberta de recursos e verificação ativa |
| Escopo permitido | Somente recursos acessíveis a partir da aplicação local |
| Saída gerada | Relatório HTML e alertas da sessão |

A varredura foi limitada ao ambiente local autorizado. O ZAP realizou a descoberta de recursos acessíveis a partir da página inicial e executou verificações ativas nesse escopo.

A sessão foi concluída com alertas de risco médio, baixo e informativo. A análise detalhada dos achados, das evidências e das propostas de correção será apresentada nos arquivos seguintes.