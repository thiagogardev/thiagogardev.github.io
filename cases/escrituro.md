---
title: "Escrituro: do mercado ao produto no ar em dois dias"
description: Case do Escrituro, sistema para escritórios de contabilidade construído para igualar e superar o Contmatic Phoenix. Pesquisa com fontes datadas, definição revisada por agente independente, fundação publicada em produção, tudo em dois dias com agentes de IA sob direção humana.
---

# Escrituro: do mercado ao produto no ar em dois dias

**Período:** novembro de 2026 até hoje · **Papel:** idealização, direção da pesquisa, arquitetura, código, verificação e publicação · **Estado:** Fase 0 no ar; Fase 1 em construção, módulo Contábil publicado

> Resumo em uma frase: em dois dias de trabalho, saí de uma pergunta ("dá para construir um sistema para escritórios de contabilidade igual ou melhor que o Contmatic Phoenix?") para um produto com nome, domínio registrado, pesquisa de mercado com 659 fontes datadas, definição de produto revisada por um agente independente e a primeira fase do sistema funcionando em produção, com domínio, certificado e backup. No mesmo dia, o módulo Contábil da Fase 1 entrou no ar, construído sobre um inventário de 233 funções dos sistemas líderes.

## 1. Contexto e problema

No dia anterior eu tinha construído o [Clinalta](clinalta.md), um sistema para clínicas, em um dia, para testar se o método que usei no ERP do meu grupo industrial servia a um produto comercial. Serviu. A pergunta seguinte foi se ele aguentava um mercado maior, mais regulado e com um produto de sete módulos que conversam entre si: o escritório de contabilidade.

A régua escolhida foi o Contmatic Phoenix, o sistema de escritório mais conhecido do Brasil. Três motivos para o alvo: o meu grupo paga um escritório externo, e seus sete CNPJs servem de primeiro cliente real; fiscal, contábil, folha, patrimônio, legal, honorários e portal exigem arquitetura, não só telas; e a reforma tributária (CBS e IBS informativos em 2026, CBS plena e Simples híbrido em 2027) obriga todos os fornecedores a reescrever o motor fiscal, o que abre uma janela para quem nasce sem legado.

Duas condições que eu coloquei: não fazer "mais um", entendendo antes como os sete líderes funcionam; e apostar no visual cinza, denso e governado por teclado do Phoenix, contra a maré do flat branco, porque é o que o analista de escritório conhece e porque densidade é produtividade.

## 2. Método em cinco etapas

As quatro etapas do Clinalta ganharam uma quinta, a revisão independente da definição antes de qualquer código, e a verificação passou a incluir a publicação. Cada etapa tem critério de saída; nada segue sem passar por ele.

```mermaid
flowchart LR
  P[1. Pesquisa<br/>6 frentes em paralelo<br/>fonte e data em tudo] --> S[2. Síntese<br/>relatório de mercado<br/>definição de produto]
  S --> R{3. Revisão<br/>agente independente<br/>31 pontos}
  R -- corrige --> S
  R -- aprovada --> C[4. Construção<br/>Fase 0: fundação<br/>reaproveita Clinalta e ERP]
  C --> V{5. Verificação<br/>148 asserções, navegador,<br/>produção na VPS}
  V -- falhou --> C
  V -- passou --> OK[No ar<br/>domínio e backup]
```

1. **Pesquisa em seis frentes paralelas.** Seis agentes, cada um com um recorte e perguntas fechadas: líderes de mercado; fiscal e reforma tributária; contábil, patrimônio e legal; folha e eSocial; dores, preços e integrações; e o padrão visual do Phoenix, com cores medidas pixel a pixel nas capturas. Regra: toda afirmação com fonte e data, lacuna declarada em vez de inventada.
2. **Síntese em dois documentos.** Um relatório de mercado com uma tabela obrigação x leiaute x prazo x fonte e doze lacunas ordenadas, e uma definição de produto em onze seções: visão, personas, módulos por fase, normas como requisitos, integrações, arquitetura, padrão visual, modelo comercial, roteiro, riscos e nomes.
3. **Revisão independente.** Um agente que não escreveu a definição a conferiu contra as notas e o relatório. Apontou 31 problemas, todos corrigidos: fases desalinhadas, critérios de pronto amarrados a datas de 2027, LALUR citado no Lucro Presumido, cota de IA por empresa que custaria mais que a mensalidade, política "nunca bloquear" sem definição exata.
4. **Construção da Fase 0.** Reaproveitei do Clinalta e do ERP o que já estava provado (login, 2FA, sessões, auditoria, unidade de isolamento) e escrevi o resto: carteira de empresas, motor de obrigações, catálogo com fonte, tarefas, protocolos, fila, biblioteca de componentes Phoenix.
5. **Verificação e publicação.** Tipos e lint limpos, 148 asserções em quatro suítes, fluxo completo no navegador e, no dia seguinte, publicação na VPS com domínio, certificado, backup diário e banco incluído na rotina de dumps.

