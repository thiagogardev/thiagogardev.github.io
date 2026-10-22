# Thiago Garofalo · Engenharia de software e IA aplicada à indústria

Diretor executivo de empresas industriais (fabricação de engates rápidos e acoplamentos, importação de suprimentos para mineração) e engenheiro de produção. Pós-graduando em **Sistemas Cognitivos com Agentes de IA**.

Este repositório é o índice do que construí para rodar as minhas próprias empresas: software que substituiu sistemas pagos, automatizou prospecção comercial e sobreviveu a um incidente de ransomware. Tudo em produção, usado todo dia, com problema real do outro lado.

**Princípios que valem para todos os projetos**

- Open-source e self-hosted: nada depende de plataforma gerenciada. Postgres, Docker e nginx/Caddy numa VPS própria.
- Dado de negócio fica fora do código. Chaves só em `.env`, nunca versionadas. Varredura de segredos antes de qualquer push.
- Humano no circuito: os agentes preparam, priorizam e sugerem; a decisão comercial e o contato são humanos.
- Documentação como fonte da verdade: cada sistema tem docs de arquitetura, banco, deploy e runbook de restauração.

## Projetos

| Projeto | O que faz | Stack | Estado | Código |
|---|---|---|---|---|
| [**Genesis**](cases/genesis.md) | Motor de agentes de prospecção B2B: coleta fontes públicas (ANM, Receita, PNCP, ComexStat), enriquece, prioriza e gera missões diárias de acionamento comercial. 3 instâncias (mineração, válvulas, obras do Metrô-SP) | Next.js 16, React 19, Drizzle, PostgreSQL + pgvector, Playwright, node-cron, Ollama | Em produção desde 2026 | Privado (case público) |
| **ERP de cotação e fiscal** | Substituiu o SAP perdido no ransomware: cotação de venda com ICMS ST por item, cadastros, contas a pagar/receber, emissão de NF-e via API, compras e estoque | Next.js 16, Drizzle, PostgreSQL, APIs NFE.io/Focus | Em produção desde 30/09/2026 | Privado (regra fiscal documentada no case) |
| **Controle Gerencial** | Contabilidade gerencial multiempresa: partida dobrada, pagar/receber com lançamento automático, balancete, razão, DRE e fluxo de caixa consolidado | Next.js 16, Drizzle, PostgreSQL | Em produção | Privado |
| **Site Acoplin EN/ES** | Site institucional bilíngue por domínio (acoplin.com e acoplin.es num app só), catálogo gerado a partir da API WordPress, formulário de cotação por e-mail | Next.js 16, i18n por middleware, Resend, Docker | No ar desde 25/08/2026 | [Público](https://github.com/thiagogardev/acoplin-site) |
| **Site TT Imports** | Catálogo de 13 linhas e 329 produtos, com fichas técnicas em HTML geradas por script a partir do catálogo original | Next.js 16, Python (pipeline de catálogo) | No ar | [Público](https://github.com/thiagogardev/tt-imports) |
| **Site Asset Consulting** | Página institucional estática servida por nginx atrás do Caddy da VPS | HTML/CSS, Docker | No ar | Público em preparação |
| **Pipeline de marketing** | Moldes e montadores de carrossel e Reels: HTML para PNG via Playwright, narração por TTS, legenda sincronizada queimada no vídeo | Python, Playwright, edge-tts, ffmpeg | Em uso | [Público](https://github.com/thiagogardev/acoplin-marketing) |

## Cases

- [Genesis: agentes de prospecção comercial em produção](cases/genesis.md). Arquitetura, ciclo do agente, decisões, lições e plano de avaliação. Base para o trabalho da pós.
- ERP de cotação com ICMS ST (em preparação)
- Infraestrutura e recuperação pós-ransomware: VPS, Docker, backup imutável com Object Lock (em preparação)

## Como ler este portfólio

Cada case segue o mesmo modelo ([cases/_modelo.md](cases/_modelo.md)): problema, resultado, arquitetura, decisões, lições e o que vem depois. Código privado pode ser mostrado em entrevista ou avaliação acadêmica, com dados sintéticos.

## Contato

GitHub: [@thiagogardev](https://github.com/thiagogardev)
