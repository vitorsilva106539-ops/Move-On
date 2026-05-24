# Tabela Fato Produção

CREATE OR REPLACE TABLE `moveon-497301.tb_moveon.ft_producao` AS
SELECT 
  string_field_0 as id_producao,
  SAFE.PARSE_DATE('%m/%d/%Y', string_field_1) as data_producao,
  string_field_2 as colaborador,
  string_field_3 as materia_prima,
  string_field_4 as produto_final,
  SAFE_CAST(string_field_5 AS INT64) as qtd_produzida
FROM `moveon-497301.tb_moveon.moveOn_prod`
WHERE string_field_0 != 'id' AND string_field_0 IS NOT NULL;

# Tabela Fato Vendas

CREATE OR REPLACE TABLE `moveon-497301.tb_moveon.ft_vendas` AS
SELECT 
  string_field_0 as id_venda,
  -- Tratando a data: use %d/%m/%Y porque no print de vendas está 15/07/2023
  SAFE.PARSE_DATE('%d/%m/%Y', string_field_1) as data_venda,
  string_field_2 as nome_cliente,
  string_field_3 as materia_prima,
  string_field_4 as produto_final,
  -- Preço Unitário (Coluna F)
  SAFE_CAST(REPLACE(REPLACE(REPLACE(string_field_5, 'R$ ', ''), '.', ''), ',', '.') AS FLOAT64) as preco_unitario,
  -- Quantidade (Coluna G)
  SAFE_CAST(string_field_6 AS INT64) as qtd_vendida,
  -- Total Venda (Coluna H)
  SAFE_CAST(REPLACE(REPLACE(REPLACE(string_field_7, 'R$ ', ''), '.', ''), ',', '.') AS FLOAT64) as valor_total_venda,
  string_field_8 as cidade_cliente,
  string_field_9 as endereco_cliente
FROM `moveon-497301.tb_moveon.moveOn_vend`
WHERE string_field_0 != 'id' AND string_field_0 IS NOT NULL;