O que ficou comigo: a pergunta, as restrições, a leitura crítica, as decisões de produto e a aceitação. O que ficou com a IA: volume, paralelismo, consistência e a revisão cruzada.

## 3. O que a pesquisa revelou

A disputa no escritório de contabilidade não é por função. Os sete líderes (Contmatic, Domínio, Alterdata com Prosoft, Fortes, Questor, SCI, Makro) cobrem o tripé fiscal, contábil e folha há décadas. A diferença está em desempenho, relação comercial, previsibilidade das atualizações e na reforma tributária que ninguém absorveu de forma nativa.

| Achado | Evidência (04/11/2026) | O que muda no produto |
|---|---|---|
| "Nuvem" é virtualização de aplicativo Windows | Phoenix Cloud e Domínio Web entregam programas Delphi por virtualização; relatos de "lerdo demais", "cai muito", ECF de quatro horas num servidor de 64 GB | Web nativa com a densidade e o teclado do desktop, SPED gerado em segundo plano com fila visível |
| Contratos prendem e preço é sob consulta | Fidelidade de 12 meses com multa de 100%, IGP-M com piso de 4%, dados removidos na rescisão; só a Makro publica tabela | Tabela pública por faixa de empresas, sem fidelidade, exportação completa inclusive após o cancelamento |
| Atualização quebra o que funcionava | Regressões no SPED da Contmatic (07/2026) e na NF da Fortes (06/2026); eSocial trocou de esquema quatro vezes em quatro meses; nenhum fornecedor publica quais leiautes suporta | Página pública de conformidade por obrigação, versão, nota técnica e vigência; regras parametrizadas por período de apuração, não por versão do software |
| A reforma é módulo colado | EFD ICMS/IPI e EFD-Contribuições não apuram CBS e IBS; Simples híbrido em 2027; CNPJ alfanumérico em todos os leiautes | IBS e CBS modelados no documento, na apuração e no plano de contas desde o início; CNPJ como texto em todo o banco |
| A automação federal tem preço conhecido | Integra Contador custa de R$ 3 a R$ 5 por empresa ao mês; procurações e-CAC expiram em cinco anos | Carteira de certificados e procurações com alerta de vencimento desde a Fase 0; custo do Serpro repassado sem margem oculta |
| Gestão do escritório é "ferramenta paralela" | Usuários do Domínio precisam de outra ferramenta; Acessórias, Nibo e Gestta vivem dessa lacuna | Agenda de obrigações gerada por regra, protocolo, tarefas, honorários e portal no núcleo do produto |
| O Phoenix é cinza, denso e governado por teclado | Fundo #F0F0F0, cabeçalho de grade #808080, seleção #0078D7, Segoe UI 12 px, 18 px por linha, raio zero, Ctrl+Enter abre consulta | Biblioteca de componentes com esses tokens medidos, não "inspirada" neles |

Uma lição de processo: a revisão independente pegou erros que eu mesmo teria deixado passar. Dois exemplos: o LALUR só existe no Lucro Real, e o MIT da DCTFWeb é um JSON que o sistema gera, não um arquivo que ele lê.

## 4. Decisões de produto

Oito princípios inegociáveis, cada um ligado a uma dor comprovada. Entraram na definição antes da primeira linha de código.

