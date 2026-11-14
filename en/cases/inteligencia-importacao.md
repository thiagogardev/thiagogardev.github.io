---
title: "Import intelligence: from the list of who imported to the decision of whom to call"
description: Case study of the import intelligence module of Genesis. It uses a commercial trade-data platform only as the source of company names and builds the decision layer on official open data, separating the addressable market from captive imports, reading each company's buying moment and computing the landed cost and the domestic price ceiling of any Brazilian NCM code. In production, with a public open-source tool.
lang: en
---

# Import intelligence: from the list of who imported to the decision of whom to call

**Period:** November 2026 · **Role:** thesis, data source research, architecture, code, validation and publishing · **Status:** in production since 2026-11-09 (automatic layer for every NCM; deep layer validated on one NCM); the open-data part published as an [open-source tool](https://github.com/thiagogardev/custo-importacao-br)

> One-sentence summary: trade-data platforms tell you who imported, from whom and how much, and stop at the spreadsheet download; on top of them and of official data I built the layer that says **whom to call today, with which product, at what price and why**, with the landed cost and the domestic price ceiling computed for every one of the 8,782 tariff codes (NCM) Brazil imports.

## 1. Context and problem

My family's group has an importer of mining supplies and a domestic manufacturer of couplings, hoses and fittings. Both compete with imported goods. The first commercial question is always the same: who buys this abroad, and for how much?

There is a paid answer. Trade intelligence platforms cross bills of lading and show, per NCM code, the likely importers and exporters with FOB value and weight. It is a good snapshot, but generic: it serves any industry and stops at the download. On the subscription in use, each query shows up to 1,000 rows, and downloads are capped per month.

The starting point was a question from the business owner himself: can we build something like it, and more differentiated?

## 2. The thesis

Importer names **are not available as open data in Brazil**: the import declaration is confidential. Rebuilding the list would mean buying manifest data at a cost higher than the subscription. So the thesis was different: **use the paid platform only as the source of names and build everything else on official data**.

The differentiation is in the decision layer, which no platform delivers ready for a specific manufacturer or distributor:

| The platform answers | The system answers |
|---|---|
| Who imported | Who among them **can buy from someone else** (excluding captive imports, a subsidiary buying from its parent) |
| How much | **When** to call: who is late to restock, who stopped, who just started, who is growing |
| From which country | **What importing really costs**, and the domestic price ceiling that beats the import |
| A spreadsheet | A **sales mission** with the reason, measured by Precision@K |

One sentence guided the design: **the platform ends at the download; the system ends at the order.**

## 3. Three-layer architecture

```mermaid
flowchart TB
  subgraph A["Automatic layer: every NCM, official data only"]
    CS[Comex Stat national data<br/>market, price, freight, mode, country] --> R[Landed cost and<br/>ceiling per NCM]
    TEC[Common External Tariff<br/>and Gecex 272/2021 annexes] --> R
    TIPI[IPI table] --> R
    PTAX[PTAX exchange rate] --> R
    R --> RK[Opportunity ranking<br/>with a score]
  end
  subgraph P["Deep layer: selected NCMs"]
    L[Monthly lists from the<br/>paid platform] --> C[Tax ID matched in the<br/>open company registry]
    C --> M[Captive, role, signals,<br/>reference price, ceiling<br/>and margin per importer]
  end
  subgraph G["Action and measurement"]
    MI[Mission with the reason] --> PK[Precision@K by origin]
  end
  RK -- "chooses where to spend the quota" --> L
  M --> MI
```

The automatic layer runs by itself every month. The deep layer is only worth it on the codes the first one points to, because each one costs download quota.

## 4. What the data revealed

The figures below are for NCM 4009.31.00 (textile-reinforced rubber hose), the pilot.

- **The yearly query was misleading.** Downloaded for 12 months at once, the list covered only **35% of the value** because of the 1,000-row cap. Downloaded month by month, each list covers 86% to almost 100% of the month. The real market was about US$ 56 to 60 million in 12 months, not the US$ 13 million the yearly screen showed.
- **Almost half the market is captive.** Matching each importer's brand against the exporter list, **47% of imported value is a Brazilian subsidiary buying from its own parent**: machinery makers, carmakers and automotive suppliers. The top name in the platform's ranking is nobody's customer for this product. The addressable market was US$ 29.8 million across 713 companies.
- **Trading companies were hidden.** Companies importing on behalf of others had been left out of the top 1,000 of an earlier open-data-only engine, and moved into the top 10 once each company's role came from its activity code (CNAE).
- **The code average misleads bulk sellers.** The national average was US$ 7.65/kg because it includes carmakers' assembled hoses. Among distributors, traders and manufacturers the median was US$ 4.63/kg. The price ceilings are very different: R$ 69.61/kg versus R$ 42.13/kg.

## 5. Landed cost and domestic price ceiling

The calculation that became the core of the system:

```text
net cost        = CIF + import duty + AFRMM (merchant marine fee) + customs fees
cash outlay     = net cost + IPI + PIS + Cofins + ICMS (gross-up)
domestic ceiling = net cost / ((1 − ICMS on the domestic sale) × (1 − PIS/Cofins credit))
```

Importers under the standard tax regime recover IPI, PIS, Cofins and import ICMS as credits. The fair comparison with a domestic purchase, which also generates credits, is the net cost. The **ceiling** is the invoice price (with ICMS, without IPI) below which buying in Brazil is cheaper than importing.

| 1 kg of 4009.31.00 in bulk (US$ 4.63, PTAX R$ 5.5146) | R$ |
|---|---|
| CIF (5.79% freight and 0.075% insurance, from Comex Stat) | 27.03 |
| Import duty (12.6%) | 3.41 |
| AFRMM and customs fees | 0.92 |
| **Net cost** | **31.35** |
| Cash outlay, with recoverable taxes | 44.51 |
| **Domestic price ceiling** | **42.13** |

Since the manufacturer sells by the meter, not by the kilo, the weight per meter of each size, computed from the wall cross-section and calibrated against manufacturers' data sheets, converts the ceiling: a 1/2" SAE 100R6 hose comes to R$ 9.56 per meter.

A product decision came from a conversation with the owner: **the selling price floats and is not what matters. The product is the ceiling.** The company's own price became optional, and the system scaled to every NCM with no manual input.

## 6. How it is measured

- **Matching names to tax IDs:** 853 of 868 importers matched to the open company registry by legal name. A check against the tax IDs in a sample of shipments got 99 out of 99 right.
- **Open-data engine versus the paid list:** the engine allocates each municipality's import value among compatible companies. Calibrated on halves, with no company named in the weights, it raised the value covered by its top 200 from 44% to 49%, measured on the half left out of the fit.
- **Tax rates:** the official rates loaded automatically for 10,522 codes matched the manual check in a subscribed tax database.
- **Usage:** every mission is born with an origin and a batch, and the salesperson marks whether the lead made sense. Precision@K by origin (rules, AI, signals) is the metric that decides whether the tool is right.

## 7. Numbers

| Measure | Value |
|---|---|
| NCM codes with cost and ceiling | 8,782 (US$ 294.6 bn imported in 12 months) |
| Comex Stat rows processed per load | 6.75 million, in about 30 s |
| Official tax rates loaded | 10,522 codes |
| Full ranking computation | about 1.5 s |
| Importers in the pilot | 868, 853 with tax ID |
| Captive market in the pilot | 47% |
| From request to deploy | about 1 day |

## 8. Lessons

- **Data first, screen later.** The most valuable finding, that the yearly query covered only 35% of value, came from adding up spreadsheets, not from building UI.
- **Licensed and open data in separate compartments.** What comes from the subscription stays inside the system. What is open became a [public tool](https://github.com/thiagogardev/custo-importacao-br). The split also defined the possible product: each customer can only load spreadsheets from their own subscription.
- **Averages hide segments.** Addressable market, role by activity code and the bulk reference exist because the code average mixed carmakers with distributors.
- **Infrastructure is product too.** The official Comex Stat server sends an incomplete TLS chain, missing the intermediate certificate. Windows completes it on its own, and the production Linux refused the connection. The fix added the intermediate as an extra authority, without turning verification off.
- **The user redefines the product.** The margin idea required a selling price. The owner pointed out that prices float, and the ceiling became the main output.

## 9. Next steps

1. **Measured use:** 2 to 4 weeks of evaluated missions on the group's product lines, plus monthly lists for two or three more codes chosen by the ranking.
2. **Proposal with numbers (shipped on 2026-11-10):** landed cost, ceiling and margin in the import-substitution proposal. The e-mail is drafted by Claude using only the facts cleared for the customer; the ceiling, the margin and the licensed data never reach the model, and every number in the text is checked against the facts, at about US$ 0.03 per draft.
3. **Active alerts:** notify when a new signal appears after the monthly load.
4. **Purchasing intelligence:** price per kg by exporter, for the importer to negotiate and choose suppliers.
5. **Closed loop:** record the commercial outcome and recalibrate the score with it.
6. **End price:** bring prices paid in public tenders, for the three price layers (FOB, importer cost, end price).

## 10. Stack and code

- **System:** Next.js 16, React 19, Drizzle, PostgreSQL (PGlite in development), node-cron, Docker, ARM VM on Oracle Cloud in São Paulo since 2026-11-12 (before that, AWS in São Paulo and, originally, Hetzner); Claude API for drafting the proposal.
- **Official sources:** Comex Stat (Ministry of Development and Trade), Common External Tariff and Gecex 272/2021 annexes, IPI table (Federal Revenue), PTAX (Central Bank), open company registry and Siscomex-enabled importers (Federal Revenue).
- **Code:** the system is private, and the code can be shown in interviews or academic review with synthetic data. The open-data part is public under MIT: [thiagogardev/custo-importacao-br](https://github.com/thiagogardev/custo-importacao-br).
- **See also:** the [Genesis case](genesis.md), the prospecting system this is a module of, and the [ICMS ST case](icms-st.md), from the same open fiscal data family.
