# /DE 0 - Introdução ao PySpark/DE 0.07 - Leitor e gravador
<hr>--i18n-ef4d95c5-f516-40e2-975d-71fc17485bba
# MAGIC
# Leitor e gravador
##### Objetivos
1. Ler arquivos CSV
1. Ler arquivos JSON
1. Gravar DataFrames em arquivos
1. Gravar DataFrames em tabelas
1. Gravar DataFrames em uma tabela Delta
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html#input-and-output" target="_blank">DataFrameReader</a>: **`csv`**, **`json`**, **`option`**, **`schema`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html#input-and-output" target="_blank">DataFrameWriter</a>: **`mode`**, **`option`**, **`parquet`**, **`format`**, **`saveAsTable`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.types.StructType.html#pyspark.sql.types.StructType" target="_blank">StructType</a>: **`toDDL`**
# MAGIC
##### Tipos de Spark
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html#data-types" target="_blank">Tipos</a>: **`ArrayType`**, **`DoubleType`**, **`IntegerType`**, **`LongType`**, **`StringType`**, **`StructType`**, **`StructField`**

<hr>--i18n-24a8edc0-6f58-4530-a256-656e2b577e3e
# MAGIC
## DataFrameReader
Interface usada para carregar um DataFrame de sistemas de armazenamento externos
# MAGIC
**`spark.read.parquet("path/to/files")`**
# MAGIC
O DataFrameReader pode ser acessado usando o atributo **`read`** da SparkSession. Esta aula inclui métodos para carregar DataFrames de diferentes sistemas de armazenamento externo.

<hr>--i18n-108685bb-e26b-47db-a974-7e8de357085f
# MAGIC
### Ler arquivos CSV
Leia o CSV usando o método **`csv`** do DataFrameReader e as seguintes opções:
# MAGIC
Use o separador por tabulação, a primeira linha como cabeçalho e deduza o esquema

<hr>--i18n-86642c4a-e773-4856-b03a-13b359fa499f
A API Python do Spark também permite especificar as opções do DataFrameReader como parâmetros do método **`csv`**

<hr>--i18n-8827b582-0b26-407c-ba78-cb64666d7a6b
# MAGIC
Defina manualmente o esquema criando um **`StructType`** com nomes de colunas e tipos de dados

<hr>--i18n-d2e7e50d-afa1-4e65-826f-7eefc0a70640
# MAGIC
Leia arquivos CSV usando esse esquema definido pelo usuário em vez de inferir o esquema

<hr>--i18n-0e098586-6d6c-41a6-9196-640766212724
# MAGIC
Ou defina o esquema usando a sintaxe da <a href="https://en.wikipedia.org/wiki/Data_definition_language" target="_blank">linguagem de definição de dados (DDL)</a>.

<hr>--i18n-bbc2fc78-c2d4-42c5-91c0-652154ce9f89
# MAGIC
### Ler de arquivos JSON
# MAGIC
Leia arquivos JSON com o método **`json`** do DataFrameReader e a opção de inferir esquema

<hr>--i18n-509e0bc1-1ffd-4c22-8188-c3317215d5e0
# MAGIC
Leis os dados mais rapidamente criando um **`StructType`** com nomes dos esquemas e tipos de dados

<hr>--i18n-ae248126-23f3-49e2-ab43-63700049405c
# MAGIC
É possível usar o método **`StructType`** do Scala **`toDDL`** e ter uma string formatada em DDL criada para você.
# MAGIC
Isso é conveniente quando você precisa obter a string formatada em DDL para ingestão de arquivos CSV e JSON, mas não quer criá-la manualmente nem usar a variante **`StructType`** do esquema.
# MAGIC
Essa funcionalidade não está disponível no Python, mas os recursos poderosos dos notebooks nos permitem usar ambas as linguagens.

<hr>--i18n-5a35a507-1eff-4f5f-b6b9-6a254b61b38f
Em um notebook Python como este, crie uma célula Scala para injetar os dados e produzir o esquema formatado em DDL

