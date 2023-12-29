# /DE 4 - Delta Live Tables/DE 4.2 - Python vs SQL
<hr>--i18n-6ee8963d-1bec-43df-9729-b4da38e8ad0d
# Delta Live Tables: Python versus SQL
# MAGIC
Nesta lição, revisaremos as principais diferenças entre as implementações Python e SQL do Delta Live Tables
# MAGIC
Ao final desta lição você será capaz de: 
# MAGIC
* Identificar as principais diferenças entre as implementações Python e SQL do Delta Live Tables

<hr>--i18n-48342971-ca6a-4774-88e1-951b8189bec4
# MAGIC
# Python x SQL
| Python | SQL | Notas |
|--------|--------|--------|
| API do Python | API de SQL proprietária |  |
| Sem verificação de sintaxe | Tem verificações de sintaxe| No Python, uma célula de notebook do DLT executada isoladamente gera um erro, enquanto que o SQL faz uma verificação e informa se o comando é sintaticamente válido. Em ambos os casos, células individuais do notebook não devem ser executadas em pipelines do DLT. |
| Observações sobre importações | Nenhuma | O módulo dlt deve ser importado explicitamente para suas bibliotecas de notebook do Python. Isso não é necessário no SQL. |
| Tabelas como DataFrames | Tabelas como resultados de query | A API DataFrame do Python permite múltiplas transformações de um dataset agrupando várias chamadas de API. Comparando ao SQL, é necessário salvar essas transformações em tabelas temporárias à medida que são processadas. |
|@dlt.table()  | Instrução SELECT | No SQL, a lógica central da query, com as transformações a fazer nos dados, está contida na instrução SELECT. No Python, as transformações de dados são especificadas quando você configura as opções de @dlt.table().  |
| @dlt.table(comment = "Python comment",table_properties = {"quality": "silver"}) | COMMENT "SQL comment"       TBLPROPERTIES ("quality" = "silver") | É assim que você adiciona comentários e propriedades de tabela no Python, em comparação com o SQL |
