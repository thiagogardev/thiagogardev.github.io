# Engenharia de software e IA aplicada à indústria

Diretor executivo de empresas industriais (fabricação de engates rápidos e acoplamentos, importação de suprimentos para mineração) e engenheiro de produção. Pós-graduando em **Sistemas Cognitivos com Agentes de IA**.

Este repositório é o índice do que construí para rodar as minhas próprias empresas: software que substituiu sistemas pagos, automatizou prospecção comercial e sobreviveu à perda total dos servidores físicos. Tudo em produção, usado todo dia, com problema real do outro lado.

**Princípios que valem para todos os projetos**

- Infra própria: Postgres, Docker e Caddy numa VM na Oracle Cloud em São Paulo, com servidor reserva e backup imutável. Serviços externos (IA, e-mail, WhatsApp, emissão fiscal) entram só por API e são trocáveis.
- Dado de negócio fica fora do código. Chaves só em `.env`, nunca versionadas. Varredura de segredos antes de qualquer push.
- Humano no circuito: os agentes preparam, priorizam e sugerem; a decisão comercial e o contato são humanos.
- Documentação como fonte da verdade: cada sistema tem docs de arquitetura, banco, deploy e runbook de restauração.

## Projetos

| Projeto | O que faz | Stack | Estado | Código |
|---|---|---|---|---|
| [**Genesis**](cases/genesis.md) | Motor de agentes de prospecção B2B: coleta fontes públicas (ANM, Receita, PNCP, ComexStat), enriquece, prioriza e gera missões diárias de acionamento comercial; o Claude redige a proposta de substituição da importação. 3 instâncias (mineração, válvulas, obras do Metrô-SP) | Next.js 16, React 19, Drizzle, PostgreSQL + pgvector, Playwright, node-cron, Claude API | Em produção desde 2026 | Privado (case público) |
| **ERP de cotação e fiscal** | Substituiu o ERP comercial perdido num incidente de infraestrutura: cotação de venda com ICMS ST por item, cadastros, contas a pagar/receber, emissão de NF-e via API, compras e estoque | Next.js 16, Drizzle, PostgreSQL, APIs NFE.io/Focus | Em produção desde 30/09/2026 | Privado (regra fiscal documentada no case) |
| **Controle Gerencial** | Contabilidade gerencial multiempresa: partida dobrada, pagar/receber com lançamento automático, balancete, razão, DRE e fluxo de caixa consolidado | Next.js 16, Drizzle, PostgreSQL | Em produção | Privado |
| **Site Acoplin EN/ES** | Site institucional bilíngue por domínio (acoplin.com e acoplin.es num app só), catálogo gerado a partir da API WordPress, formulário de cotação por e-mail | Next.js 16, i18n por middleware, Resend, Docker | No ar desde 25/08/2026 | Privado |
| **Site TT Imports** | Catálogo de 13 linhas e 329 produtos, com fichas técnicas em HTML geradas por script a partir do catálogo original | Next.js 16, Python (pipeline de catálogo) | No ar | Privado |
| [**Site Asset Tecnologia**](https://assettecnologia.com.br) | Site institucional da empresa de software que mantém o Escrituro e o Clinalta, servido por nginx atrás do Caddy | HTML/CSS, Docker | No ar | Privado |
| [**Clinalta**](cases/clinalta.md) | Sistema de gestão para consultórios e clínicas de até 20 profissionais: login com 2FA, multiclínica com perfis, pacientes com consentimentos versionados, agenda em 3 cliques, fila e tarefas, CID-10 embutida, prontuário com linha do tempo e evolução imutável (hash SHA-256 encadeado e gatilho no banco), prescrição pela Memed em homologação, auditoria e exportação total. Nascido de pesquisa de mercado com 501 fontes datadas | Next.js 16, React 19, Drizzle, PGlite/PostgreSQL, Docker | No ar desde 05/11/2026; Fase 1 (prontuário) em construção | Privado (case público) |
| [**Escrituro**](cases/escrituro.md) | Sistema para escritórios de contabilidade construído para igualar e superar o Contmatic Phoenix: contábil, fiscal do Simples ao Lucro Real, SPED (ECD, ECF, EFD ICMS/IPI, EFD-Contribuições, Reinf), folha com eSocial, departamento legal, caixa postal federal, IA com pseudonimização, app do cliente, BI e reforma de 2027, no visual cinza e denso do Phoenix na web. Nascido de pesquisa com 659 fontes datadas e definição revisada por agente independente | Next.js 16, React 19, Drizzle, PGlite/PostgreSQL, Docker, Claude API | Versão 0.65.0 no ar (14/11/2026); piloto no próprio grupo | Privado (case público) |
| [**Monitor ICMS ST**](cases/icms-st.md) | Vigia as normas de ICMS substituição tributária do Confaz e dos estados: coleta, guarda cada versão como prova, detecta a mudança com fonte e data e avisa por e-mail; publica em dados abertos o Convênio 142/2018 com a vigência de cada redação, a matriz de protocolos entre estados (SP para 18 destinos e as demais origens signatárias) e a MVA de MG, PR, RS, BA, MT, PE e AL, cruzadas entre si e já usadas no sistema de cotação. Módulo do Escrituro | Python, httpx, lxml, pdfplumber, SQLite/PostgreSQL, cron | Fase 0 em produção (37 fontes); Fase 1 publicada; 7 estados estruturados | Monitor privado; [dados abertos públicos](https://github.com/thiagogardev/icms-st-dados) |
| [**Inteligência de importação**](cases/inteligencia-importacao.md) | Módulo do Genesis que transforma a lista de quem importou na decisão de quem acionar: mercado endereçável sem a importação cativa, papel de cada empresa pelo CNAE, sinais de momento de compra e o custo de importar e o teto de preço nacional de qualquer um dos 8.782 NCMs, só com dado oficial (Comex Stat, TEC, TIPI, PTAX) | Next.js 16, Drizzle, PostgreSQL, node-cron, Docker | Em produção desde 09/11/2026 | Sistema privado; [ferramenta pública](https://github.com/thiagogardev/custo-importacao-br) |
| [**Central de Atendimento**](cases/central-atendimento.md) | Um número oficial de WhatsApp para três produtos da Asset Tecnologia: menu por produto e assunto, IA que responde dúvidas de suporte só com base no manual do produto, qualificação de vendas e chamado com protocolo, painel com 2FA e SLA em minutos úteis | Next.js 16, Drizzle, PostgreSQL, WhatsApp Cloud API, Claude Haiku, Amazon SES | No ar desde 10/11/2026 | Privado (case público) |
| **Pipeline de marketing** | Moldes e montadores de carrossel e Reels: HTML para PNG via Playwright, narração por TTS, legenda sincronizada queimada no vídeo | Python, Playwright, edge-tts, ffmpeg | Em uso | Privado |

## Cases

- [Genesis: agentes de prospecção comercial em produção](cases/genesis.md). Arquitetura, ciclo do agente, decisões, lições e plano de avaliação. Base para o trabalho da pós.
- [Clinalta: do mercado ao produto em um dia](cases/clinalta.md). Método em quatro etapas, o que a pesquisa revelou, decisões de produto, Fase 0 com telas, a publicação, o prontuário com evolução imutável em três camadas, a prescrição pela Memed em homologação, números e próximos passos.
- [Escrituro: do mercado ao produto no ar em dois dias](cases/escrituro.md). Método em cinco etapas com revisão independente, o que a pesquisa revelou sobre os líderes, Fase 0 com telas, módulo Contábil, IA pelo manual, o caminho da fundação à versão 0.65.0 em cerca de dez dias, números, o que falta e o roteiro.
- [ICMS ST: vigiar a legislação que muda sem aviso](cases/icms-st.md). Como a regra funciona, o que o levantamento das fontes revelou, arquitetura, monitor em produção, Convênio 142, matriz de protocolos e MVA de sete estados em dados abertos, uso real no sistema de cotação, como se mede e próximos passos.
- [Inteligência de importação: da lista de quem importou à decisão de quem acionar](cases/inteligencia-importacao.md). A tese, a arquitetura em três camadas, o que os dados revelaram (47% de importação cativa, a consulta anual que cobria só 35%), o custo de importar e o teto de preço nacional, como se mede e o código aberto.
- [Central de Atendimento: um WhatsApp, três produtos e IA que responde pelo manual](cases/central-atendimento.md). Webhook assinado e idempotente, fluxo de conversa como função pura, IA com o manual em cache (de cerca de US$ 0,09 para US$ 0,0004 por pergunta), SLA e um incidente real com a API da Meta.
- [Infraestrutura e recuperação de desastre: do ransomware a três nuvens em cinco dias](cases/infraestrutura.md). Backup de hora em hora, Object Lock em modo Compliance, restauração testada, cópias cifradas e duas migrações de provedor com bancos conferidos por contagem.
- [Como trabalho com agentes de IA](atividade.md). Registro diário, só com números, das tarefas fechadas, commits e deploys feitos com o Claude Code, atualizado por uma tarefa agendada.
- ERP de cotação com ICMS ST (em preparação)

## Como ler este portfólio

Cada case segue o mesmo modelo ([cases/modelo-de-case.md](cases/modelo-de-case.md)): problema, resultado, arquitetura, decisões, lições e o que vem depois. Os repositórios das empresas são privados; o código pode ser mostrado em entrevista ou avaliação acadêmica, com dados sintéticos.

## Contato

Site: [garofalodeveloper.com](https://garofalodeveloper.com) · GitHub: [@thiagogardev](https://github.com/thiagogardev) · LinkedIn: [thiago-garofalo](https://www.linkedin.com/in/thiago-garofalo-792a2653/)