<hr>--i18n-1a79ce6b-d803-4d25-a1c2-c24a70a0d6bf
Este é um ótimo truque para produzir um esquema para um dataset completamente novo e acelerar o desenvolvimento.
# MAGIC
Quando terminar (por exemplo, no passo 7), lembre-se de excluir o código temporário.
# MAGIC
<img src="https://files.training.databricks.com/images/icon_warn_32.png"> AVISO: **Não use este truque no ambiente de produção**</br>
A inferência de um esquema pode ser MUITO lenta, pois<br/>
exige a leitura completa do dataset de origem

<hr>--i18n-f57b5940-857f-4e37-a2e4-030b27b3795a
# MAGIC
## DataFrameWriter
Interface usada para gravar um DataFrame em sistemas de armazenamento externos
# MAGIC
<strong><code>
(df  
&nbsp;  .write                         
&nbsp;  .option("compression", "snappy")  
&nbsp;  .mode("overwrite")      
&nbsp;  .parquet(output_dir)       
)
</code></strong>
# MAGIC
O DataFrameWriter pode ser acessado usando o atributo **`write`** da SparkSession. Esta classe inclui métodos para gravar DataFrames em diferentes sistemas de armazenamento externo.

<hr>--i18n-8799bf1d-1d80-4412-b093-ad3ba71d73b8
# MAGIC
### Gravar DataFrames em arquivos
# MAGIC
Grave **`users_df`** em Parquet com o método **`parquet`** do DataFrameWriter e as seguintes configurações:
# MAGIC
Compressão rápida, modo de substituição

<hr>--i18n-a2f733f5-8afc-48f0-aebb-52df3a6e461f
Assim como acontece com o DataFrameReader, a API Python do Spark também permite especificar as opções do DataFrameWriter como parâmetros para o método **`parquet`**

<hr>--i18n-61a5a982-f46e-4cf6-bce2-dd8dd68d9ed5
# MAGIC
### Gravar DataFrames em tabelas
# MAGIC
Grave **`events_df`** em uma tabela usando o método **`saveAsTable`** do DataFrameWriter
# MAGIC
<img src="https://files.training.databricks.com/images/icon_note_32.png" alt="Note"> Isso cria uma tabela global, ao contrário da visão local criada pelo método **`createOrReplaceTempView`** do DataFrame

<hr>--i18n-abcfcd19-ba89-4d97-a4dd-2fa3a380a953
# MAGIC
Esta tabela foi salva no banco de dados criado para você na configuração da sala de aula. O nome do banco de dados é indicado abaixo.

<hr>--i18n-9a929c69-4b77-4c4f-b7b6-2fca645037aa
## Delta Lake
# MAGIC
Em quase todos os casos, a prática recomendada é utilizar o formato Delta Lake, principalmente quando a referência aos dados for feita em um workspace do Databricks. 
# MAGIC
A <a href="https://delta.io/" target="_blank">Delta Lake</a> é uma tecnologia de código aberto projetada para funcionar com o Spark a fim de tornar os data lakes confiáveis.
# MAGIC
![delta](https://files.training.databricks.com/images/aspwd/delta_storage_layer.png)
# MAGIC
#### Principais recursos da tecnologia Delta Lake
- Transações ACID
- Processamento escalável de metadados
- Processamento em streaming ou lotes (batch) unificados
- Viagem do tempo (controle de versão de dados)
- Imposição e evolução de esquema
- Histórico de auditoria
- Formato Parquet
- Compatível com as APIs do Apache Spark

<hr>--i18n-ba1e0aa1-bd35-4594-9eb7-a16b65affec1
### Gravar resultados em uma tabela Delta
# MAGIC
Grave **`events_df`** com o método **`save`** do DataFrameWriter e as seguintes configurações: formato Delta e modo de substituição.

<hr>--i18n-331a0d38-4573-4987-9aa6-ebfc9476f85d
# MAGIC
### Limpar a sala de aula