- **Os dados são do escritório e do cliente dele.** Exportação total a um clique: arquivos SPED, eventos eSocial com recibos, leiautes TXT dos concorrentes, CSV de tudo.
- **Nunca bloquear por cobrança.** Inadimplência nunca para o que tem prazo legal; o que para, depois de 60 dias, é abrir empresa, usuário e competência novos.
- **Preço público e previsível.** Cinco faixas, de R$ 149 por mês para até 10 empresas a R$ 2.490 para até 500, todos os módulos, usuários ilimitados, sem fidelidade, reajuste só pelo IPCA.
- **Rápido e denso.** Página em menos de um segundo, grade de 18 px por linha, Enter avança, Esc cancela, Alt+letra aciona o botão.
- **Nada se perde.** Lançamento, documento e evento nunca sofrem alteração de conteúdo; correção gera estorno ou nova versão com motivo; auditoria em toda tabela.
- **Conformidade por leiaute versionado e pública.** Cada obrigação tem versão com vigência e o sistema escolhe pela competência; uma página pública lista o que é suportado.
- **Relacionamento dentro do sistema.** Portal, WhatsApp, protocolo, tarefas e honorários fazem parte do produto.
- **Suporte humano com prazo.** Chamado com número, resposta no mesmo dia útil, notas de versão antes de cada atualização, janela fora do fechamento.

Três decisões de escopo: começar pelo contábil e pelo fiscal do Simples, com as empresas do meu grupo como primeiro cliente; deixar as declarações estaduais legadas para a carteira real, não para o catálogo; e manter o visual Phoenix contra a maré, testando as cinco telas principais com contadores ao lado das capturas originais.

## 5. O que foi construído na Fase 0

A fundação: o que todas as fases vão usar e o que um escritório já consegue operar para controlar prazos. O escritório é a unidade de isolamento; toda tabela carrega o seu id, e a empresa cliente e a competência ativas vivem na sessão, como no Phoenix.

| Módulo | O que faz | Origem |
|---|---|---|
| Acesso | Login com senha, verificação em duas etapas, sessões por aparelho, desconexão remota | Reaproveitado do Clinalta |
| Escritório e perfis | Um login, um perfil por escritório (administrador, gerente, fiscal, contábil, folha, legal, financeiro, consulta), restrito a todas as empresas ou às marcadas; compromisso de acesso do suporte escrito na ajuda | Adaptado: a clínica virou escritório, e a empresa cliente é um segundo nível |
| Carteira | Empresas com apelido, CNPJ numérico ou alfanumérico validado, regime por vigência (mudar abre nova linha, nunca sobrescreve), indicadores que ligam obrigações (funcionários, ICMS, ST, IPI, ISS, retenções), analistas responsáveis, certificado A1 cifrado, procurações com alerta de vencimento | Novo |
| Agenda de obrigações | Catálogo de 20 obrigações federais com norma, leiaute e versão; regras de prazo como funções puras (dia fixo, último dia útil, enésimo dia útil, data anual, 13º) com feriados nacionais e bancários e Páscoa calculada; posterga ou antecipa; geração por competência idempotente; situações com recibo e dispensa; avanço em lote; fechamento por módulo | Novo |
| Tarefas e protocolos | Pendências com responsável, prioridade e prazo; protocolos de entrega e recebimento com numeração sequencial e via para assinatura | Novo |
| Fila de trabalho | Rotinas longas enfileiradas com situação visível, preparadas para a geração de SPED em segundo plano | Novo |
| Painel e relatórios | Obrigações da competência por situação, próximos sete dias, carga por analista, vencimentos | Novo |
| Auditoria e exportação | Toda alteração campo a campo; acessos por empresa; nove tabelas em CSV, na hora | Reaproveitado |
| Padrão visual Phoenix | Tokens medidos, cerca de 40 ícones coloridos, menus de texto em cascata, toolbar com rótulos, janela com barra de status, faixa "Base ativa, Empresa ativa, Competência", teclado primeiro | Novo |
| Infra | Docker com Postgres e backup diário, rota de saúde, ajuda por capítulo, página de conformidade, escritório de demonstração | Reaproveitado |

### Telas do escritório de demonstração

Os dados abaixo são do escritório de demonstração, com as empresas do grupo cadastradas pelo script de seed.

![Painel da carteira: obrigações da competência por situação, próximos sete dias e carga por analista](../assets/escrituro/painel.png)

