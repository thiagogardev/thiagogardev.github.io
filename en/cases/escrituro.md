---
lang: en
title: "Escrituro: from market research to a product in production in two days"
description: Case study of Escrituro, a system for accounting firms built to match and surpass Contmatic Phoenix. Research with dated sources, a product definition reviewed by an independent agent and a foundation phase deployed to production, all in two days with AI agents under human direction, and version 0.65.0, with SPED, payroll, eSocial and AI, ten days later.
---

# Escrituro: from market research to a product in production in two days

**Period:** November 2026 to date · **Role:** concept, research direction, architecture, code, verification and deployment · **Status:** version 0.65.0 live (2026-11-14), Phases 0 to 3 shipped, pilot in the author's own group

> One-sentence summary: in two working days I went from a question ("can I build a system for accounting firms as good as or better than Contmatic Phoenix?") to a product with a name, a registered domain, market research with 659 dated sources, a product definition reviewed by an independent agent and the first phase of the system running in production, with domain, certificate and backups. On the same day, the accounting module of Phase 1 went live, built on an inventory of 233 functions from the market leaders. Ten days later the system reached version 0.65.0, with SPED, Presumed and Actual Profit, payroll and eSocial, corporate department, AI and the 2027 tax reform.

## 1. Context and problem

The day before, I had built [Clinalta](clinalta.md), a practice management system for clinics, in a single day, to test whether the method I used on my industrial group's ERP would work for a commercial product. It did. The next question was whether it could handle a larger, more regulated market and a product with seven modules that talk to each other: the accounting firm.

The benchmark was Contmatic Phoenix, the best-known system for accounting firms in Brazil. Three reasons for the target: my group pays an external accounting firm, and its seven legal entities serve as the first real client; tax, bookkeeping, payroll, fixed assets, corporate records, fees and client portal demand architecture, not just screens; and the Brazilian tax reform (informational CBS and IBS in 2026, full CBS and the hybrid Simples regime in 2027) forces every vendor to rewrite its tax engine, which opens a window for a product born without legacy.

Two conditions I set: not to build "yet another one", understanding first how the seven leaders work; and to bet on the gray, dense, keyboard-driven look of Phoenix, against the tide of flat white, because it is what the firm's analyst knows and because density is productivity.

## 2. A five-step method

The four steps of Clinalta gained a fifth, an independent review of the definition before any code, and verification now includes deployment. Each step has an exit criterion; nothing moves on without passing it.

```mermaid
flowchart LR
  P[1. Research<br/>6 parallel fronts<br/>source and date on everything] --> S[2. Synthesis<br/>market report<br/>product definition]
  S --> R{3. Review<br/>independent agent<br/>31 findings}
  R -- fixes --> S
  R -- approved --> C[4. Construction<br/>Phase 0: foundation<br/>reuses Clinalta and the ERP]
  C --> V{5. Verification<br/>148 assertions, browser,<br/>production on the VPS}
  V -- failed --> C
  V -- passed --> OK[Live<br/>domain and backups]
```

1. **Research on six parallel fronts.** Six agents, each with a scope and closed questions: market leaders; tax and the tax reform; bookkeeping, fixed assets and corporate records; payroll and eSocial; pain points, prices and integrations; and the Phoenix visual standard, with colors measured pixel by pixel from screenshots. Rule: every claim with a source and a date, gaps declared instead of invented.
2. **Synthesis into two documents.** A market report with an obligation x layout x deadline x source table and twelve ranked gaps, and a product definition in eleven sections: vision, personas, modules by phase, rules as requirements, integrations, architecture, visual standard, commercial model, roadmap, risks and names.
3. **Independent review.** An agent that did not write the definition checked it against the notes and the report. It raised 31 issues, all fixed: misaligned phases, completion criteria tied to 2027 dates, the LALUR tax ledger cited for a regime that does not use it, an AI quota per company that would cost more than the subscription, a "never lock" policy with no exact definition.
4. **Construction of Phase 0.** I reused from Clinalta and the ERP what was already proven (login, two-factor authentication, sessions, audit trail, isolation unit) and wrote the rest: client portfolio, obligations engine, sourced catalog, tasks, delivery receipts, work queue, Phoenix component library.
5. **Verification and deployment.** Clean types and lint, 148 assertions in four suites, the full flow in the browser and, the next day, deployment to the VPS with domain, certificate, daily backups and the database added to the dump routine.

What stayed with me: the question, the constraints, the critical reading, the product decisions and the acceptance. What went to the AI: volume, parallelism, consistency and the cross-review.

