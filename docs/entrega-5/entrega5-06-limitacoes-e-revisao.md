## 25.6 Limitações e revisão dos resultados

A verificação foi realizada em uma única sessão automatizada do OWASP ZAP, limitada ao OWASP Juice Shop executado localmente. Portanto, os resultados representam os alertas identificados nesse cenário específico e não comprovam, por si só, a exploração completa de vulnerabilidades.

A análise foi direcionada aos dois alertas de risco médio e ao alerta de risco baixo apresentados no resumo do relatório. Os três alertas informativos não foram selecionados como achados principais porque possuem menor impacto isolado ou exigem investigação adicional para demonstrar uma consequência de segurança concreta.

Entre os alertas informativos encontrados, o aviso sobre comentários suspeitos foi tratado como informação complementar. Embora possa indicar exposição de detalhes de implementação, não houve evidência suficiente, nesta sessão, de que o comentário permitisse acesso indevido, alteração de dados ou interrupção do serviço. Dessa forma, ele poderá ser revisado posteriormente, mas não foi priorizado em relação aos alertas de CSP, CORS e divulgação de timestamp.

Também é importante considerar a possibilidade de falsos positivos ou alertas dependentes de contexto. O alerta de configuração CORS, por exemplo, precisa ser interpretado conforme os endpoints afetados e o tipo de informação exposta. Já o alerta de timestamp possui risco baixo e pode ser aceito caso a informação seja necessária para o funcionamento da aplicação e não revele padrões exploráveis.

A principal limitação da sessão é que ela foi executada em uma aplicação de treinamento deliberadamente vulnerável. Portanto, os achados não devem ser atribuídos automaticamente ao BAITA Eventos. O objetivo da atividade foi utilizar a ferramenta de forma autorizada, interpretar os resultados e relacionar os alertas às práticas de segurança estudadas nas etapas anteriores.

A revisão final confirmou que os três achados selecionados possuem evidências no relatório, impactos descritos, referências reconhecidas e propostas de correção compatíveis com os riscos analisados.