*Painel da carteira. A faixa superior mostra a base, a empresa e a competência ativas, como no Phoenix; os contadores são calculados sobre a agenda gerada.*

![Agenda de obrigações da competência 10/2026 para a empresa ativa, com prazo calculado em dias úteis](../assets/escrituro/obrigacoes.png)

*Agenda de obrigações. O PGDAS-D caiu em 23/11 porque 20/11 é feriado e a regra posterga; o FGTS Digital foi antecipado para 19/11 pela regra inversa. Cada linha abre a obrigação para recibo, protocolo ou dispensa.*

![Cadastro de empresa em abas, com histórico de regime, certificado digital e procurações eletrônicas](../assets/escrituro/empresa.png)

*Cadastro de empresa. Campos obrigatórios em vermelho, abas, histórico de regime por vigência, certificado A1 e procurações e-CAC com data de fim. Nada se apaga: inativar tira das listas e mantém na exportação.*

### Conformidade embutida

A definição mapeou 31 normas e leiautes como requisitos de software, com versão e vigência. As que já moldaram a Fase 0:

| Norma ou leiaute | O que exige | Como está no sistema |
|---|---|---|
| IN RFB 2.237/2024 alterada pela IN 2.248/2025 | DCTFWeb até o último dia útil do mês seguinte | Regra "último dia útil" no catálogo, com fonte |
| Res. CGSN 140/2018 e 183/2025 | PGDAS-D até o dia 20, com multa por atraso desde 01/01/2026 | Regra "dia 20 postergado" e aviso de atraso no painel |
| FAQ do FGTS Digital (Lei 8.036, art. 18) | Vencimento no dia 20, antecipado em feriado nacional ou bancário | Regra "dia 20 antecipado", com feriados bancários no calendário |
| EFD ICMS/IPI leiaute 020 e 021 | Leiaute 021 a partir de 01/01/2027 | Catálogo com vigência por versão; a competência escolhe o leiaute |
| CNPJ alfanumérico (IN RFB 2.229/2024) | Novos CNPJs com letras a partir de julho de 2026 | CNPJ como texto em todo o banco, validação com peso ASCII, formatação única |
| IN RFB 1.751/2017 (procuração e-CAC) | Procuração com prazo, exigida para transmitir pelo escritório | Procurações por empresa com data de fim e alerta 30 dias antes |
| LGPD arts. 37 e 46 | Registro das operações e segurança | Auditoria campo a campo, acessos por empresa, certificado e segredo de 2FA cifrados |

## 6. Fase 1, módulo Contábil: o piso do mercado e as lacunas que ninguém entrega

Com a Fase 0 no ar, a pergunta seguinte foi o que um módulo precisa ter para um contador considerá-lo completo. Em vez de adivinhar, uma segunda pesquisa, em seis frentes paralelas, inventariou função a função o que Contmatic, Domínio, Alterdata, Fortes, Questor, SCI, Makro e IOB documentam em público: 233 funções, classificadas em piso (quatro ou mais fornecedores), diferencial (um a três) e lacuna (ninguém). A Folhamatic, citada como referência, não existe mais: virou IOB Online Sistemas em 2020. O módulo Contábil foi construído a partir dessa lista, com o piso inteiro e as lacunas que cabem na fase.

O método mudou de escala: o núcleo (esquema de dez tabelas, funções puras de plano, partida dobrada, balancete, DRE, balanço e encerramento, leitor de OFX, regras de conciliação e 64 verificações novas) ficou comigo e com um agente; as 23 telas foram divididas entre quatro agentes em paralelo, cada um dono de uma pasta, sem tocar nos arquivos dos outros. A integração, os menus, a ajuda e a verificação ponta a ponta no navegador, com Playwright digitando o lançamento e importando o extrato, fecharam o ciclo.

