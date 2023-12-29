# /DE 2 - ETL with Spark/DE 2.7B - Python UDFs
<hr>--i18n-7c0e5ecf-c2e4-4a89-a418-76faa15ce226
# MAGIC
# Funções Python definidas pelo usuário
# MAGIC
##### Objetivos
1. Definir uma função
1. Criar e aplicar uma UDF
1. Criar e registrar uma UDF com sintaxe de decoradores do Python
1. Criar e aplicar uma UDF Pandas (vetorizada)
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/3.1.3/api/python/reference/api/pyspark.sql.functions.udf.html" target="_blank">Decorador de UDF Python</a>: **`@udf`**
- <a href="https://spark.apache.org/docs/3.1.3/api/python/reference/api/pyspark.sql.functions.pandas_udf.html" target="_blank">Decorador de UDF Pandas</a>: **`@pandas_udf`**

<hr>--i18n-1e94c419-dd84-4f8d-917a-019b15fc6700
# MAGIC
### Função definida pelo usuário (UDF)
Uma função personalizada de transformação de coluna tem estas características:
# MAGIC
- A função não pode ser otimizada pelo otimizador Catalyst
- A função é serializada e enviada aos executores
- Os dados de linha são desserializados do formato binário nativo do Spark e passados para a UDF; os resultados são serializados de volta no formato nativo do Spark
- Em UDFs Python, há uma sobrecarga adicional de comunicação interprocessual entre o executor e um interpretador Python em execução em cada nó de worker

<hr>--i18n-4d1eb639-23fb-42fa-9b62-c407a0ccde2d
# MAGIC
Nesta demonstração, usaremos os dados de vendas.

<hr>--i18n-05043672-b02a-4194-ba44-75d544f6af07
# MAGIC
### Definir uma função
# MAGIC
Defina uma função (no driver) para obter a primeira letra de uma string do campo **`email`**.

<hr>--i18n-17f25aa9-c20f-41da-bac5-95ebb413dcd4
# MAGIC
### Criar e aplicar uma UDF
Registre a função como uma UDF. Essa ação serializa a função e a envia aos executores para permitir a transformação dos registros do DataFrame.

<hr>--i18n-75abb6ee-291b-412f-919d-be646cf1a580
# MAGIC
Aplique a UDF na coluna **`email`**.

<hr>--i18n-26f93012-a994-4b6a-985e-01720dbecc25
# MAGIC
### Usar a sintaxe do decorador (somente Python)
# MAGIC
Alternativamente, você pode definir e registrar uma UDF usando a <a href="https://realpython.com/primer-on-python-decorators/" target="_blank">sintaxe do decorador do Python</a>. O parâmetro do decorador **`@udf`** é o tipo de dado de coluna que a função retorna.
# MAGIC
Você não poderá mais chamar a função local Python (ou seja, **`first_letter_udf("annagray@kaufman.com")`** não funcionará).
# MAGIC
<img src="https://files.training.databricks.com/images/icon_note_32.png" alt="Note"> Este exemplo também usa <a href="https://docs.python.org/3/library/typing.html" target="_blank">dicas de tipo de Python</a>, que foram introduzidas no Python 3.5. As dicas de tipo não são necessárias para este exemplo, mas servem como um guia para ajudar os desenvolvedores a usar a função corretamente. Elas são usadas neste exemplo para enfatizar que a UDF processa um registro de cada vez, utilizando um único argumento **`str`** e retornando um valor **`str`**.

<hr>--i18n-4d628fe1-2d94-4d86-888d-7b9df4107dba
# MAGIC
O decorador de UDF é usado neste passo.

<hr>--i18n-3ae354c0-0b10-4e8c-8cf6-da68e8fba9f2
# MAGIC
### UDFs vetorizadas/Pandas
# MAGIC
A disponibilidade em Python aumenta a eficiência das UDFs Pandas. As UDFs Pandas utilizam o Apache Arrow para acelerar a computação.
# MAGIC
* <a href="https://databricks.com/blog/2017/10/30/introducing-vectorized-udfs-for-pyspark.html" target="_blank">Postagem no blog</a>
* <a href="https://spark.apache.org/docs/latest/api/python/user_guide/sql/arrow_pandas.html?highlight=arrow" target="_blank">Documentação</a>
# MAGIC
<img src="https://databricks.com/wp-content/uploads/2017/10/image1-4.png" alt="Benchmark" width ="500" height="1500">
# MAGIC
As funções definidas pelo usuário são executadas usando: 
* O <a href="https://arrow.apache.org/" target="_blank">Apache Arrow</a>, um formato de dados colunares na memória usado no Spark para transferir dados com eficiência entre processos JVM e Python com custo quase zero de (des)serialização
* Pandas dentro da função para trabalhar com instâncias e APIs do Pandas
# MAGIC
<img src="https://files.training.databricks.com/images/icon_warn_32.png" alt="Warning"> A partir do Spark 3.0, você deve **sempre** definir a UDF Pandas usando dicas de tipo de Python.

<hr>--i18n-9a2fb1b1-8060-4e50-a759-f30dc73ce1a1
# MAGIC
Podemos registrar essas UDFs Pandas no namespace SQL.

<hr>--i18n-5e506b8d-a488-4373-af9a-9ebb14834b1b
# MAGIC
### Limpar sala de aula

