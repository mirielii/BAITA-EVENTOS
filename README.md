# 🎉 BAITA EVENTOS

> Plataforma para Gestão de Eventos

## 📖 Sobre o projeto

O **BAITA EVENTOS** é uma plataforma desenvolvida para centralizar o gerenciamento de eventos acadêmicos, institucionais e corporativos. O sistema permite a criação de eventos, gerenciamento de inscrições, controle de participantes, realização de check-in, avaliação de atividades e acompanhamento de indicadores.

Este repositório foi criado para o desenvolvimento das atividades da disciplina de **Engenharia de Software Seguro**, com foco na identificação de ameaças, modelagem de riscos e definição de casos de abuso durante as etapas do projeto.

---

## 🎯 Objetivo

Realizar a análise de segurança do sistema **BAITA EVENTOS**, aplicando técnicas de modelagem de ameaças, especialmente o método **STRIDE**, identificando vulnerabilidades, ativos críticos e possíveis casos de abuso antes da implementação do software.

---

## 👥 Integrantes

- Mirieli de Oliveira
- Vitoria Garcia
- Andressa Assae
- Maiara Neri
- Natalia Perri
- Hanna Mendonça

---

## 📂 Estrutura do repositório

```text
.
├── README.md
├── docs/
│   ├── entrega-1/                  # ETAPA 1 — STRIDE e casos de abuso
│   │   ├── 01-descricao-do-sistema.md
│   │   ├── 02-usuarios-ativos.md
│   │   ├── 03-arquitetura.md
│   │   ├── 04-modelagem-stride.md
│   │   ├── 05-casos-de-abuso.md
│   │   └── 06-consideracoes-finais.md
│   ├── entrega-2/                  # ETAPA 2 — Análise e tratamento de riscos (NIST CSF)
│   │   ├── 01-criterios-e-registro-de-riscos.md
│   │   ├── 02-priorizacao-dos-riscos.md
│   │   ├── 03-estrategias-e-funcoes-nist.md
│   │   ├── 04-mapeamento-nist.md
│   │   ├── 05-plano-de-tratamento.md
│   │   └── 06-implementacao-e-risco-residual.md
│   ├── entrega-3/                  # ETAPA 3 — Arquitetura segura
│   │   ├── 01-requisito-rs01.md
│   │   ├── 02-requisito-rs02.md
│   │   ├── 03-requisito-rs03.md
│   │   ├── 04-vulnerabilidades-catalogadas.md
│   │   └── 05-decisoes-arquitetura.md
│   ├── entrega-4/                  # ETAPA 4 — Código seguro e testes
│   │   ├── 01-contexto-e-testes-autorizacao.md
│   │   ├── 02-implementacao-autorizacao.md
│   │   ├── 03-resultados-e-referencia-autorizacao.md
│   │   ├── 04-contexto-e-testes-sessoes.md
│   │   ├── 05-implementacao-sessoes.md
│   │   └── 06-resultados-e-referencia-sessoes.md
│   ├── entrega-5/                  # ETAPA 5 — Verificação de vulnerabilidades
│   │   └── 01-verificacao-de-vulnerabilidades.md
│   └── entrega-7/                  # ETAPA 7 — DevSecOps e vídeo final
│       └── 01-devsecops-e-videofinal.md
├── diagramas/
│   ├── diagrama-stride.png
│   └── modelo-de-ameacas.tm7
├── evidencias/
│   └── etapa-5/
│       ├── 01-resumo-alertas-zap.png
│       ├── 02-alerta-csp-ausente.png
│       ├── 03-alerta-cors.png
│       ├── 04-alerta-timestamp.png
│       └── relatorio-zap-juice-shop.html
├── materiais-complementares/
│   ├── ameacas-stride-original.csv
│   ├── ameacas-stride-original.xlsx
│   └── ameacas-stride-selecionadas.xlsx
└── roteiros/
    ├── entrega-6/etapa-6-deteccao-de-intrusoes.md
    └── entrega-7/entrega-7-devsecops-videofinal.md
```

Os diagramas e os materiais complementares da modelagem STRIDE são produzidos com o Microsoft Threat Modeling Tool.

---

## 📚 Documentação

