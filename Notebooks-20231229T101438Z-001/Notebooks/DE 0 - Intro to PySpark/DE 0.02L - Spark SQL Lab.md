# /DE 0 - Introdução ao PySpark/DE 0.02L - Laboratório do Spark SQL
<hr>--i18n-da4e23df-1911-4f58-9030-65da697d7b61
# Laboratório do Spark SQL
# MAGIC
##### Tarefas
1. Criar um DataFrame a partir da tabela **`events`**
1. Exibir o DataFrame e inspecionar seu esquema
1. Aplicar transformações para filtrar e classificar eventos **`macOS`**
1. Contar os resultados e utilizar as primeiras cinco linhas
1. Criar o mesmo DataFrame usando uma query SQL
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/spark_session.html" target="_blank">SparkSession</a>: **`sql`**, **`table`**
- Transformações do <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html" target="_blank">DataFrame</a>: **`select`**, **`where`**, **`orderBy`**
- Ações do <a href="https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.html" target="_blank">DataFrame</a>: **`select`**, **`count`**, **`take`**
- Outros métodos do <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html" target="_blank">DataFrame</a>: **`printSchema`**, **`schema`**, **`createOrReplaceTempView`**

<hr>--i18n-e0f3f405-8c97-46d1-8550-fb8ff14e5bd6
# MAGIC
### 1. Crie um DataFrame a partir da**`events`** tabela
- Use a SparkSession para criar um DataFrame a partir da tabela **`events`**

<hr>--i18n-fb5458a0-b475-4d77-b06b-63bb9a18d586
# MAGIC
### 2. Exibir o DataFrame e inspecionar o esquema
- Use os métodos acima para inspecionar o conteúdo e o esquema do DataFrame

<hr>--i18n-76adfcb2-f182-485c-becd-9e569d4148b6
# MAGIC
### 3. Aplicar transformações para filtrar e classificar**`macOS`** eventos
- Filtre por linhas onde **`device`** é **`macOS`**
- Classifique as linhas por **`event_timestamp`**
# MAGIC
<img src="https://files.training.databricks.com/images/icon_hint_32.png" alt="Hint"> Use aspas simples e duplas na expressão SQL de filtro

<hr>--i18n-81f8748d-a154-468b-b02e-ef1a1b6b2ba8
# MAGIC
### 4. Conte os resultados e utilize as primeiras cinco linhas
- Use ações do DataFrame para contar e utilizar linhas

<hr>--i18n-4e340689-5d23-499a-9cd2-92509a646de6
# MAGIC
**4.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-cbb03650-db3b-42b3-96ee-54ea9b287ab5
# MAGIC
### 5. Criar o mesmo DataFrame usando uma query SQL
- Use a SparkSession para executar uma query SQL na tabela **`events`**
- Use comandos SQL para escrever o mesmo filtro e classificar a query usada anteriormente

<hr>--i18n-1d203e4e-e835-4778-a245-daf30cc9f4bc
# MAGIC
**5.1: VERIFIQUE SEU TRABALHO**
- Você só deve ver valores **`macOS`** na coluna **`device`**
- A quinta linha deve ser um evento com timestamp **`1592539226602157`**

<hr>--i18n-5b3843b3-e615-4dc6-aec4-c8ce4d684464
# MAGIC
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