| Piso do mercado | Como está no Escrituro |
|---|---|
| Plano de contas modelo com referencial da RFB, plano por empresa, código reduzido, natureza por conta | Modelo ITG 1000 com 182 contas em quatro níveis, copiado para cada empresa na primeira abertura; referencial da RFB preenchido na Fase 2 com a tabela oficial, nunca inventado |
| Lançamento com várias partidas, históricos padrão com variáveis, centros de custo, lotes, saldos iniciais | Partidas ilimitadas, Ctrl+M acrescenta linha, Ctrl+Enter ou F5 abre a consulta de conta, débitos e créditos precisam fechar na tela |
| Importação de OFX e conciliação por data, valor e histórico | Leitor OFX sem dependências; sugestão por lançamento existente de mesmo valor em até três dias ou por regra; o analista aprova, o sistema não grava sozinho |
| Balancete, razão, diário, DRE, balanço, apuração e zeramento | Todos em vídeo e impressão; encerramento mostra a simulação antes de gravar |
| Fechamento por competência com bloqueio | Competência fechada bloqueia gravar, excluir e restaurar |

| Lacuna que nenhum fornecedor documenta | Resposta no Escrituro |
|---|---|
| Numeração sequencial por livro conforme a ITG 2000 e estorno vinculado | Número por empresa que nunca se repete; estorno gera lançamento novo ligado ao original nos dois sentidos |
| Reabertura de período com motivo e trilha | Reabrir exige motivo e fica na auditoria, campo a campo |
| Lixeira com restauração | Excluir manda para a lixeira com motivo; restaurar devolve com o mesmo número |
| Regras de conciliação aprendidas das conciliações manuais | A regra nasce da conciliação feita pelo analista, por trecho do histórico, e vale para a empresa ou para todas |
| Painel de consistências antes da ECD | Saldos invertidos, lançamentos sem histórico, contas sem referencial, sem movimento, competências abertas e lixeira, num painel só |

### Telas do módulo Contábil

![Lançamento contábil com duas partidas, seletor de conta e totais fechando](../assets/escrituro/lancamento.png)

*Lançamento contábil. A conta é digitada pelo código reduzido ou escolhida na consulta; a diferença entre débitos e créditos aparece até fechar, e Gravar só libera quando fecha.*

![Conciliação bancária com as seis linhas do extrato OFX de exemplo e as sugestões de lançamento](../assets/escrituro/conciliacao.png)

*Conciliação bancária. Cada linha do extrato traz a sugestão: um lançamento já digitado de mesmo valor ou a regra aprendida; o analista concilia, gera o lançamento ou ignora com motivo.*

![Balancete de verificação de outubro de 2026 com saldo anterior, débitos, créditos e saldo final](../assets/escrituro/balancete.png)

*Balancete de verificação. Sintéticas em negrito somam as analíticas pelo código; o saldo anterior inclui a implantação de saldos; débitos e créditos do período fecham.*

## 7. Números

Tudo entre 4 e 5 de novembro de 2026, da primeira pergunta à publicação.

| Medida | Valor |
|---|---|
| Frentes de pesquisa em paralelo | 6 |
| Palavras de notas de pesquisa | 42.300 |
| Fontes distintas citadas com URL | 659 |
| Relatório de mercado | 8.500 palavras, 12 lacunas ordenadas |
| Definição do produto | 11.600 palavras, 11 seções, 31 normas e leiautes, 20 integrações, 6 fases, 13 riscos |
| Pontos da revisão independente aplicados | 31 |
| Arquivos versionados no commit inicial | 122 |
| Linhas de TypeScript | 7.876 |
| Tabelas no banco | 21 |
| Telas e rotas | 36 |
| Obrigações no catálogo padrão, com fonte | 20 |
| Asserções automatizadas | 148, em 4 suítes |
| Nomes candidatos checados no Registro.br | 10 |
| Tempo da decisão de publicar ao sistema respondendo na VPS | cerca de 30 minutos |

Depois do módulo Contábil, no mesmo 5 de novembro:

| Medida | Valor |
|---|---|
| Funções inventariadas nos sistemas líderes | 233 (103 piso, 97 diferenciais, 33 lacunas), 518 citações |
| Contas no plano modelo | 182, 132 analíticas |
| Tabelas no banco | 30 |
| Telas e rotas | 60, das quais 23 do contábil |
| Linhas de TypeScript | 14.460 |
| Asserções automatizadas | 212, em 5 suítes |
| Agentes construindo telas em paralelo | 4 |

