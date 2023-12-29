# /DE 0 - Introdução ao PySpark/DE 0.06L - Laboratório de receita por tráfego
<hr>--i18n-8382e200-81c0-4bc3-9bdb-6aee604b0a8c
# Laboratório de receita por tráfego
Obtenha as três fontes de tráfego que geram a maior receita total.
1. Agregar receita por origem de tráfego
2. Obter as três principais fontes de tráfego por receita total
3. Limpar as colunas de receita para ter duas casas decimais
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html" target="_blank">DataFrame</a>: **`groupBy`**, **`sort`**, **`limit`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/column.html" target="_blank">Coluna</a>: **`alias`**, **`desc`**, **`cast`**, **`operators`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html" target="_blank">Funções integradas</a>: **`avg`**, **`sum`**

<hr>--i18n-b6ac5716-1668-4b34-8343-ee2d5c77cfad
### Configuração
Execute a célula abaixo para criar o DataFrame inicial **`df`**.

<hr>--i18n-78acde42-f2b7-4b0f-9760-65fba886ef5b
# MAGIC
### 1. Receita agregada por origem de tráfego
- Agrupe por **`traffic_source`**.
- Obtenha a soma de **`revenue`** como **`total_rev`**. Arredonde para a casa decimal (por exemplo `nnnnn.n`). 
- Obtenha a média de **`revenue`** como **`avg_rev`**.
# MAGIC
Lembre-se de importar as funções integradas necessárias.

<hr>--i18n-0ef1149d-9690-49a3-b717-ae2c38a166ed
# MAGIC
**1.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-f5c20afb-2891-4fa2-8090-cca7e313354d
# MAGIC
### 2. Obter as três principais fontes de tráfego por receita total
- Ordene por **`total_rev`** em ordem decrescente
- Limite às três primeiras linhas

<hr>--i18n-2ef23b0a-fc13-48ca-b1f3-9bc425023024
# MAGIC
**2.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-04399ae3-d2ab-4ada-9a51-9f8cc21cc45a
# MAGIC
### 3. Limitar as colunas de receita a duas casas decimais
- Modifique as colunas **`avg_rev`** e **`total_rev`** para conter números com duas casas decimais
  - Use **`withColumn()`** com os mesmos nomes para substituir essas colunas
  - Para limitar a duas casas decimais, multiplique cada coluna por 100, converta em long e divida por 100

<hr>--i18n-d28b2d3a-6db6-4ba0-8a2c-a773635a69a4
# MAGIC
**3.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-4e2d3b62-bee6-497e-b6af-44064f759451
### 4. Bônus: Reescrever usando uma função matemática integrada
Encontre uma função matemática integrada que arredonde para um número especificado de casas decimais

<hr>--i18n-6514f89e-1920-4804-96e4-a73998026023
# MAGIC
**4.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-8f19689f-4cf7-4031-bc4c-eb2ece7cb56d
# MAGIC
### 5. Encadear todos os passos acima

<hr>--i18n-53c0b070-d2bc-45e9-a3ea-da25a375d6f3
# MAGIC
**5.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-f8095ac2-c3cf-4bbb-b20b-eed1891489e0
# MAGIC
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

