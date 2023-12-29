# /DE 0 - Introdução ao PySpark/DE 0.05 - Agregação
<hr>--i18n-3fbfc7bd-6ef2-4fea-b8a2-7f949cd84044
# Agregação
# MAGIC
##### Objetivos
1. Agrupar dados por colunas especificadas
1. Aplicar métodos de dados agrupados para agregar dados
1. Aplicar funções integradas para agregar dados
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html" target="_blank">DataFrame</a>: **`groupBy`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/grouping.html" target="_blank" target="_blank">Dados agrupados</a>: **`agg`**, **`avg`**, **`count`**, **`max`**, **`sum`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html" target="_blank">Funções integradas</a>: **`approx_count_distinct`**, **`avg`**, **`sum`**

<hr>--i18n-88095892-40a1-46dd-a809-19186953d968
# MAGIC
Vamos usar o conjunto de dados de eventos BedBricks.

<hr>--i18n-a04aa8bd-35f0-43df-b137-6e34aebcded1
# MAGIC
### Agrupando dados
# MAGIC
<img src="https://files.training.databricks.com/images/aspwd/aggregation_groupby.png" width="60%" />

<hr>--i18n-cd0936f7-cd8a-4277-bbaf-d3a6ca2c29ec
# MAGIC
### groupBy
Utilize o método **`groupBy`** do DataFrame para criar um objeto de dados agrupado. 
# MAGIC
Esse objeto de dados agrupado é chamado de **`RelationalGroupedDataset`** em Scala e **`GroupedData`** em Python.

<hr>--i18n-7918f032-d001-4e38-bd75-51eb68c41ffa
# MAGIC
### Métodos de dados agrupados
Há vários métodos de agregação disponíveis no objeto <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/grouping.html" target="_blank">GroupedData</a>.
# MAGIC
# MAGIC
| Método | Descrição |
| --- | --- |
| agg | Calcula valores agregados especificando uma série de colunas agregadas |
| avg | Calcula o valor médio de cada coluna numérica no grupo |
| count | Conta o número de linhas de cada grupo |
| max | Calcula o valor máximo de cada coluna numérica no grupo |
| mean | Calcula o valor médio de cada coluna numérica no grupo |
| min | Calcula o valor mínimo de cada coluna numérica no grupo |
| pivot | Dinamiza uma coluna do DataFrame atual e executa a agregação especificada |
| sum | Calcula a soma de cada coluna numérica no grupo |

<hr>--i18n-bf63efea-c4f7-4ff9-9d42-4de245617d97
# MAGIC
Aqui estamos obtendo a receita média de compra de cada um.

<hr>--i18n-b11167f4-c270-4f7b-b967-75538237c915
E aqui, estamos obtendo a quantidade total e a soma da receita de compra de cada combinação de estado e cidade.

<hr>--i18n-62a4e852-249a-4dbf-b47a-e85a64cbc258
# MAGIC
## Funções integradas
Além dos métodos de transformação de DataFrame e Coluna, há muitas funções úteis no módulo de <a href="https://docs.databricks.com/spark/latest/spark-sql/language-manual/sql-ref-functions-builtin.html" target="_blank">funções do SQL</a> integrado do Spark.
# MAGIC
O Scala usa <a href="https://spark.apache.org/docs/latest/api/scala/org/apache/spark/sql/functions$.html" target="_blank">**`org.apache.spark.sql.functions`**</a>, e o Python, <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html#functions" target="_blank">**`pyspark.sql.functions`**</a>. As funções deste módulo devem ser importadas para seu código.

<hr>--i18n-68f06736-e457-4893-8c0d-be83c818bd91
# MAGIC
### Funções de agregação
# MAGIC
A seguir, são listadas algumas das funções integradas disponíveis para agregação.
# MAGIC
| Método | Descrição |
| --- | --- |
| approx_count_distinct | Retorna o número aproximado de itens distintos em um grupo |
| avg | Retorna a média dos valores de um grupo |
| collect_list | Retorna uma lista de objetos com duplicatas |
| corr | Retorna o coeficiente de correlação de Pearson para duas colunas |
| max | Calcule o valor máximo para cada coluna numérica de cada grupo |
| mean | Calcule o valor médio de cada coluna numérica para cada grupo |
| stddev_samp | Retorna o desvio padrão da amostra da expressão em um grupo |
| sumDistinct | Retorna a soma dos valores distintos na expressão |
| var_pop | Retorna a variação populacional dos valores em um grupo |
# MAGIC
Use o método de dados agrupados <a href="https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.GroupedData.agg.html#pyspark.sql.GroupedData.agg" target="_blank">**`agg`**</a> para aplicar funções agregadas integradas
# MAGIC
Isso permite aplicar outras transformações nas colunas resultantes, como <a href="https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.Column.alias.html" target="_blank">**`alias`**</a>.

<hr>--i18n-875e6ef8-fec3-4468-ab86-f3f6946b281f
# MAGIC
Aplique múltiplas funções de agregação em dados agrupados

<hr>--i18n-6bb4a15f-4f5d-4f70-bf50-4be00167d9fa
# MAGIC
### Funções matemáticas
A seguir, são listadas algumas das funções integradas para operações matemáticas.
# MAGIC
| Método | Descrição |
| --- | --- |
| ceil | Calcula o teto de uma determinada coluna. |
| cos | Calcula o cosseno de um determinado valor. |
| log | Calcula o logaritmo natural de um determinado valor. |
| round | Retorna o valor da coluna arredondado para 0 casas decimais com o modo de arredondamento HALF_UP. |
| sqrt | Calcula a raiz quadrada do valor flutuante especificado. |

<hr>--i18n-d03fb77f-5e4c-43b8-a293-884cd7cb174c
# MAGIC
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

