# /DE 0 - Introdução ao PySpark/DE 0.01 - Spark SQL
<hr>--i18n-ad7af192-ab00-41a3-b683-5de4856cacb0
# Spark SQL
# MAGIC
Demonstrar conceitos fundamentais do Spark SQL usando a API DataFrame.
# MAGIC
##### Objetivos
1. Executar uma query SQL
1. Criar um DataFrame a partir de uma tabela
1. Escrever a mesma query usando transformações do DataFrame
1. Acionar a computação com ações do DataFrame
1. Converter entre DataFrames e SQL
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/spark_session.html" target="_blank">SparkSession</a>: **`sql`**, **`table`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html" target="_blank">DataFrame</a>:
  - Transformações:  **`select`**, **`where`**, **`orderBy`**
  - Ações: **`show`**, **`count`**, **`take`**
  - Outros métodos: **`printSchema`**, **`schema`**, **`createOrReplaceTempView`**

<hr>--i18n-3ad6c2cb-bfa4-4af5-b637-ba001a9ef54b
# MAGIC
## Múltiplas interfaces
O Spark SQL é um módulo para processamento estruturado de dados com múltiplas interfaces.
# MAGIC
Podemos interagir com o Spark SQL de duas maneiras:
1. Executando queries SQL
1. Trabalhando com a API DataFrame.

<hr>--i18n-236a9dcf-8e89-4b08-988a-67c3ca31bb71
**Método 1: Executando queries SQL**
# MAGIC
Esta é uma query SQL básica.

<hr>--i18n-58f7e711-13f5-4015-8cff-c18ec5b305c6
# MAGIC
**Método 2: Trabalhando com a API DataFrame**
# MAGIC
Também podemos expressar queries do Spark SQL usando a API DataFrame.
A célula a seguir retorna um DataFrame contendo os mesmos resultados recuperados acima.

<hr>--i18n-5b338899-be0c-46ae-92d9-8cfc3c2c3fb8
# MAGIC
Examinaremos a sintaxe da API DataFrame posteriormente nesta lição, mas você pode ver que esse padrão de design do construtor nos permite encadear uma sequência de operações muito semelhante àquelas que encontramos no SQL.

