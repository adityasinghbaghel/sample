# /DE 0 - Introdução ao PySpark/DE 0.04L - Laboratório de receitas de compra
<hr>--i18n-5b46ceba-8f87-4062-91cc-6f02f3303258
# Laboratório de receitas de compra
# MAGIC
Prepare um dataset de eventos com receita de compra.
# MAGIC
##### Tarefas
1. Extrair a receita de compra de cada evento
2. Filtrar eventos cuja receita não é nula
3. Verificar os tipos de eventos que geram receita
4. Descartar colunas desnecessárias
# MAGIC
##### Métodos
- DataFrame: **`select`**, **`drop`**, **`withColumn`**, **`filter`**, **`dropDuplicates`**
- Coluna: **`isNotNull`**

<hr>--i18n-412840ac-10d6-473e-a3ea-8e9e92446b80
# MAGIC
### 1. Extraia a receita de compra para cada evento
Adicione uma nova coluna **`revenue`** extraindo **`ecommerce.purchase_revenue_in_usd`**

<hr>--i18n-66dfc9f4-0a59-482e-a743-cfdbc897aee8
# MAGIC
**1.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-cb49af43-880a-4834-be9c-62f65581e67a
# MAGIC
### 2. Filtrar eventos onde a receita não é nula
Filtre os registros em que **`revenue`** não é **`null`**

<hr>--i18n-3363869f-e2f4-4ec6-9200-9919dc38582b
# MAGIC
**2.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-6dd8d228-809d-4a3b-8aba-60da65c53f1c
# MAGIC
### 3. Verifique quais tipos de eventos geram receita
Encontre valores **`event_name`** únicos em **`purchases_df`** de duas maneiras:
- Selecione "event_name" e obtenha registros distintos
- Descarte registros duplicados com base apenas no "event_name"
# MAGIC
<img src="https://files.training.databricks.com/images/icon_hint_32.png" alt="Hint"> Há apenas um evento associado a receitas

<hr>--i18n-f0d53260-4525-4942-b901-ce351f55d4c9
### 4. Eliminar coluna desnecessária
Como há apenas um tipo de evento, descarte **`event_name`** de **`purchases_df`**.

<hr>--i18n-8ea4b4df-c55e-4015-95ee-1caccafa44d6
# MAGIC
**4.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-ed143b89-079a-44e9-87f3-9c8d242f09d2
# MAGIC
### 5. Encadeie todos os passos acima, exceto o passo 3

<hr>--i18n-d7b35e13-8c38-4e17-b676-2146b64045fe
# MAGIC
**5.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-03e7e278-385e-4afe-8268-229a1984a654
# MAGIC
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

