# /DE 0 - Introdução ao PySpark/DE 0.11 - Funções adicionais
<hr>--i18n-df421470-173e-44c6-a85a-1d48d8a14d42
# Funções adicionais
# MAGIC
##### Objetivos
1. Aplicar funções integradas para gerar dados para novas colunas
1. Aplicar funções NA do DataFrame para lidar com valores nulos
1. Unir DataFrames
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.join.html#pyspark.sql.DataFrame.join" target="_blank">Métodos do DataFrame </a>: **`join`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrameNaFunctions.html#pyspark.sql.DataFrameNaFunctions" target="_blank">DataFrameNaFunctions</a>: **`fill`**, **`drop`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html" target="_blank">Funções integradas</a>:
  - Agregação: **`collect_set`**
  - Coleção: **`explode`**
  - Não agregação e diversos: **`col`**, **`lit`**

<hr>--i18n-c80fc2ec-34e6-459f-b5eb-afa660db9491
# MAGIC
### Funções de não agregação e diversas
A seguir, são listadas algumas funções integradas adicionais de não agregação e diversas.
# MAGIC
| Método | Descrição |
| --- | --- |
| col / column | Retorna uma coluna com base no nome de coluna determinado. |
| lit | Cria uma coluna de valor literal. |
| isnull | Retorna verdadeiro quando a coluna é nula. |
| rand | Gere uma coluna aleatória com amostras independentes e identicamente distribuídas (i.i.d.) de maneira uniforme em [0.0, 1.0]. |

<hr>--i18n-bae51fa6-6275-46ec-854d-40ba81788bac
# MAGIC
Podemos selecionar uma coluna específica usando a função **`col`**.

<hr>--i18n-a88d37a6-5e98-40ad-9045-bb8fd4d36331
# MAGIC
A função **`lit`** pode ser usada para criar uma coluna a partir de um valor, o que é útil para adicionar colunas.  

<hr>--i18n-d7436832-7254-4bfa-845b-2ab10170f171
# MAGIC
### DataFrameNaFunctions
O <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrameNaFunctions.html#pyspark.sql.DataFrameNaFunctions" target="_blank">DataFrameNaFunctions</a> é um submódulo do DataFrame com métodos para lidar com valores nulos. É possível obter uma instância de DataFrameNaFunctions acessando o atributo **`na`** de um DataFrame.
# MAGIC
| Método | Descrição |
| --- | --- |
| drop | Retorna um novo DataFrame omitindo linhas com qualquer, todos ou um número especificado de valores nulos, considerando um subconjunto opcional de colunas |
| fill | Substitui os valores nulos pelo valor especificado em um subconjunto opcional de colunas |
| replace | Retorna um novo DataFrame substituindo um valor por outro valor, considerando um subconjunto opcional de colunas |

<hr>--i18n-da0ccd36-e2b5-4f79-ae61-cb8252e5da7c
Esta é uma contagem de linhas antes e depois de descartar linhas com valores nulos/não disponíveis.  

<hr>--i18n-aef560b8-7bb6-4985-a43d-38541ba78d33
Como as contagens de linhas são iguais, não temos colunas nulas.  Será necessário explodir os itens para encontrar valores nulos em colunas como items.coupon.  

<hr>--i18n-4c01038b-2afa-41d8-a390-fad45d1facfe
# MAGIC
Podemos preencher os códigos de cupom ausentes com **`na.fill`**.

<hr>--i18n-8a65ceb6-7bc2-4147-be66-71b63ae374a1
# MAGIC
### Unindo DataFrames
O método <a href="https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.join.html?highlight=join#pyspark.sql.DataFrame.join" target="_blank">**`join`**</a> do DataFrame une dois DataFrames com base em uma determinada expressão join. 
# MAGIC
Há vários tipos diferentes de junções compatíveis:
# MAGIC
A junção interna baseada em valores iguais de uma coluna compartilhada chamada de "nome" (ou seja, uma equi-junção ou junção por igualdade)<br/>
**`df1.join(df2, "name")`**
# MAGIC
Uma junção interna baseada em valores iguais das colunas compartilhadas chamadas de "name" e "age"<br/>
**`df1.join(df2, ["name", "age"])`**
# MAGIC
Uma full outer join baseado em valores iguais de uma coluna compartilhada chamada de "name"<br/>
**`df1.join(df2, "name", "outer")`**
# MAGIC
Um left outer join com base em uma expressão de coluna explícita<br/>
**`df1.join(df2, df1["customer_name"] == df2["account_name"], "left_outer")`**

<hr>--i18n-67001b92-91e6-4137-9138-b7f00950b450
Carregaremos os dados dos usuários e os uniremos às gmail_accounts acima.

<hr>--i18n-9a47f04c-ce9c-4892-b457-1a2e57176721
# MAGIC
### Limpar sala de aula