O que não está feito, dito com a mesma clareza: a restauração do backup em produção ainda não foi testada, o pedido de marca no INPI não foi protocolado, o certificado A1 só foi testado sem arquivo real, os prazos de EFD ICMS/IPI e ISS usam os de São Paulo, o catálogo não tem declarações estaduais, o referencial da RFB está em branco e, da Fase 1, faltam a captura de documentos fiscais, o PGDAS-D pelo Integra Contador, honorários e portal.

## 8. Lições

- **Medir vale mais que descrever.** O agente do padrão visual leu as cores das capturas do Phoenix pixel a pixel. Os tokens saíram exatos, e a primeira tela já parecia o original em vez de "inspirada" nele.
- **Revisão independente antes do código.** Trinta e um pontos corrigidos numa definição de 11 mil palavras, por um agente que não a escreveu. É barato comparado a descobrir no código que a cota de IA custaria mais que a mensalidade.
- **Datas externas não esperam o roteiro.** A definição amarrava critérios de pronto a janeiro de 2027. A revisão desamarrou: o leiaute 021 e o simulador do Simples híbrido entram na Fase 2, e a competência 01/2027 pode ser refeita depois a partir dos arquivos.
- **Dia útil é regra de negócio.** PGDAS-D posterga, FGTS Digital antecipa, e feriado bancário conta. O motor de prazos é função pura, testada contra o calendário real de 2026.
- **O domínio certo é o .com.br.** O escrituro.com estava com um especulador desde abril. Para um produto brasileiro, o .com.br registrado direto no Registro.br resolve, depois de desfazer o vínculo do CPF com o provedor anterior.

## 9. Próximos passos

Seis fases, cada uma com critério de pronto verificável e sem datas prometidas. Cada fase só começa com a anterior em uso real.

1. **Fase 0, fundação (no ar).** Falta testar a restauração do backup em produção, protocolar a marca e validar as cinco telas com contadores. Portão: escritório de demonstração com as empresas do grupo, agenda gerada por regime e um analista marcando obrigações com recibo em produção.
2. **Fase 1, MVP contábil e fiscal do Simples (módulo Contábil publicado).** Plano de contas com referencial, lançamentos, conciliação, balancete e fechamento, reaproveitando o controle gerencial do grupo; captura de NF-e e CT-e por NSU com manifestação; NFS-e nacional; PGDAS-D, DAS e DEFIS pelo Integra Contador; apuração informativa de IBS e CBS; honorários e portal do cliente. Portão: um mês inteiro das empresas do grupo escriturado no sistema e um escritório externo pequeno em piloto.
3. **Fase 2, regime normal e SPED.** Apuração de ICMS, ST, DIFAL, IPI, PIS e COFINS; EFD ICMS/IPI 020 e 021 por vigência; EFD-Contribuições; Reinf; DCTFWeb com MIT; ECD com signatários; patrimônio com depreciação dupla. Portão: EFD e ECD de uma empresa do grupo validadas nos programas oficiais e transmitidas.
4. **Fase 3, folha e eSocial.** Eventos S-1.3, cálculo mensal, férias, 13º, rescisão, FGTS Digital e DCTFWeb ponta a ponta. Portão: folha de uma empresa do grupo fechada no sistema com totalizadores conferidos.
5. **Fase 4, legal, IA e carteira completa.** Departamento legal com Redesim, assistente de IA com cota por escritório, aplicativo do empregado. Portão: escritório piloto operando a carteira inteira.
6. **Fase 5, reforma plena e ecossistema.** CBS real, Simples híbrido, split payment, API pública. Portão: competência 01/2027 em diante apurada sem retrabalho.

## 10. Stack

Next.js 16 com server actions · React 19 · TypeScript · Tailwind 4 · Drizzle ORM · PGlite em desenvolvimento e PostgreSQL em produção · Docker na VPS atrás do Caddy. Convenções herdadas do grupo: dinheiro em centavos, datas sempre no fuso de São Paulo, toda tabela com o id do escritório, toda ação grava auditoria, nada se apaga.

## 11. Código

O repositório é privado: o produto é comercial e vai operar dados fiscais, contábeis e trabalhistas de terceiros. Pode ser apresentado em entrevista ou avaliação acadêmica com o escritório de demonstração. A definição do produto, o relatório de mercado e as notas de pesquisa existem no repositório e seguem este case.
