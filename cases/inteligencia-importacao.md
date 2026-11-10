---
title: "Inteligência de importação: da lista de quem importou à decisão de quem acionar"
description: Case do módulo de inteligência de importação do Genesis. Usa uma plataforma comercial só como fonte de nomes e constrói a camada de decisão com dados abertos e oficiais, separando o mercado endereçável da importação cativa, lendo o momento de compra de cada empresa e calculando o custo de importar e o teto de preço nacional de qualquer NCM. Em produção, com uma ferramenta pública de código aberto.
---

# Inteligência de importação: da lista de quem importou à decisão de quem acionar

**Período:** novembro de 2026 · **Papel:** tese, levantamento das fontes, arquitetura, código, validação e publicação · **Estado:** em produção desde 09/11/2026 (camada automática para todos os NCMs; camada profunda validada num NCM); parte de dado aberto publicada como [ferramenta de código aberto](https://github.com/thiagogardev/custo-importacao-br)

> Resumo em uma frase: as plataformas de comércio exterior dizem quem importou, de quem e quanto, e terminam no download da planilha; construí, em cima delas e de dados oficiais, a camada que diz **quem acionar hoje, com qual produto, a que preço e por quê**, com o custo de importar e o teto de preço nacional calculados para qualquer um dos 8.782 NCMs importados pelo Brasil.

## 1. Contexto e problema

O meu grupo tem uma importadora de suprimentos para mineração e uma fabricante nacional de engates, mangueiras e conexões. As duas competem com produto importado. A primeira pergunta comercial é sempre a mesma: quem compra isto lá fora, e por quanto?

Existe resposta paga para isso. As plataformas de inteligência de comércio exterior cruzam os conhecimentos de embarque e mostram, por NCM, os prováveis importadores e exportadores, com o valor FOB e o peso. É um bom retrato, mas é genérico: serve a qualquer setor e para no download. Na assinatura usada, cada consulta mostra até 1.000 linhas, e há cota de downloads por mês.

O ponto de partida foi uma pergunta do próprio dono da empresa: dá para fazer algo parecido, e mais diferenciado?

## 2. A tese

Os nomes dos importadores **não existem em dado aberto no Brasil**: a declaração de importação é sigilosa. Reproduzir a lista exige comprar o dado de manifesto, e o custo sairia maior que a assinatura. Por isso a tese foi outra: **usar a plataforma paga só como fonte dos nomes e construir todo o resto com dado oficial**.

O diferencial está na camada de decisão, que nenhuma plataforma entrega pronta para um fabricante ou distribuidor específico:

| A plataforma responde | O sistema responde |
|---|---|
| Quem importou | Quem, entre eles, **pode comprar de outro** (sem a importação cativa, da filial que compra da matriz) |
| Quanto importou | **Quando** comprar: quem está atrasado na reposição, quem parou, quem entrou, quem cresce |
| De que país | **Quanto custa de fato importar** o produto, e qual o teto de preço nacional que vence a importação |
| A lista em planilha | A **missão comercial** com o porquê, medida por Precision@K |

Uma frase guiou o desenho: **a plataforma termina no download; o sistema termina no pedido.**

## 3. Arquitetura em três camadas

```mermaid
flowchart TB
  subgraph A["Camada automática: todos os NCMs, só dado oficial"]
    CS[Comex Stat nacional<br/>mercado, preço, frete, via, país] --> R[Custo líquido e<br/>teto por NCM]
    TEC[TEC e anexos<br/>Res. Gecex 272/2021] --> R
    TIPI[TIPI] --> R
    PTAX[PTAX] --> R
    R --> RK[Ranking de oportunidades<br/>com nota]
  end
  subgraph P["Camada profunda: NCMs escolhidos"]
    L[Listas mensais da<br/>plataforma paga] --> C[CNPJ casado na<br/>base aberta da Receita]
    C --> M[Cativo, papel, sinais,<br/>preço de referência, teto<br/>e margem por importador]
  end
  subgraph G["Ação e medição"]
    MI[Missão com o porquê] --> PK[Precision@K por origem]
  end
  RK -- "escolhe onde gastar a cota" --> L
  M --> MI
```

A camada automática roda sozinha todo mês. A profunda só vale a pena nos NCMs que a primeira apontar, porque cada um custa downloads da cota.

## 4. O que os dados revelaram

Os números abaixo são do NCM 4009.31.00 (mangueira de borracha com reforço têxtil), escolhido como piloto.

- **A consulta anual enganava.** Baixada para 12 meses de uma vez, a lista cobria só **35% do valor**, por causa do teto de 1.000 linhas. Baixada mês a mês, cada lista cobre de 86% a quase 100% do mês. O mercado real era de cerca de US$ 56 a 60 milhões em 12 meses, não os US$ 13 milhões que a tela anual mostrava.
- **Quase metade do mercado é cativo.** Cruzando a marca de cada importador com a lista de exportadores, **47% do valor importado é a filial brasileira comprando da própria matriz**: fabricantes de máquinas, montadoras e sistemistas. O primeiro lugar do ranking da plataforma não é cliente de ninguém nesse produto. O mercado endereçável ficou em US$ 29,8 milhões, em 713 empresas.
- **As tradings estavam escondidas.** Empresas que importam para terceiros tinham ficado fora do top 1.000 de um motor que montei antes só com dado aberto, e entraram entre as 10 primeiras quando o papel de cada uma passou a vir do CNAE.
- **A média do NCM mente para quem vende a granel.** A média nacional era US$ 7,65/kg porque inclui mangueira montada de montadora. Entre distribuidores, tradings e fabricantes, a mediana era US$ 4,63/kg. São tetos de preço muito diferentes: R$ 69,61/kg contra R$ 42,13/kg.

## 5. O custo de importar e o teto de preço nacional

O cálculo que virou o centro do sistema:

```text
custo líquido  = CIF + Imposto de Importação + AFRMM + despesas aduaneiras
desembolso     = custo líquido + IPI + PIS + Cofins + ICMS (por dentro)
teto nacional  = custo líquido / ((1 − ICMS da venda) × (1 − crédito de PIS/Cofins))
```

Quem importa no regime normal recupera IPI, PIS, Cofins e ICMS da importação como crédito. A comparação justa com a compra nacional, que também gera crédito, é pelo custo líquido. O **teto** é o preço na nota, com ICMS e sem IPI, abaixo do qual comprar no Brasil sai mais barato do que importar.

| 1 kg de 4009.31.00 a granel (US$ 4,63, PTAX R$ 5,5146) | R$ |
|---|---|
| CIF (frete de 5,79% e seguro de 0,075%, do Comex Stat) | 27,03 |
| Imposto de Importação (12,6%) | 3,41 |
| AFRMM e despesas aduaneiras | 0,92 |
| **Custo líquido** | **31,35** |
| Desembolso, com os tributos recuperáveis | 44,51 |
| **Teto de preço nacional** | **42,13** |

Como a fabricante vende por metro e não por kg, o peso por metro de cada bitola, calculado pela seção da parede e calibrado em fichas de fabricantes, converte o teto: a mangueira SAE 100R6 de 1/2" fica em R$ 9,56 por metro.

Uma decisão de produto saiu de uma conversa com o dono: **o preço de venda flutua e não é o que importa. O produto é o teto.** O preço próprio ficou opcional, e o sistema escalou para todos os NCMs sem exigir cadastro manual.

## 6. Como se mede

- **Casamento dos nomes com o CNPJ:** 853 de 868 importadores casados com a base aberta da Receita pela razão social. A validação contra os CNPJs de uma amostra de embarques acertou 99 de 99.
- **Motor por dado aberto contra a lista paga:** o motor rateia o valor importado por município entre as empresas compatíveis. Calibrado em metades, sem citar nenhuma empresa, ele levou o valor coberto pelo top 200 de 44% para 49%. A validação foi feita na metade que não entrou no ajuste.
- **Alíquotas:** as alíquotas oficiais carregadas automaticamente, de 10.522 NCMs, bateram com a conferência manual numa base fiscal assinada.
- **Uso:** cada missão nasce com origem e lote, e o vendedor marca se o lead fazia sentido. O Precision@K por origem (regras, IA, sinais) é a métrica que decide se a ferramenta acerta.

## 7. Números

| Medida | Valor |
|---|---|
| NCMs com custo e teto calculados | 8.782 (US$ 294,6 bi importados em 12 meses) |
| Linhas do Comex Stat processadas por carga | 6,75 milhões, em cerca de 30 s |
| Alíquotas oficiais carregadas | 10.522 NCMs |
| Cálculo do ranking completo | cerca de 1,5 s |
| Importadores no piloto | 868, 853 com CNPJ |
| Mercado cativo no piloto | 47% |
| Do pedido ao deploy | cerca de 1 dia |

## 8. Lições

- **Primeiro o dado, depois a tela.** A descoberta mais valiosa, de que a consulta anual cobria só 35% do valor, veio de somar planilhas, não de construir interface.
- **Dado licenciado e dado aberto em compartimentos separados.** O que vem da assinatura fica dentro do sistema. O que é aberto virou uma [ferramenta pública](https://github.com/thiagogardev/custo-importacao-br). A separação também definiu o produto possível: um cliente só pode carregar as planilhas da própria assinatura.
- **Média esconde segmento.** Mercado endereçável, papel por CNAE e referência a granel existem porque a média do NCM misturava montadora com distribuidor.
- **Infraestrutura também é produto.** O servidor oficial do Comex Stat entrega a cadeia TLS sem o certificado intermediário. O Windows completa sozinho, e o Linux de produção recusava a conexão. A correção foi incluir o intermediário como autoridade adicional, sem desligar a verificação.
- **O usuário redefine o produto.** A ideia de margem exigia o preço de venda. O dono mostrou que o preço flutua, e o teto virou o resultado principal.

## 9. Próximos passos

1. **Uso medido:** 2 a 4 semanas de missões avaliadas nas linhas do grupo, e listas mensais de mais dois ou três NCMs escolhidos pelo ranking.
2. **Proposta com números:** levar custo, teto e margem para a proposta de substituição da importação, com redação por LLM.
3. **Alerta ativo:** avisar quando aparecer um sinal novo depois da carga mensal.
4. **Inteligência de compra:** preço por kg por exportador, para a importadora negociar e escolher fornecedor.
5. **Ciclo fechado:** registrar o desfecho comercial e recalibrar a nota com ele.
6. **Preço na ponta:** trazer o preço pago em licitações públicas, para ter as três camadas de preço (FOB, custo do importador, preço final).

## 10. Stack e código

- **Sistema:** Next.js 16, React 19, Drizzle, PostgreSQL (PGlite no desenvolvimento), node-cron, Docker, servidor na AWS em São Paulo.
- **Fontes oficiais:** Comex Stat (MDIC), TEC e anexos da Res. Gecex 272/2021 (MDIC/Camex), TIPI (Receita Federal), PTAX (Banco Central), base aberta do CNPJ e habilitados no Siscomex (Receita Federal).
- **Código:** o sistema é privado, e o código pode ser mostrado em entrevista ou avaliação com dados sintéticos. A parte de dado aberto é pública, com licença MIT: [thiagogardev/custo-importacao-br](https://github.com/thiagogardev/custo-importacao-br).
- **Ver também:** [case do Genesis](genesis.md), o sistema de prospecção do qual este é um módulo, e [case ICMS ST](icms-st.md), da mesma família de dado aberto fiscal.
