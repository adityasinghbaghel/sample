# /DE 0 - Introdução ao PySpark/DE 0.13 - Otimização de queries
<hr>--i18n-15802400-50d0-40e5-854c-89b08b50c14e
# MAGIC
# Otimização de queries
# MAGIC
Vamos explorar planos e otimizações de queries em vários exemplos, incluindo otimizações lógicas e exemplos com e sem pushdown de predicado.
# MAGIC
##### Objetivos
1. Otimizações lógicas
1. Pushdown de predicado
1. Sem pushdown de predicado
# MAGIC
##### Métodos 
- <a href="https://spark.apache.org/docs/3.1.3/api/python/reference/api/pyspark.sql.DataFrame.explain.html#pyspark.sql.DataFrame.explain" target="_blank">DataFrame</a>: **`explain`**

<hr>--i18n-8cb4efc1-cf1b-42a5-9cf3-109ccc0b5bb5
# MAGIC
Vamos executar a célula de configuração e armazenar o DataFrame inicial na variável **`df`**. Esse DataFrame mostra dados de eventos.

<hr>--i18n-63293e50-d68e-468d-a3c2-08608c66fb1d
# MAGIC
### Otimização lógica
# MAGIC
**`explain(..)`** imprime os planos de query, com a opção de formatação por um modo de explicação específico. Compare o plano lógico e o plano físico a seguir, observando como o Catalyst lidou com as múltiplas transformações de **`filter`**.

<hr>--i18n-cc9b8d61-bb89-4961-819d-d135ec4f4aac
É claro que poderíamos ter escrito a query nós mesmos usando uma única condição de **`filter`**. Compare o plano de query anterior ao plano a seguir.

<hr>--i18n-27a81fc2-4aec-46bf-89c0-bb8b90fa9e17
É óbvio que não escreveríamos o código a seguir intencionalmente, mas talvez você não note as condições de filtro duplicadas em uma query longa e complexa. Vamos ver como o Catalyst lida com esta query.

<hr>--i18n-90d320e9-9295-4869-8042-217652fe355b
### Armazenamento em cache
# MAGIC
Por default, os dados de um DataFrame permanecem em um cluster do Spark apenas enquanto estão são processados durante uma query e não persistem automaticamente no cluster após o processamento. (O Spark é um mecanismo de processamento de dados, não um sistema de armazenamento de dados.) Você pode solicitar explicitamente ao Spark para persistir um DataFrame no cluster invocando o método **`cache`**.
# MAGIC
Se você armazenar um DataFrame em cache, deverá sempre removê-lo explicitamente do cache invocando **`unpersist`** quando não precisar mais dele.
# MAGIC
<img src="https://files.training.databricks.com/images/icon_best_32.png" alt="Best Practice"> Armazenar um DataFrame em cache pode ser apropriado quando você tem certeza de que usará o mesmo DataFrame várias vezes, como em:
# MAGIC
- Análises de dados exploratórias
- Treinamentos de modelo do machine learning
# MAGIC
<img src="https://files.training.databricks.com/images/icon_warn_32.png" alt="Warning"> Além desses casos de uso, você **não** deve armazenar DataFrames em cache, pois isso provavelmente *reduzirá* o desempenho da aplicação.
# MAGIC
- O armazenamento em cache consome recursos do cluster que poderiam ser usados para a execução de tarefas
- O armazenamento em cache pode impedir que o Spark execute otimizações de query, conforme mostrado no próximo exemplo

<hr>--i18n-2256e20c-d69c-4ce8-ae74-c513a8d673f5
# MAGIC
### Pushdown de predicado
# MAGIC
O exemplo a seguir mostra a leitura de uma fonte JDBC, em que o Catalyst determina que o *pushdown de predicado* pode acontecer.

<hr>--i18n-b067b782-e86b-4284-80f4-4faedfb0953e
# MAGIC
Observe a ausência de um elemento **Filter** e a presença do elemento **PushedFilters** em **Scan**. A operação de filtro é enviada ao banco de dados e apenas os registros correspondentes são enviados ao Spark. Isso pode reduzir significativamente a quantidade de dados que o Spark precisa ingerir.

<hr>--i18n-e378204a-cce7-4903-a1e4-f2f3e387c4f5
# MAGIC
### Sem pushdown de predicado
# MAGIC
Em comparação, armazenar os dados em cache antes da filtragem elimina a possibilidade de pushdown do predicado.

<hr>--i18n-7923a69e-43bd-4a4d-8de9-ac83d6eee749
# MAGIC
Em adição ao elemento **Scan** (leitura do JDBC) que vimos no exemplo anterior, neste exemplo vemos também **InMemoryTableScan** seguido por um **Filter** no plano de explicação.
# MAGIC
Isso significa que o Spark precisou ler TODOS os dados do banco de dados, armazená-los em cache e, em seguida, fazer a varredura no cache para encontrar os registros que correspondem à condição do filtro.

<hr>--i18n-20c1b03f-3627-40bf-b426-f24cb3111430
Lembre-se de limpar tudo quando terminar!

<hr>--i18n-be8bb4b0-cdcc-4457-baa3-145a71d04b35
# MAGIC
### Limpar sala de aula

