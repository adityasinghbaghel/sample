# /DE 3 - Delta Lake/DE 3.1 - Schemas and Tables
<hr>--i18n-4c4121ee-13df-479f-be62-d59452a5f261
-- MAGIC
-- MAGIC
# Esquemas e tabelas no Databricks
Nesta demonstração, você criará e explorará esquemas e tabelas.
-- MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
* Usar o Spark SQL DDL para definir esquemas e tabelas
* Descrever como a palavra-chave **`LOCATION`** afeta o diretório de armazenamento default
-- MAGIC
-- MAGIC
-- MAGIC
**Recursos**
* <a href="https://docs.databricks.com/user-guide/tables.html" target="_blank">Esquemas e tabelas - Documentação do Databricks</a>
* <a href="https://docs.databricks.com/user-guide/tables.html#managed-and-unmanaged-tables" target="_blank">Tabelas gerenciadas e não gerenciadas</a>
* <a href="https://docs.databricks.com/user-guide/tables.html#create-a-table-using-the-ui" target="_blank">Criar uma tabela com a UI</a>
* <a href="https://docs.databricks.com/user-guide/tables.html#create-a-local-table" target="_blank">Criar uma tabela local</a>
* <a href="https://spark.apache.org/docs/latest/sql-data-sources-load-save-functions.html#saving-to-persistent-tables" target="_blank">Salvar em tabelas persistentes</a>

<hr>--i18n-acb0c723-a2bf-4d00-b6cb-6e9aef114985
-- MAGIC
-- MAGIC
## Configuração da lição
O script a seguir limpa as execuções anteriores desta demonstração e configura algumas variáveis do Hive que serão usadas nas consultas SQL.

<hr>--i18n-cc3d2766-764e-44bb-a04b-b03ae9530b6d
-- MAGIC
 
## Esquemas
Vamos começar criando um esquema (banco de dados).

<hr>--i18n-427db4b9-fa6c-47aa-ae70-b95087298362
-- MAGIC
-- MAGIC
 
Observe que o primeiro esquema (banco de dados) usa o local default em **`dbfs:/user/hive/warehouse/`** e que o diretório do esquema é o nome do esquema com a extensão **`.db`**

<hr>--i18n-a0fda220-4a73-419b-969f-664dd4b80024
## Tabelas gerenciadas
-- MAGIC
Vamos criar uma tabela **gerenciada** (não especificando um caminho para o local).
-- MAGIC
Criaremos a tabela no esquema (banco de dados) que criamos acima.
-- MAGIC
Observe que o esquema da tabela deve ser definido porque não há dados dos quais as colunas e os tipos de dados da tabela podem ser inferidos

<hr>--i18n-5c422056-45b4-419d-b4a6-2c3252e82575
-- MAGIC
 
Podemos conferir a descrição estendida da tabela para encontrar o local (você precisará rolar para baixo nos resultados).

<hr>--i18n-bdc6475c-1c77-46a5-9ea1-04d5a538c225
-- MAGIC
-- MAGIC
Por default, as tabelas **gerenciadas** em um esquema sem local especificado serão criadas no diretório **`dbfs:/user/hive/warehouse/<schema_name>.db/`**.
-- MAGIC
Podemos ver que, como esperado, os dados e metadados da tabela estão armazenados nesse local.

<hr>--i18n-507a84a5-f60f-4923-8f48-475ee3270dbd
-- MAGIC
 
Descarte a tabela.

<hr>--i18n-0b390bf4-3e3b-4d1a-bcb8-296fa1a7edb8
-- MAGIC
 
Observe que o diretório da tabela e seus arquivos de log e dados foram excluídos. Apenas o diretório do esquema (banco de dados) permanece.

<hr>--i18n-0e4046c8-2c3a-4bab-a14a-516cc0f41eda
-- MAGIC
 
## Tabelas externas
A seguir, criaremos uma tabela **externa** (não gerenciada) de dados de amostra. 
-- MAGIC
Os dados que usaremos estão no formato CSV. Queremos criar uma tabela Delta com um **`LOCATION`** fornecido no diretório de nossa escolha.

<hr>--i18n-6b5d7597-1fc1-4747-b5bb-07f67d806c2b
-- MAGIC
 
Observe a localização dos dados da tabela no diretório de trabalho desta lição.

<hr>--i18n-72f7bef4-570b-4c20-9261-b763b66b6942
-- MAGIC
 
Agora descartamos a tabela.

<hr>--i18n-f71374ea-db51-4a2c-8920-9f8a000850df
-- MAGIC
 
A definição da tabela não existe mais no metastore, mas os dados subjacentes permanecem intactos.

<hr>--i18n-7defc948-a8e4-4019-9633-0886d653b7c6
-- MAGIC
## Limpar
Descarte o esquema.

<hr>--i18n-bb4a8ae9-450b-479f-9e16-a76f1131bd1a
-- MAGIC
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