<hr>--i18n-bb02bfff-cf98-4639-af21-76bec5c8d95b
# MAGIC
## Execução de queries
Podemos expressar a mesma query usando qualquer interface. O mecanismo Spark SQL gera o mesmo plano de queries usado para otimizar e executar comandos no cluster do Spark.
# MAGIC
![mecanismo de execução de queries](https://files.training.databricks.com/images/aspwd/spark_sql_query_execution_engine.png)
# MAGIC
<img src="https://files.training.databricks.com/images/icon_note_32.png" alt="Note"> Resilient Distributed Datasets (RDDs) são a representação de baixo nível de datasets processados por um cluster do Spark. Nas primeiras versões do Spark, era necessário escrever <a href="https://spark.apache.org/docs/latest/rdd-programming-guide.html" target="_blank">código manipulando os RDDs diretamente</a>. Nas versões modernas do Spark, em vez disso, você deve usar o nível mais alto das APIs DataFrame, que o Spark compila automaticamente em operações RDD de baixo nível.

<hr>--i18n-fbaea5c1-fefc-4b3b-a645-824ffa77bbd5
# MAGIC
## Documentação da Spark API
# MAGIC
Para saber como trabalhamos com DataFrames no Spark SQL, vamos primeiro dar uma olhada na documentação da Spark API.
A página principal da <a href="https://spark.apache.org/docs/latest/" target="_blank">documentação</a> do Spark inclui links para documentos de API e guias úteis de cada versão do Spark.
# MAGIC
A <a href="https://spark.apache.org/docs/latest/api/scala/org/apache/spark/index.html" target="_blank">API do Scala</a> e a <a href="https://spark.apache.org/docs/latest/api/python/index.html" target="_blank">API do Python</a> são mais comumente usadas, e geralmente é útil consultar a documentação de ambas as linguagens.
Os documentos do Scala tendem a ser mais abrangentes, e os documentos do Python normalmente têm mais exemplos de código.
# MAGIC
#### Navegando nos documentos do módulo do Spark SQL
Encontre o módulo do Spark SQL navegando para **`org.apache.spark.sql`** na API do Scala ou **`pyspark.sql`** na API do Python.
A primeira classe que exploraremos neste módulo é a **`SparkSession`**. Você pode encontrá-la inserindo “SparkSession” na barra de pesquisa.

<hr>--i18n-24790eda-96df-49bb-af34-b1ed839fa80a
## SparkSession
A classe **`SparkSession`** é o ponto de entrada único para todas as funcionalidades do Spark usando a API DataFrame.
# MAGIC
Nos notebooks do Databricks, a SparkSession é criada para você e armazenada em uma variável chamada **`spark`**.

<hr>--i18n-4f5934fb-12b9-4bf2-b821-5ab17d627309
# MAGIC
O exemplo do início desta lição usou o método **`table`** do SparkSession para criar um DataFrame a partir da tabela **`products`**. Vamos salvar isso na variável **`products_df`**.

<hr>--i18n-f9968eff-ed08-4ed6-9fe7-0252b94bf50a
Vários métodos adicionais que podemos usar para criar DataFrames são listados abaixo. Todo este conteúdo pode ser encontrado na <a href="https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.SparkSession.html" target="_blank">documentação</a> da **`SparkSession`**.
# MAGIC
#### **`SparkSession`** Métodos
| Método | Descrição |
| --- | --- |
| sql | Retorna um DataFrame representando o resultado de uma query determinada |
| table | Retorna a tabela especificada como um DataFrame |
| read | Retorna um DataFrameReader que pode ser usado para ler dados como um DataFrame |
| range | Cria um DataFrame com uma coluna contendo os elementos de um intervalo do início ao fim (exclusivo) com valor do passo e número de partições |
| createDataFrame | Cria um DataFrame, usado principalmente para testes, a partir de uma lista de tuplas |

<hr>--i18n-2277e250-91f9-489a-940b-97d17e75c7f5
# MAGIC
Vamos usar o método da SparkSession para executar SQL.

<hr>--i18n-f2851702-3573-4cb4-9433-ec31d4ceb0f2
# MAGIC
## DataFrames
Lembre-se de que expressar uma query usando os métodos da API DataFrame retorna resultados em um DataFrame. Vamos armazenar isso na variável **`budget_df`**.
# MAGIC
Um **DataFrame** é uma coleção distribuída de dados agrupados em colunas nomeadas.

<hr>--i18n-d538680a-1d7a-433c-9715-7fd975d4427b
# MAGIC
Podemos usar **`display()`** para gerar os resultados de um dataframe.

<hr>--i18n-ea532d26-a607-4860-959a-00a2eca34305
# MAGIC
O **esquema** define os nomes das colunas e os tipos de um dataframe.
# MAGIC
Acesse o esquema de um dataframe usando o atributo **`schema`**.

<hr>--i18n-4212166a-a200-44b5-985c-f7f1b33709a3
# MAGIC
Veja uma saída melhor para este esquema usando o método **`printSchema()`**.

<hr>--i18n-7ad577db-093a-40fb-802e-99bbc5a4435b
# MAGIC
## Transformações
Quando criamos **`budget_df`**, usamos uma série de métodos de transformação do DataFrame, por exemplo, **`select`**, **`where`** e **`orderBy`**.
# MAGIC
<strong><code>products_df  
&nbsp;  .select("name", "price")  
&nbsp;  .where("price < 200")  
&nbsp;  .orderBy("price")  
</code></strong>
    
As transformações operam e retornam DataFrames, permitindo encadear métodos de transformação para construir novos DataFrames.
No entanto, essas operações não podem ser executadas sozinhas, pois os métodos de transformação não são processados por **avaliação preguiçosa**.
# MAGIC
A execução da célula a seguir não aciona nenhum cálculo.

<hr>--i18n-56f40b55-842f-44cf-b34a-b0fd17a962d4
# MAGIC
## Ações
Por outro lado, as ações do DataFrame são métodos que **acionam a computação**.
São necessárias ações para acionar a execução de quaisquer transformações do DataFrame.
# MAGIC
A ação **`show`** faz a célula a seguir executar transformações.

<hr>--i18n-6f574091-0026-4dd2-9763-c6d4c3b9c4fe
# MAGIC
Vários exemplos de ações do <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html#dataframe-apis" target="_blank">DataFrame</a> são listados abaixo.
# MAGIC
### Métodos de ação do DataFrame
| Método | Descrição |
| --- | --- |
| show | Exibe as n primeiras linhas do DataFrame em formato tabular |
| count | Retorna o número de linhas no DataFrame |
| describe, summary | Calcula estatísticas básicas para colunas numéricas e de string |
| first, head | Retorna a primeira linha |
| collect | Retorna uma matriz que contém todas as linhas deste DataFrame |
| take | Retorna uma matriz das primeiras n linhas no DataFrame |

<hr>--i18n-7e725f41-43bc-4e56-9c44-d46becd375a0
**`count`** retorna o número de registros em um DataFrame.

<hr>--i18n-12ea69d5-587e-4953-80d9-81955eeb9d7b
**`collect`** retorna uma matriz de todas as linhas em um DataFrame.

<hr>--i18n-983f5da8-b456-42b5-b21c-b6f585c697b4
# MAGIC
## Converter entre DataFrames e SQL

<hr>--i18n-0b6ceb09-86dc-4cdd-9721-496d01e8737f
**`createOrReplaceTempView`** cria uma view temporária baseada no DataFrame. O tempo de vida da view temporária está vinculado à SparkSession usada para criar o DataFrame.

<hr>--i18n-81ff52ca-2160-4bfd-a78f-b3ba2f8b4933
# MAGIC
Execute a célula a seguir para excluir as tabelas e os arquivos associados a esta lição.

