# Tabela dimensão produto

CREATE OR REPLACE TABLE `moveon-497301.tb_moveon.dim_produtos` AS
SELECT DISTINCT 
  produto_final as nome_produto,
  materia_prima
FROM (
  SELECT string_field_4 as produto_final, string_field_3 as materia_prima FROM `moveon-497301.tb_moveon.moveOn_prod`
  UNION DISTINCT
  SELECT string_field_4 as produto_final, string_field_3 as materia_prima FROM `moveon-497301.tb_moveon.moveOn_vend`
)
WHERE produto_final != 'produto_final' AND produto_final IS NOT NULL;

# Tabela dimensão data

CREATE OR REPLACE TABLE `moveon-497301.tb_moveon.dim_calendario` AS
SELECT
  data,
  EXTRACT(YEAR FROM data) AS ano,
  EXTRACT(MONTH FROM data) AS mes,
  EXTRACT(DAY FROM data) AS dia,
  FORMAT_DATE('%B', data) AS nome_mes,
  EXTRACT(QUARTER FROM data) AS trimestre
FROM UNNEST(GENERATE_DATE_ARRAY('2022-01-01', '2026-12-31')) AS data;