## 3. What the research revealed

The fight in the accounting firm is not about features. The seven leaders (Contmatic, Domínio, Alterdata with Prosoft, Fortes, Questor, SCI, Makro) have covered tax, bookkeeping and payroll for decades. The difference lies in performance, the commercial relationship, the predictability of updates and a tax reform that nobody has absorbed natively.

| Finding | Evidence (2026-11-04) | What it changes in the product |
|---|---|---|
| "Cloud" means a virtualized Windows application | Phoenix Cloud and Domínio Web deliver Delphi programs through virtualization; reports of "far too slow", "drops all the time", a four-hour ECF run on a 64 GB server | Native web with desktop density and keyboard, SPED files generated in the background with a visible queue |
| Contracts lock in and prices are on request | 12-month commitment with a 100% penalty, IGP-M indexation with a 4% floor, data removed on termination; only Makro publishes a price list | Public price table by number of companies, no commitment, full export including after cancellation |
| Updates break what worked | Regressions in Contmatic's SPED (07/2026) and Fortes' invoicing (06/2026); eSocial changed its schema four times in four months; no vendor publishes which layouts it supports | Public compliance page per obligation, version, technical note and validity; rules parameterized by assessment period, not by software version |
| The tax reform is a bolted-on module | EFD ICMS/IPI and EFD-Contribuições do not compute CBS and IBS; hybrid Simples in 2027; alphanumeric tax IDs in every layout | IBS and CBS modeled in the document, the assessment and the chart of accounts from the start; tax ID stored as text across the database |
| Federal automation has a known price | Integra Contador costs R$ 3 to R$ 5 per company per month; e-CAC powers of attorney expire after five years | Certificate and power of attorney portfolio with expiry alerts from Phase 0; Serpro cost passed through with no hidden margin |
| Firm management is a "parallel tool" | Domínio users need another tool; Acessórias, Nibo and Gestta live off that gap | Rule-generated obligations calendar, delivery receipts, tasks, fees and portal in the product's core |
| Phoenix is gray, dense and keyboard-driven | Background #F0F0F0, grid header #808080, selection #0078D7, Segoe UI 12 px, 18 px per row, zero radius, Ctrl+Enter opens lookups | A component library with those measured tokens, not "inspired" by them |

A process lesson: the independent review caught errors I would have let through. Two examples: the LALUR ledger exists only under the actual-profit regime, and the DCTFWeb's MIT is a JSON file the system generates, not one it reads.

## 4. Product decisions

Eight non-negotiable principles, each tied to a proven pain. They entered the definition before the first line of code.

- **The data belongs to the firm and its client.** Full export in one click: SPED files, eSocial events with receipts, competitors' TXT layouts, CSV of everything.
- **Never lock over billing.** Overdue accounts never stop anything with a legal deadline; what stops, after 60 days, is creating new companies, users and periods.
- **Public, predictable pricing.** Five tiers, from R$ 149 per month for up to 10 companies to R$ 2,490 for up to 500, all modules, unlimited users, no commitment, adjustments only by the official inflation index.
- **Fast and dense.** Pages under one second, 18 px grid rows, Enter advances, Esc cancels, Alt+letter triggers the button.
- **Nothing is lost.** Entries, documents and events never have their content altered; a correction creates a reversal or a new version with a reason; audit trail on every table.
- **Compliance by versioned, public layout.** Each obligation has a version with validity dates and the system picks it by period; a public page lists what is supported.
- **Relationship inside the system.** Portal, WhatsApp, delivery receipts, tasks and fees are part of the product.
- **Human support with a deadline.** Numbered tickets, reply on the same business day, release notes before every update, update windows outside closing periods.

Three scope decisions: start with bookkeeping and the tax routine of the Simples regime, with my group's companies as the first client; leave legacy state-level returns to the real portfolio, not to the catalog; and keep the Phoenix look against the tide, testing the five main screens with accountants next to the original screenshots.

## 5. What was built in Phase 0

The foundation: what every later phase will use, and what a firm can already operate to control deadlines. The firm is the isolation unit; every table carries its id, and the active client company and period live in the session, as in Phoenix.

