# /DE 0 - Introdução ao PySpark/DE 0.03 - DataFrame e coluna
<hr>--i18n-ab2602b5-4183-4f33-8063-cfc03fcb1425
# DataFrame e coluna
##### Objetivos
1. Construir colunas
1. Subdividir colunas
1. Adicionar ou substituir colunas
1. Subdividir linhas
1. Classificar linhas
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html" target="_blank">DataFrame</a>: **`select`**, **`selectExpr`**, **`drop`**, **`withColumn`**, **`withColumnRenamed`**, **`filter`**, **`distinct`**, **`limit`**, **`sort`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/column.html" target="_blank">Coluna</a>: **`alias`**, **`isin`**, **`cast`**, **`isNotNull`**, **`desc`**, operadores

<hr>--i18n-ef990348-e991-4edb-bf45-84de46a34759
# MAGIC
Vamos usar o dataset de eventos BedBricks.

<hr>--i18n-4ea9a278-1eb6-45ad-9f96-34e0fd0da553
# MAGIC
## Expressões da coluna
# MAGIC
Uma <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/column.html" target="_blank">coluna</a> é uma construção lógica que será calculada com base nos dados de um DataFrame usando uma expressão
# MAGIC
Construir uma nova coluna com base nas colunas existentes em um DataFrame

<hr>--i18n-d87b8303-8f78-416e-99b0-b037caf2107a
O Scala oferece suporte a uma sintaxe adicional para criar uma coluna a partir de colunas existentes no DataFrame

<hr>--i18n-64238a77-0877-4bd4-af46-a9a8bd4763c6
# MAGIC
### Operadores e métodos de coluna
| Método | Descrição |
| --- | --- |
| \*, + , <, >= | Operadores matemáticos e de comparação |
| ==, != | Testes de igualdade e desigualdade (os operadores Scala são **`===`** e **`=!=`**) |
| alias | Dá um alias à coluna |
| cast, astype | Converte a coluna em um tipo de dado diferente |
| isNull, isNotNull, isNan | Is null, is not null, is NaN |
| asc, desc | Retorna uma expressão de classificação baseada na ordem crescente/decrescente da coluna |

<hr>--i18n-6d68007e-3dbf-4f18-bde4-6990299ef086
# MAGIC
Crie expressões complexas com colunas, operadores e métodos existentes.

<hr>--i18n-7c1c0688-8f9f-4247-b8b8-bb869414b276
Este é um exemplo de uso dessas expressões de coluna no contexto de um DataFrame

<hr>--i18n-7ba60230-ecd3-49dd-a4c8-d964addc6692
# MAGIC
## Métodos de transformação do DataFrame
| Método | Descrição |
| --- | --- |
| **`select`** | Retorna um novo DataFrame calculando a expressão dada para cada elemento |
| **`drop`** | Retorna um novo DataFrame com uma coluna descartada |
| **`withColumnRenamed`** | Retorna um novo DataFrame com uma coluna renomeada |
| **`withColumn`** | Retorna um novo DataFrame adicionando uma coluna ou substituindo uma coluna existente que tenha o mesmo nome |
| **`filter`**, **`where`** | Filtra linhas usando a condição determinada |
| **`sort`**, **`orderBy`** | Retorna um novo DataFrame classificado pelas expressões determinadas |
| **`dropDuplicates`**, **`distinct`** | Retorna um novo DataFrame com linhas duplicadas removidas |
| **`limit`** | Retorna um novo DataFrame utilizando as primeiras n linhas |
| **`groupBy`** | Agrupa o DataFrame usando as colunas especificadas para permitir sua agregação |

<hr>--i18n-3e95eb92-30e4-44aa-8ee0-46de94c2855e
# MAGIC
### Subdividir colunas
Use transformações do DataFrame para subdividir colunas

<hr>--i18n-987cfd99-8e06-447f-b1c7-5f104cd5ed2f
# MAGIC
#### **`select()`**
Seleciona uma lista de colunas ou expressões baseadas em colunas

<hr>--i18n-8d556f84-bfcd-436a-a3dd-893143ce620e
# MAGIC
#### **`selectExpr()`**
Seleciona uma lista de expressões SQL

<hr>--i18n-452f7fb3-3866-4835-827f-6d359f364046
# MAGIC
#### **`drop()`**
Retorna um novo DataFrame após descartar uma coluna especificada identificada como um objeto string ou coluna
# MAGIC
Use strings para especificar múltiplas colunas

<hr>--i18n-b11609a3-11d5-453b-b713-15131b277066
# MAGIC
### Adicionar ou substituir colunas
Use transformações do DataFrame para adicionar ou substituir colunas

<hr>--i18n-f29a47d9-9567-40e5-910b-73c640cc61ca
# MAGIC
#### **`withColumn()`**
Retorna um novo DataFrame adicionando uma coluna ou substituindo uma coluna existente que tenha o mesmo nome.

<hr>--i18n-969c0d9f-202f-405a-8c66-ef29076b48fc
# MAGIC
#### **`withColumnRenamed()`**
Retorna um novo DataFrame com uma coluna renomeada.

<hr>--i18n-23b0a9ef-58d5-4973-a610-93068a998d5e
# MAGIC
### Subdividir linhas
Use transformações do DataFrame para subdividir linhas

<hr>--i18n-4ada6444-7345-41f7-aaa2-1de2d729483f
# MAGIC
#### **`filter()`**
Filtra linhas usando a expressão SQL determinada ou uma condição baseada em coluna.
# MAGIC
##### Alias: **`where`**

<hr>--i18n-4d6a79eb-3989-43e1-8c28-5b976a513f5f
# MAGIC
#### **`dropDuplicates()`**
Retorna um novo DataFrame com linhas duplicadas removidas, considerando opcionalmente apenas um subconjunto de colunas.
# MAGIC
##### Alias: **`distinct`**

<hr>--i18n-433c57f4-ce40-48c9-8d04-d3a13c398082
# MAGIC
#### **`limit()`**
Retorna um novo DataFrame utilizando as primeiras n linhas.

<hr>--i18n-d4117305-e742-497e-964d-27a7b0c395cd
# MAGIC
### Classificar linhas
Use transformações do DataFrame para classificar linhas

<hr>--i18n-16b3c7fe-b5f2-4564-9e8e-4f677777c50c
# MAGIC
#### **`sort()`**
Retorna um novo DataFrame classificado pelas colunas ou expressões determinadas.
# MAGIC
##### Alias: **`orderBy`**

<hr>--i18n-555c663e-3f62-4478-9d76-c9ee090beca1
# MAGIC
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

