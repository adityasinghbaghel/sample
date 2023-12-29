# /DE 0 - Introdução ao PySpark/DE 0.10 - Tipos complexos
<hr>--i18n-10b1d2c4-b58e-4a1c-a4be-29c3c07c7832
# Tipos complexos
# MAGIC
Explore funções integradas para trabalhar com coleções e strings.
# MAGIC
##### Objetivos
1. Aplicar funções de coleção a matrizes de processos
1. Unir DataFrames
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/dataframe.html" target="_blank">DataFrame</a>:**`union`**, **`unionByName`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html" target="_blank">Funções integradas</a>:
  - Agregação: **`collect_set`**
  - Coleção: **`array_contains`**, **`element_at`**, **`explode`**
  - String: **`split`**

<hr>--i18n-4306b462-66db-488e-8106-66e1bbbd30d9
# MAGIC
### Funções de string
A seguir, são listadas algumas das funções integradas disponíveis para manipulação de strings.
# MAGIC
| Método | Descrição |
| --- | --- |
| translate | Traduz qualquer caractere na origem por um caractere específico em replaceString |
| regexp_replace | Substitui todas as substrings com o valor de string que corresponde à expressão regular pela string indicada |
| regexp_extract | Extrai um grupo específico correspondente a uma regex em Java da coluna de string especificada |
| ltrim | Remove os caracteres de espaço iniciais da coluna de string especificada |
| lower | Converte uma coluna de string em minúsculas |
| split | Divide strings de acordo com o padrão determinado |

<hr>--i18n-12dcf4bd-35e7-4316-b03f-ec076e9739c7
Por exemplo, se precisarmos analisar a coluna **`email`**, usaremos a função **`split`** para separar o domínio e o nome de usuário.

<hr>--i18n-4be5a98f-61e3-483b-b7a6-af4b671eb057
# MAGIC
### Funções de coleção
# MAGIC
A seguir, são listadas algumas das funções integradas disponíveis para trabalhar com matrizes.
# MAGIC
| Método | Descrição |
| --- | --- |
| array_contains | Retornará nulo se a matriz for nula, verdadeiro se a matriz tiver alguma valor e falso no caso contrário. |
| element_at | Retorna o elemento da matriz em um determinado índice. Os elementos da matriz são numerados começando por **1**. |
| explode | Cria uma nova linha para cada elemento na matriz ou coluna do mapa determinada. |
| collect_set | Retorna um conjunto de objetos exclusivos, com os elementos duplicados eliminados. |

<hr>--i18n-110c4036-291a-4ca8-a61c-835e2abb1ffc
# MAGIC
### Funções de agregação
# MAGIC
A seguir, são listadas algumas das funções de agregação integradas disponíveis para criar matrizes, normalmente a partir de GroupedData.
# MAGIC
| Método | Descrição |
| --- | --- |
| collect_list | Retorna uma matriz que consiste em todos os valores do grupo. |
| collect_set | Retorna uma matriz que consiste em todos os valores exclusivos do grupo. |

<hr>--i18n-1e0888f3-4334-4431-a486-c58f6560210f
# MAGIC
Por exemplo, para saber os tamanhos dos colchões encomendados por cada endereço de email, podemos usar a função **`collect_set`**.

<hr>--i18n-7304a528-9b97-4806-954f-56cbf7bed6dc
# MAGIC
##Union e unionByName
<img src="https://files.training.databricks.com/images/icon_warn_32.png" alt="Warning"> O método <a href="https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.union.html" target="_blank">**`union`**</a> do DataFrame resolve colunas por posição, como no SQL padrão. Você só deve usar esse método quando os dois DataFrames têm exatamente o mesmo esquema, incluindo a ordem das colunas. Já o método <a href="https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.sql.DataFrame.unionByName.html" target="_blank">**`unionByName`**</a> do DataFrame resolve colunas por nome, como o UNION ALL no SQL.  Nenhum desses dois métodos remove duplicatas.  
# MAGIC
A verificação mostrada abaixo pode ser usada para confirmar se os dois dataframes têm um esquema correspondente no qual seria apropriado usar **`union`**.

<hr>--i18n-7bb80944-614a-487f-85b8-bb3983e259ed
# MAGIC
Se for possível corresponder os dois esquemas com uma instrução **`select`** simples, poderemos usar **`union`**.

<hr>--i18n-52fdd386-f0dc-4850-87c7-4775fd2c64d4
# MAGIC
## Limpar sala de aula
# MAGIC
E por último, limpamos a sala de aula.