| Module | What it does | Origin |
|---|---|---|
| Access | Password login, two-factor authentication, per-device sessions, remote sign-out | Reused from Clinalta |
| Firm and roles | One login, one role per firm (administrator, manager, tax, bookkeeping, payroll, corporate, finance, read-only), restricted to all companies or to selected ones; the support access commitment is written into the help | Adapted: the clinic became the firm, and the client company is a second level |
| Portfolio | Companies with a short alias, numeric or alphanumeric tax ID validated, tax regime by validity period (a change opens a new row, never overwrites), indicators that switch obligations on (employees, ICMS, tax substitution, IPI, ISS, withholdings), responsible analysts, encrypted A1 certificate, powers of attorney with expiry alerts | New |
| Obligations calendar | Catalog of 20 federal obligations with rule, layout and version; deadline rules as pure functions (fixed day, last business day, nth business day, annual date, 13th salary) with national and bank holidays and a computed Easter; postpone or bring forward; idempotent generation per period; statuses with receipt and waiver; batch advance; closing per module | New |
| Tasks and receipts | Open items with owner, priority and deadline; delivery and receipt protocols with sequential numbering and a copy for signature | New |
| Work queue | Long routines queued with visible status, prepared for background SPED generation | New |
| Dashboard and reports | Period obligations by status, next seven days, load per analyst, due dates | New |
| Audit and export | Every change field by field; access per company; nine tables as CSV, instantly | Reused |
| Phoenix visual standard | Measured tokens, about 40 colored icons, cascading text menus, labeled toolbar, window with status bar, "Active base, Active company, Period" strip, keyboard first | New |
| Infra | Docker with Postgres and daily backup, health route, help by chapter, compliance page, demo firm | Reused |

### Screens of the demo firm

The data below comes from the demo firm. The active company's name and tax ID were covered in the screenshots.

![Portfolio dashboard: period obligations by status, next seven days and load per analyst](../../assets/escrituro/painel.png)

*Portfolio dashboard. The top strip shows the active base, company and period, as in Phoenix; the counters are computed over the generated calendar.*

![Obligations calendar for period 10/2026 for the active company, with deadlines computed in business days](../../assets/escrituro/obrigacoes.png)

*Obligations calendar. PGDAS-D fell on 23 November because 20 November is a holiday and the rule postpones; FGTS Digital was brought forward to 19 November by the opposite rule. Each row opens the obligation for receipt, protocol or waiver.*

![Company record in tabs, with regime history, digital certificate and electronic powers of attorney](../../assets/escrituro/empresa.png)

*Company record. Required fields in red, tabs, regime history by validity period, A1 certificate and e-CAC powers of attorney with end dates. Nothing is deleted: deactivating removes from lists and keeps in exports.*

### Built-in compliance

The definition mapped 31 rules and layouts as software requirements, with version and validity. Those that already shaped Phase 0:

| Rule or layout | What it requires | How it is in the system |
|---|---|---|
| IN RFB 2.237/2024 as amended by IN 2.248/2025 | DCTFWeb by the last business day of the following month | "Last business day" rule in the catalog, with source |
| CGSN Resolutions 140/2018 and 183/2025 | PGDAS-D by the 20th, with a late penalty since 2026-01-01 | "20th, postponed" rule and overdue warning on the dashboard |
| FGTS Digital FAQ (Law 8.036, art. 18) | Due on the 20th, brought forward on national or bank holidays | "20th, brought forward" rule, with bank holidays in the calendar |
| EFD ICMS/IPI layouts 020 and 021 | Layout 021 from 2027-01-01 | Catalog with validity per version; the period picks the layout |
| Alphanumeric tax ID (IN RFB 2.229/2024) | New tax IDs with letters from July 2026 | Tax ID as text across the database, ASCII-weighted validation, single formatter |
| IN RFB 1.751/2017 (e-CAC power of attorney) | Power of attorney with a term, required to file on the firm's behalf | Powers of attorney per company with end date and an alert 30 days before |
| LGPD arts. 37 and 46 | Record of processing operations and security | Field-by-field audit trail, access per company, certificate and two-factor secret encrypted |

## 6. Phase 1, accounting module: the market floor and the gaps nobody fills

With Phase 0 live, the next question was what a module needs in order for an accountant to call it complete. Instead of guessing, a second research run, on six parallel fronts, inventoried function by function what Contmatic, Domínio, Alterdata, Fortes, Questor, SCI, Makro and IOB document publicly: 233 functions, classified as floor (four or more vendors), differentiator (one to three) and gap (nobody). Folhamatic, named as a reference, no longer exists: it became IOB Online Sistemas in 2020. The accounting module was built from that list, with the whole floor and the gaps that fit the phase.

