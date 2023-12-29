# /DE 0 - Introdução ao PySpark/DE 0.08L - Laboratório de ingestão de dados
<hr>--i18n-b24dbbe7-205a-4a6a-8b35-ef14ee51f01c
# Laboratório de ingestão de dados
# MAGIC
Leia arquivos CSV contendo dados de produtos.
# MAGIC
##### Tarefas
1. Ler com um esquema de inferência
2. Ler com um esquema definido pelo usuário
3. Ler com um esquema como string formatada em DDL
4. Gravar usando o formato Delta

<hr>--i18n-13d2d883-def7-49df-b634-910428adc5a2
# MAGIC
### 1. Ler com esquema de inferência
- Visualize o primeiro arquivo CSV usando o método **`fs.head`** de DBUtils com o caminho do arquivo fornecido na variável **`single_product_csv_file_path`**
- Crie **`products_df`** lendo arquivos CSV localizados no caminho de arquivo fornecido na variável **`products_csv_path`**
  - Configure opções para usar a primeira linha como cabeçalho e inferir o esquema

<hr>--i18n-3ca1ef04-e7a4-4e9b-a49e-adba3180d9a4
# MAGIC
**1.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-d7f8541b-1691-4565-af41-6c8f36454e95
# MAGIC
### 2. Ler com o usuário- esquema definido
Defina o esquema criando um **`StructType`** com nomes de colunas e tipos de dados

<hr>--i18n-af9a4134-5b88-4c6b-a3bb-8a1ae7b00e53
# MAGIC
**2.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-0a52c971-8cff-4d89-b9fb-375e1c48364d
# MAGIC
### 3. Ler com uma string formatada em DDL

<hr>--i18n-733b1e61-b319-4da6-9626-3f806435eec5
# MAGIC
**3.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-0b7e2b59-3fb1-4179-9793-bddef0159c89
# MAGIC
### 4. Gravar em Delta
Grave **`products_df`** no caminho de arquivo fornecido na variável **`products_output_path`**

<hr>--i18n-fb47127c-018d-4da0-8d6d-8584771ccd64
# MAGIC
**4.1: VERIFIQUE SEU TRABALHO**

<hr>--i18n-42fb4bd4-287f-4863-b6ea-f635f315d8ec
# MAGIC
### Limpar sala de aula

