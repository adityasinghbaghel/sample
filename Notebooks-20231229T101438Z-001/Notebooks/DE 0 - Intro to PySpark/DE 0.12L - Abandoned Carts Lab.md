# /DE 0 - Introdução ao PySpark/DE 0.12L - Laboratório de carrinhos abandonados
<hr>--i18n-c52349f9-afd2-4532-804b-2d50a67839fa
# Laboratório de carrinhos abandonados
Obter itens de carrinhos abandonados para acionamento por email de abandono.
1. Obter emails de clientes que efetuaram transações
2. Unir os emails aos IDs de usuário
3. Obter o histórico de itens do carrinho para cada usuário
4. Unir emails ao histórico de itens do carrinho
5. Filtrar emails com itens de carrinhos abandonados
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.join.html#pyspark.sql.DataFrame.join" target="_blank">DataFrame</a>: **`join`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html" target="_blank">Funções integradas</a>: **`collect_set`**, **`explode`**, **`lit`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrameNaFunctions.html#pyspark.sql.DataFrameNaFunctions" target="_blank">DataFrameNaFunctions</a>: **`fill`**

<hr>--i18n-f1f59329-e456-4268-836a-898d3736f378
### Configuração
Execute as células abaixo para criar DataFrames de **`sales_df`**, **`users_df`** e **`events_df`**.

<hr>--i18n-c2065783-4c56-4d12-bdf8-2e0fce22016f
# MAGIC
### 1: Receba e-mails de de clientes convertidos em transações
- Selecionar a coluna **`email`** em **`sales_df`** e remover duplicatas
- Adicionar uma nova coluna **`converted`** com o valor **`True`** para todas as linhas
# MAGIC
Salve o resultado como **`converted_users_df`**.

<hr>--i18n-4becd415-94d5-4e58-a995-d17ef1be87d0
# MAGIC
#### 1.1: Verifique seu trabalho
# MAGIC
Execute a seguinte célula para confirmar se sua solução funciona:

<hr>--i18n-72c8bd3a-58ae-4c30-8e3e-007d9608aea3
### 2: Junte e-mails com IDs de usuário
- Executar um outer join em **`converted_users_df`** e **`users_df`** com o campo **`email`**
- Filtrar por usuário onde **`email`** não é nulo
- Preencher valores nulos de **`converted`** como **`False`**
# MAGIC
Salve o resultado como **`conversions_df`**.

<hr>--i18n-48691094-e17f-405d-8f91-286a42ce55d7
# MAGIC
#### 2.1: Verifique seu trabalho
# MAGIC
Execute a seguinte célula para verificar se sua solução funciona:

<hr>--i18n-8a92bfe3-cad0-40af-a283-3241b810fc20
### 3: Obtenha o histórico de itens do carrinho para cada usuário
- Explodir o campo **`items`** em **`events_df`** com os resultados substituindo os campos **`items`** existentes
- Agrupar por **`user_id`**
  - Coletar um conjunto de todos os objetos **`items.item_id`** para cada usuário e chamar a coluna de "cart"
# MAGIC
Salve o resultado como **`carts_df`**.

<hr>--i18n-d0ed8434-7016-44c0-a865-4b55a5001194
# MAGIC
#### 3.1: Verifique seu trabalho
# MAGIC
Execute a seguinte célula para verificar se sua solução funciona:

<hr>--i18n-97b01bbb-ada0-4c4f-a253-7c578edadaf9
### 4: Junte-se ao histórico de itens do carrinho com e-mails
- Executar um left join em **`conversions_df`** e **`carts_df`** no campo **`user_id`**
# MAGIC
Salve o resultado como **`email_carts_df`**.

<hr>--i18n-0cf80000-eb4c-4f0f-a3f8-7e938b99f1ef
# MAGIC
#### 4.1: Verifique seu trabalho
# MAGIC
Execute a seguinte célula para verificar se sua solução funciona:

<hr>--i18n-380e3eda-3543-4f67-ae11-b883e5201dba
### 5: Filtrar e-mails com itens abandonados do carrinho
- Filtrar **`email_carts_df`** para usuários onde **`converted`** é falso
- Filtrar por usuários com carrinhos não nulos
# MAGIC
Salve o resultado como **`abandoned_carts_df`**.

<hr>--i18n-05ff2599-c1e6-404f-a38b-262fb0a055fa
# MAGIC
#### 5.1: Verifique seu trabalho
# MAGIC
Execute a seguinte célula para verificar se sua solução funciona:

<hr>--i18n-e2f48480-5c42-490a-9f14-9b92d29a9823
### 6: Atividade bônus
Plotar o número de itens de carrinhos abandonados por produto

<hr>--i18n-a08b8c26-6a94-4a20-a096-e74721824eac
# MAGIC
#### 6.1: Verifique seu trabalho
# MAGIC
Execute a seguinte célula para verificar se sua solução funciona:

<hr>--i18n-f2f44609-5139-465a-ad7d-d87f3f06a380
# MAGIC
### Limpar sala de aula