The method changed scale: the core (a ten-table schema, pure functions for the chart of accounts, double entry, trial balance, income statement, balance sheet and year-end closing, an OFX reader, reconciliation rules and 64 new assertions) stayed with me and one agent; the 23 screens were split among four agents in parallel, each owning one folder and never touching the others' files. Integration, menus, help and the end-to-end browser check, with Playwright typing the entry and importing the statement, closed the loop.

| Market floor | How it is in Escrituro |
|---|---|
| Model chart of accounts with the tax authority's reference codes, chart per company, short code, nature per account | ITG 1000 model with 182 accounts in four levels, copied to each company on first use; reference codes filled in Phase 2 from the official table, never invented |
| Entries with several lines, standard narrations with variables, cost centers, batches, opening balances | Unlimited lines, Ctrl+M adds a line, Ctrl+Enter or F5 opens the account lookup, debits and credits must balance on screen |
| OFX import and reconciliation by date, amount and narration | Dependency-free OFX reader; suggestion from an existing entry of the same amount within three days or from a rule; the analyst approves, the system never posts on its own |
| Trial balance, ledger, journal, income statement, balance sheet, closing entries | All on screen and in print; year-end closing shows a simulation before posting |
| Period closing with lock | A closed period blocks posting, deleting and restoring |

| Gap no vendor documents | Escrituro's answer |
|---|---|
| Sequential numbering per book under ITG 2000 and linked reversals | A per-company number that is never reused; a reversal creates a new entry linked to the original in both directions |
| Reopening a period with a reason and a trail | Reopening requires a reason and is written to the field-by-field audit trail |
| Recycle bin with restore | Deleting sends the entry to the bin with a reason; restoring brings it back with the same number |
| Reconciliation rules learned from manual reconciliations | The rule is born from the analyst's own reconciliation, keyed by a fragment of the narration, for one company or all |
| Pre-ECD consistency panel | Inverted balances, entries without narration, accounts without reference code or without movement, open periods and the bin, on one panel |

### Screens of the accounting module

![Accounting entry with two lines, account lookup and balancing totals](../../assets/escrituro/lancamento.png)

*Accounting entry. The account is typed by its short code or picked from the lookup; the difference between debits and credits stays visible until it balances, and Save only unlocks when it does.*

![Bank reconciliation with the six lines of the sample OFX statement and the suggested entries](../../assets/escrituro/conciliacao.png)

*Bank reconciliation. Each statement line carries its suggestion: an entry already posted with the same amount, or the learned rule; the analyst reconciles, generates the entry or ignores it with a reason.*

![Trial balance for October 2026 with opening balance, debits, credits and closing balance](../../assets/escrituro/balancete.png)

*Trial balance. Synthetic accounts in bold add up their analytic accounts by code; the opening balance includes the opening entry; debits and credits for the period balance.*

## 7. AI that answers from the manual (RAG)

Escrituro has a manual with more than two hundred topics, and a usage question ("how do I post the vacation accrual?") should not become a support ticket. Ask the Help answers with RAG (Retrieval-Augmented Generation): it first retrieves the right passages of the manual, then asks the model to answer using only those passages and to cite where each answer came from.

| Step | How it works |
|---|---|
| Retrieval | Local BM25, no cost and no external call: accent-free words reduced to their stem, acronyms and codes kept whole (S-1200, R-4010, CFOP), weight 4 for the title, 3 for the screen path, 2 for the summary and 1 for the body. The 5 most relevant topics go into the context, together with the index of all titles |
| Generation | Claude answers only with what is in those topics; if the manual does not cover the question, the answer says so instead of making something up |
| Check | The topics cited in the answer are checked against the ones sent and become links to the manual |
| Privacy and cost | Only the question is sent, with tax IDs, e-mails and phone numbers masked; a repeated question in the same version comes from cache; limit of 100 per firm per day; about US$ 0.05 per new question |
| Evaluation | Every answer has "Did this help? Yes or No". The "No" answers show where the manual or the retrieval fails |

Why keyword search and not embeddings: the manual uses the same vocabulary as the screens, and users ask with the names they see on screen. In that setting stemmed BM25 performs well with no cost and no dependency. Moving to hybrid search (keywords plus embeddings, with a reranker) depends on measurement: if the "No" answers show questions the search missed, a test set with the right topic for each question is built and the hit rate in the top 5 is compared before changing anything.

Besides Ask the Help, Escrituro's AI suggests the tax classification of invoice items and the account for bank reconciliation, using as examples the rules the firm itself has already approved, and explains in plain language the errors returned by eSocial and EFD-Reinf. Before any call, tax IDs, e-mails, phone numbers and Pix keys are pseudonymized, and the answer is rehydrated on the server. In every case the AI only suggests: the analyst saves, and every decision is logged.

