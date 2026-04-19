# Análise de Receita e Custo Diário

## Visão Geral
Este projeto tem como objetivo analisar e comparar a receita gerada por vendas com os custos de publicidade em base diária. A proposta é consolidar diferentes fontes de dados em uma única estrutura analítica, facilitando a análise de desempenho.

## Problema de Negócio
Empresas que investem em marketing precisam entender:
- Quanto faturam por dia
- Quanto gastam com publicidade
- Se o investimento está gerando retorno

Sem essa visibilidade, torna-se difícil tomar decisões estratégicas sobre orçamento e campanhas.

## Solução
Foi desenvolvida uma query SQL que:
- Calcula a receita diária a partir das tabelas de pedidos e produtos
- Calcula o custo diário de publicidade
- Combina as duas métricas em uma única tabela utilizando `UNION ALL`

## Query SQL

```sql
SELECT
    s.date AS date,
    'revenue' AS type,
    ROUND(SUM(p.price), 2) AS value
FROM `data-analytics-mate.DA.order` AS o
JOIN `data-analytics-mate.DA.session` AS s
    ON o.ga_session_id = s.ga_session_id
JOIN `data-analytics-mate.DA.product` AS p
    ON o.item_id = p.item_id
GROUP BY s.date

UNION ALL

SELECT
    psc.date AS date,
    'cost' AS type,
    ROUND(SUM(psc.cost), 2) AS value
FROM `data-analytics-mate.DA.paid_search_cost` AS psc
GROUP BY psc.date

ORDER BY date, type;