A documentação de cada etapa do trabalho está organizada na pasta **docs/**, em subpastas `entrega-1` a `entrega-5` e na subpasta `entrega-7` (DevSecOps e vídeo final). A Etapa 5 está consolidada em **docs/entrega-5/01-verificacao-de-vulnerabilidades.md**, enquanto as capturas de tela e o relatório do ZAP permanecem em **evidencias/etapa-5/**.

### Etapa 1 — Identificação do sistema, STRIDE e casos de abuso

| Documento | Descrição |
|-----------|-----------|
| [01 — Descrição do sistema](docs/entrega-1/01-descricao-do-sistema.md) | Problema resolvido, usuários e funcionalidades |
| [02 — Usuários, ativos e pontos de interação](docs/entrega-1/02-usuarios-ativos.md) | Perfis, dados sensíveis e recursos a proteger |
| [03 — Visão geral da arquitetura](docs/entrega-1/03-arquitetura.md) | Componentes, fluxos e fronteiras de confiança |
| [04 — Modelagem de ameaças (STRIDE)](docs/entrega-1/04-modelagem-stride.md) | 30 ameaças em seis categorias |
| [05 — Casos de abuso](docs/entrega-1/05-casos-de-abuso.md) | Casos de abuso relacionados ao STRIDE |
| [06 — Considerações finais](docs/entrega-1/06-consideracoes-finais.md) | Síntese, ativos e dificuldades da análise |

### Etapa 2 — Análise, priorização e tratamento de riscos (NIST CSF)

| Documento | Descrição |
|-----------|-----------|
| [01 — Critérios e registro de riscos](docs/entrega-2/01-criterios-e-registro-de-riscos.md) | Critérios de probabilidade/impacto e registro R01–R10 |
| [02 — Priorização dos riscos](docs/entrega-2/02-priorizacao-dos-riscos.md) | Ordem de prioridade e justificativas |
| [03 — Estratégias e funções NIST](docs/entrega-2/03-estrategias-e-funcoes-nist.md) | Estratégias de tratamento e funções do NIST CSF 2.0 |
| [04 — Mapeamento NIST](docs/entrega-2/04-mapeamento-nist.md) | Mapeamento dos riscos R01–R10 para as funções do NIST CSF 2.0 |
| [05 — Plano de tratamento](docs/entrega-2/05-plano-de-tratamento.md) | Controles, responsáveis e evidências de verificação |
| [06 — Implementação e risco residual](docs/entrega-2/06-implementacao-e-risco-residual.md) | Ordem de implementação e estimativa do risco residual |

### Etapa 3 — Arquitetura segura

| Documento | Descrição |
|-----------|-----------|
| [01 — Requisito RS01](docs/entrega-3/01-requisito-rs01.md) | Requisito de segurança para a disponibilidade da plataforma |
| [02 — Requisito RS02](docs/entrega-3/02-requisito-rs02.md) | Proteção de contas autenticadas |
| [03 — Requisito RS03](docs/entrega-3/03-requisito-rs03.md) | Autorização de operações protegidas na API REST |
| [04 — Vulnerabilidades catalogadas](docs/entrega-3/04-vulnerabilidades-catalogadas.md) | Mapeamento de vulnerabilidades catalogadas (CWE) para os riscos prioritários |
| [05 — Decisões de arquitetura](docs/entrega-3/05-decisoes-arquitetura.md) | Arquitetura segura e justificativas |

### Etapa 4 — Código seguro e testes

| Documento | Descrição |
|-----------|-----------|
| [01 — Contexto e testes de autorização](docs/entrega-4/01-contexto-e-testes-autorizacao.md) | Testes TS01/TS02 definidos antes da implementação |
| [02 — Implementação da autorização](docs/entrega-4/02-implementacao-autorizacao.md) | Pseudocódigo do controle de autorização na API REST |
| [03 — Resultado esperado e referência](docs/entrega-4/03-resultados-e-referencia-autorizacao.md) | Resultado esperado e OWASP Authorization Cheat Sheet |
| [04 — Contexto e testes de sessões](docs/entrega-4/04-contexto-e-testes-sessoes.md) | Testes TS03/TS04 definidos antes da implementação |
| [05 — Implementação de sessões](docs/entrega-4/05-implementacao-sessoes.md) | Pseudocódigo da criação de sessões locais protegidas |
| [06 — Resultado esperado e referência (sessões)](docs/entrega-4/06-resultados-e-referencia-sessoes.md) | Resultado esperado e OWASP Session Management Cheat Sheet |

---

## 📋 Etapas do trabalho

| Etapa | Entregável | Status |
|-------|-----------|--------|
| 1 — Modelagem de ameaças (STRIDE) e casos de abuso | [docs/entrega-1/](docs/entrega-1/) | ✅ Concluída |
| 2 — Análise e tratamento de riscos (NIST CSF) | [docs/entrega-2/](docs/entrega-2/) | ✅ Concluída |
| 3 — Arquitetura segura | [docs/entrega-3/](docs/entrega-3/) | ✅ Concluída |
| 4 — Código seguro e testes | [docs/entrega-4/](docs/entrega-4/) | ✅ Concluída |
| 5 — Verificação de vulnerabilidades | [docs/entrega-5/](docs/entrega-5/) + [evidencias/etapa-5/](evidencias/etapa-5/) | ✅ Concluída |
| 6 — Roteiro de detecção de intrusões | [roteiros/entrega-6/](roteiros/entrega-6/) | ✅ Concluída |
| 7 — DevSecOps e vídeo final | [docs/entrega-7/](docs/entrega-7/) | ⏳ Vídeo final pendente |

---

## 🛠 Ferramentas utilizadas

- Git
- GitHub
- Markdown
- Draw.io
- Microsoft Threat Modeling Tool
- OWASP ZAP e OWASP Juice Shop
- GitHub Projects +

---

## 🤝 Colaboração

Cada integrante do grupo trabalha em sua própria branch e envia as contribuições para a `main` por meio de *pull requests*, preservando a autoria individual de cada alteração no histórico do repositório.

---

## 📅 Etapas do projeto

- ✅ Estruturação do repositório
- ✅ Etapa 1 — STRIDE e casos de abuso
- ✅ Etapa 2 — Riscos e plano de tratamento
- ✅ Etapa 3 — Arquitetura segura
- ✅ Etapa 4 — Código seguro e testes
- ✅ Etapa 5 — Verificação de vulnerabilidades
- ✅ Etapa 6 — Roteiro de detecção de intrusões
- ⏳ Etapa 7 — Vídeo final
- ⏳ Revisão final e entrega

---