## 8. From foundation to 0.65 in about ten days

Between 5 and 14 November 2026, Escrituro went from Phase 0 to version 0.65.0, in 97 commits. The method of the first two days became a routine:

- **Daily, versioned deliveries.** Every version has a number, a dated note in the help's release notes and its own commit; production receives the whole version, never pieces.
- **A backup before every version.** A dump of the production database before every switch, on top of the hourly backup, so any version can be rolled back.
- **Tests per suite.** Every module ships with its own suite (payroll, eSocial, Reinf, ECD, imports and so on), and all of them run before deployment: from 4 suites in Phase 0 to 65.
- **Validation against the official source.** SPED files checked in the tax authority's validators (EFD ICMS/IPI in PVA; EFD-Contribuições and ECF in PGE), and eSocial and EFD-Reinf events sent to the government's restricted production environment, with receipts and totals matching the payroll to the cent.
- **Written rules where the law leaves doubt.** Whatever is unclear becomes an "adopted rule", documented in the help and taken to an accountant, instead of a silent assumption.

| Area | Live in 0.65.0 |
|---|---|
| Bookkeeping | Chart of accounts, entries, OFX reconciliation, trial balance, income statement, balance sheet, financial statements (cash flow, changes in equity, notes), vacation and 13th-salary provisions, fixed assets and ICMS credit on fixed assets (CIAP) |
| SPED and returns | ECD, ECF, EFD ICMS/IPI, EFD-Contribuições, EFD-Reinf, DeSTDA and DEFIS |
| Tax regimes | Simples Nacional, Presumed Profit and Actual Profit with e-Lalur and e-Lacs; PIS/COFINS only under the cumulative regime |
| Payroll | Monthly calculation, vacations, 13th salary, terminations, leaves, occupational health and safety, collective wage adjustments and labor lawsuits; eSocial validated in restricted production; DCTFWeb, FGTS Digital and bank payments through CNAB 240 |
| Corporate and federal | Corporate department through Redesim; tax clearance certificates and the federal (e-CAC) and state (DTE) mailboxes through Integra Contador, with a daily monitor |
| AI | Classification of invoice items and reconciliation from the rules already approved, explanation of eSocial and Reinf errors and Ask the Help, with tax IDs, e-mails, phone numbers and Pix keys pseudonymized before reaching the model |
| Client relationship | Fees, client portal and app (PWA with the firm's brand), client dashboard, firm BI with targets per analyst and NPS survey |
| 2027 tax reform | CBS and IBS under the standard regime with transition credits, and a simulator for the hybrid Simples |
| ICMS ST and foreign trade | ICMS tax substitution lookup for all 27 states; checking of import invoices, import process with exchange rates and an import cost simulator |
| Tax routine | NF-e capture by NSU and by access key, weekly tax bulletin and single-phase PIS/COFINS in the Simples (0.65.0, 2026-11-14) |

The pilot is the author's own group, starting with period 11/2026: it runs in parallel with the current bookkeeping, without filing anything through Escrituro until the first months are checked.

## 9. Numbers

Between 4 and 5 November 2026, from the first question to deployment:

| Measure | Value |
|---|---|
| Parallel research fronts | 6 |
| Words of research notes | 42,300 |
| Distinct sources cited with URL | 659 |
| Market report | 8,500 words, 12 ranked gaps |
| Product definition | 11,600 words, 11 sections, 31 rules and layouts, 20 integrations, 6 phases, 13 risks |
| Independent review findings applied | 31 |
| Files in the initial commit | 122 |
| Lines of TypeScript | 7,876 |
| Database tables | 21 |
| Screens and routes | 36 |
| Obligations in the default catalog, with sources | 20 |
| Automated assertions | 148, in 4 suites |
| Candidate names checked at the Brazilian registry | 10 |
| Time from the decision to deploy to the system answering on the VPS | about 30 minutes |

After the accounting module, on the same 5 November:

| Measure | Value |
|---|---|
| Functions inventoried in the leading systems | 233 (103 floor, 97 differentiators, 33 gaps), 518 citations |
| Accounts in the model chart | 182, 132 analytic |
| Database tables | 30 |
| Screens and routes | 60, of which 23 in accounting |
| Lines of TypeScript | 14,460 |
| Automated assertions | 212, in 5 suites |
| Agents building screens in parallel | 4 |

At version 0.65.0, on 14 November 2026, recounted in the repository:

| Measure | Value |
|---|---|
| Commits | 97 |
| Versioned files | 1,030 |
| Lines of TypeScript | 121,911 |
| Database tables | 159 |
| Migrations | 59 |
| Screens and routes | 251 |
| Automated assertions | 3,635, in 65 suites |
| Manual topics | more than 200 |

What is not done, stated just as clearly: filing the PGDAS-D in production is blocked until one parameter of Serpro's activity table is confirmed (the demo environment only accepts the official example); non-cumulative PIS/COFINS under Actual Profit is not assessed, and EFD-Contribuições is generated only under the cumulative regime; eSocial and EFD-Reinf were validated in restricted production but have no real activity in production yet; the CNAB 240 file still has to be certified with a bank; the 2027 CBS rate is a provisional parameter until the Senate resolution; and the pilot does not file anything through Escrituro yet.

## 10. Lessons

- **Measuring beats describing.** The visual-standard agent read the colors from Phoenix screenshots pixel by pixel. The tokens came out exact, and the first screen already looked like the original instead of "inspired" by it.
- **Independent review before code.** Thirty-one points fixed in an 11-thousand-word definition, by an agent that did not write it. Cheap compared to discovering in code that the AI quota would cost more than the subscription.
- **External dates do not wait for the roadmap.** The definition tied completion criteria to January 2027. The review untied them: layout 021 and the hybrid Simples simulator move to Phase 2, and period 01/2027 can be redone later from the files.
- **Business days are a business rule.** PGDAS-D postpones, FGTS Digital brings forward, and bank holidays count. The deadline engine is a pure function, tested against the real 2026 calendar.
- **The official validator is the final test.** The first run of the Actual Profit ECF in PGE returned dozens of errors; the second, only environment ones. One error in another return came from a long dash copied from the official descriptions, which the validator itself rejects.
- **The right domain is .com.br.** escrituro.com had been with a speculator since April. For a Brazilian product, the .com.br registered directly at the national registry is enough, after unlinking the owner's tax ID from the previous provider.

## 11. Roadmap: shipped and still missing

The six phases of the definition, with what has shipped and what is missing. The gates stay the same: they only count with real use.

1. **Phase 0, foundation.** Live since 2026-11-05; trademark filed with INPI on 2026-11-09, class 42.
2. **Phase 1, bookkeeping and the Simples tax routine.** Shipped on 5 and 6 November 2026 (0.2 to 0.7): bookkeeping, NF-e and CT-e capture, national service invoice, Simples assessment and DEFIS, fees and client portal. Missing: filing the PGDAS-D in production. Gate: a full month of the pilot companies booked in the system.
3. **Phase 2, standard regime and SPED.** Shipped from 6 to 7 November 2026 (0.8 to 0.17): ICMS, IPI, tax substitution and DIFAL, EFD ICMS/IPI, EFD-Contribuições, ECD, ECF, Reinf, DeSTDA, fixed assets and CIAP; EFD ICMS/IPI, EFD-Contribuições and ECF validated in the official programs. Missing: non-cumulative PIS/COFINS.
4. **Phase 3, payroll and eSocial.** Shipped from 7 to 9 November 2026 (0.18 to 0.45), together with Actual Profit, e-Lalur and e-Lacs: events accepted in restricted production and totals checked against the payroll. Gate: the payroll of one pilot company closed in the system with real activity.
5. **Phase 4, corporate records, AI and full portfolio.** Shipped on 9 and 10 November 2026 (0.46 to 0.56): corporate department, federal mailbox, AI, client app, BI, NPS and the onboarding script for the pilot firm. Gate in progress: the pilot operating its whole portfolio.
6. **Phase 5, full tax reform and ecosystem.** Started in 0.58.0 with the 2027 reform under the standard regime and the hybrid Simples simulator. Missing: the final CBS rate, split payment and the public API. Gate: periods from 01/2027 onward assessed without rework.

## 12. Stack

Next.js 16 with server actions · React 19 · TypeScript · Tailwind 4 · Drizzle ORM · PGlite in development and PostgreSQL in production · Docker on an ARM VM on Oracle Cloud in São Paulo, behind Caddy. Conventions inherited from the group: money in cents, dates always in São Paulo time, every table with the firm id, every action writes to the audit trail, nothing is deleted.

## 13. Code

The repository is private: the product is commercial and will process third-party tax, accounting and payroll data. It can be shown in interviews or academic evaluation with the demo firm. The product definition, the market report and the research notes exist in the repository and follow this case.
