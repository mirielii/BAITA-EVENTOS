### 25.5.3 A03 — Timestamp Disclosure - Unix

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
