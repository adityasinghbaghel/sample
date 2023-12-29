# /DE 2 - ETL with Spark/DE 2.2 - Providing Options for External Sources
<hr>--i18n-ac3b4f33-4b00-4169-a663-000fddc1fb9d
-- MAGIC
-- MAGIC
# Fornecendo opções para fontes externas
Embora a consulta direta de arquivos funcione bem para formatos autodescritivos, muitas fontes de dados exigem configurações adicionais ou declaração de esquema para ingerir registros adequadamente.
-- MAGIC
Nesta lição, criaremos tabelas usando fontes de dados externas. Embora essas tabelas não estejam armazenadas no formato Delta Lake (e, portanto, não sejam otimizadas para o Lakehouse), essa técnica facilita a extração de dados de sistemas externos diversos.
-- MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
- Usar o Spark SQL para configurar opções de extração de dados de fontes externas
- Criar tabelas a partir de fontes de dados externas em vários formatos de arquivo
- Descrever o comportamento default ao consultar tabelas definidas em fontes externas

<hr>--i18n-d0bd783f-524d-4953-ac3c-3f1191d42a9e
-- MAGIC
-- MAGIC
## Executar a configuração
-- MAGIC
O script de configuração criará os dados e declarará os valores necessários para a execução do restante deste notebook.

<hr>--i18n-90830ba2-82d9-413a-974e-97295b7246d0
-- MAGIC
-- MAGIC
## Quando as queries diretas não funcionam 
-- MAGIC
-- MAGIC
Os arquivos CSV são um dos formatos de arquivo mais comuns, mas uma consulta direta a esses arquivos raramente retorna os resultados desejados.

<hr>--i18n-2de59e8e-3bc3-4609-96ad-e8985b250154
-- MAGIC
-- MAGIC
Como mostrado acima, podemos ver que:
1. A linha do cabeçalho está sendo extraída como uma linha da tabela
1. Todas as colunas estão sendo carregadas como uma única coluna
1. O arquivo é delimitado por barras verticais (**`|`**)
1. A coluna final parece conter dados aninhados que estão truncados

<hr>--i18n-3eae3be1-134c-4f3b-b423-d081fb780914
-- MAGIC
-- MAGIC
## Registrando tabelas em dados externos com opções de leitura
-- MAGIC
Embora o Spark extraia eficientemente dados de algumas fontes autodescritivas usando as configurações default, muitos formatos exigirão uma declaração de esquema ou outras opções.
-- MAGIC
Há várias <a href="https://docs.databricks.com/spark/latest/spark-sql/language-manual/sql-ref-syntax-ddl-create-table-using.html" target="_blank">configurações adicionais</a> que você pode definir ao criar tabelas com base em fontes externas, mas a sintaxe abaixo demonstra os fundamentos necessários para extrair dados da maioria dos formatos.
-- MAGIC
<strong><code>
CREATE TABLE table_identifier (col_name1 col_type1, ...)<br/>
USING data_source<br/>
OPTIONS (key1 = val1, key2 = val2, ...)<br/>
LOCATION = path<br/>
</code></strong>
-- MAGIC
Observe que as opções são passadas com chaves como texto sem aspas e com valores entre aspas. O Spark dá suporte a muitas <a href="https://docs.databricks.com/data/data-sources/index.html" target="_blank">fontes de dados</a> com opções personalizadas, e sistemas adicionais podem ter suporte não oficial por meio de <a href="https://docs.databricks.com/libraries/index.html" target="_blank">bibliotecas</a> externas. 
-- MAGIC
**OBSERVAÇÃO**: Dependendo das configurações do seu workspace, você pode precisar de assistência do administrador para carregar bibliotecas e definir as configurações de segurança necessárias para algumas fontes de dados.

<hr>--i18n-1c947c25-bb8b-4cad-acca-29651e191108
-- MAGIC
A célula abaixo demonstra o uso do Spark SQL DDL para criar uma tabela em uma fonte CSV externa especificando:
1. Os nomes e tipos das colunas
1. O formato do arquivo
1. O delimitador usado para separar campos
1. A presença de um cabeçalho
1. O caminho do local de armazenamento dos dados

<hr>--i18n-4631ecfc-06b5-494a-904f-8577e345c98d
-- MAGIC
**OBSERVAÇÃO:** Para criar uma tabela a partir de uma fonte externa no PySpark, você pode encapsular o código SQL com a função **`spark.sql()`**.

<hr>--i18n-964019da-1d24-4a60-998a-bbf23ffc64a6
-- MAGIC
-- MAGIC
Observe que nenhum dado foi movido durante a declaração da tabela. 
-- MAGIC
De forma semelhante à consulta direta aos arquivos para criação de uma view, esta etapa apenas aponta para os arquivos armazenados em um local externo.
-- MAGIC
Execute a célula a seguir para confirmar se os dados estão sendo carregados corretamente.

<hr>--i18n-d11afb5e-08d3-42c3-8904-14cdddfe5431
-- MAGIC
-- MAGIC
Todos os metadados e opções passados durante a declaração da tabela serão persistidos no metastore, garantindo que os dados no local sempre serão lidos com essas opções.
-- MAGIC
**OBSERVAÇÃO**: Ao trabalhar com CSVs como fonte de dados, é importante garantir que a ordem das colunas não seja alterada se outros arquivos de dados forem adicionados ao diretório de origem. Como o formato de dados não tem uma forte imposição de esquema, o Spark carregará colunas e aplicará nomes de colunas e tipos de dados na ordem especificada durante a declaração da tabela.
-- MAGIC
Executar **`DESCRIBE EXTENDED`** em uma tabela mostrará todos os metadados associados à definição da tabela.

<hr>--i18n-fdbb45bc-72b3-4610-97a6-accd30ec8fec
-- MAGIC
-- MAGIC
## Limites de tabelas com fontes de dados externas
-- MAGIC
Se você já fez outros cursos sobre o Databricks ou leu documentos publicados por nossa empresa, já deve ter ouvido falar de Delta Lake e Lakehouse. Observe que sempre que definimos tabelas ou queries em fontes de dados externas, **não podemos** esperar as garantias de desempenho associadas ao Delta Lake e ao Lakehouse.
-- MAGIC
Por exemplo: embora as tabelas do Delta Lake garantam que você sempre consulte a versão mais recente dos dados de origem, as tabelas registradas em outras fontes de dados podem representar versões mais antigas em cache.
-- MAGIC
A célula abaixo executa uma lógica que podemos considerar como sendo a representação de um sistema externo atualizando diretamente os arquivos subjacentes à nossa tabela.

<hr>--i18n-28b2112b-4eb2-4bd4-ad76-131e010dfa44
-- MAGIC
-- MAGIC
Podemos ver que o número da contagem atual de registros na tabela não reflete as linhas recém-inseridas.

<hr>--i18n-bede6aed-2b6b-4ee7-8017-dfce2217e8b4
-- MAGIC
-- MAGIC
Na query anterior que fizemos a essa fonte de dados, o Spark armazenou automaticamente em cache os dados subjacentes no armazenamento local. Dessa forma, o Spark garante que fornecerá o desempenho ideal nas consultas subsequentes consultando apenas o cache local.
-- MAGIC
A fonte de dados externa não está configurada para informar o Spark de que ele deve atualizar esses dados. 
-- MAGIC
Nós **podemos** atualizar manualmente o cache dos dados executando o comando **`REFRESH TABLE`**.

<hr>--i18n-656a9929-a4e5-4fb1-bfe6-6c3cc7137598
-- MAGIC
-- MAGIC
Observe que atualizar a tabela invalidará o cache, o que significa que precisaremos verificar novamente a fonte de dados original e colocar todos os dados de volta na memória. 
-- MAGIC
Para datasets muito grandes, isso pode levar um tempo significativo.

<hr>--i18n-ee1ac9ff-add1-4247-bc44-2e71f0447390
-- MAGIC
-- MAGIC
## Extraindo dados de bancos de dados SQL
Os bancos de dados SQL são uma fonte de dados extremamente comum e o Databricks tem um driver JDBC padrão para conexão com vários tipos de SQL.
-- MAGIC
A sintaxe geral para criar essas conexões é:
-- MAGIC
<strong><code>
CREATE TABLE <jdbcTable><br/>
USING JDBC<br/>
OPTIONS (<br/>
&nbsp; &nbsp; url = "jdbc:{databaseServerType}://{jdbcHostname}:{jdbcPort}",<br/>
&nbsp; &nbsp; dbtable = "{jdbcDatabase}.table",<br/>
&nbsp; &nbsp; user = "{jdbcUsername}",<br/>
&nbsp; &nbsp; password = "{jdbcPassword}"<br/>
)
</code></strong>
-- MAGIC
No exemplo de código abaixo, nos conectaremos ao <a href="https://www.sqlite.org/index.html" target="_blank">SQLite</a>.
  
**OBSERVAÇÃO:** O SQLite usa um arquivo local para armazenar um banco de dados e não requer porta, nome de usuário ou senha.  
  
<img src="https://files.training.databricks.com/images/icon_warn_24.png"> **AVISO**: A configuração de back-end do servidor JDBC pressupõe que o notebook está sendo executado em um cluster de nó único. Se você estiver executando o notebook em um cluster com vários workers, o cliente em execução nos executores não conseguirá se conectar ao driver.

<hr>--i18n-33fb962c-707d-43b8-8a37-41ebb5d83b2f
-- MAGIC
-- MAGIC
Agora podemos consultar esta tabela como se ela tivesse sido definida localmente.

<hr>--i18n-3576239e-8f73-4ef9-982e-e42542d4fc70
-- MAGIC
-- MAGIC
Os metadados da tabela mostram que capturamos as informações do esquema do sistema externo.
-- MAGIC
As propriedades de armazenamento (que incluem o nome de usuário e a senha associados à conexão) são automaticamente ocultadas.

<hr>--i18n-c20051d4-f3c3-483c-b4fa-ff2d356fcd5e
-- MAGIC
-- MAGIC
Listar o conteúdo do local especificado confirma que nenhum dado está sendo persistido localmente.

<hr>--i18n-1cb11f07-755c-4fb2-a122-1eb340033712
-- MAGIC
-- MAGIC
Observe que alguns sistemas SQL, como data warehouses, têm drivers personalizados. O Spark interage com vários bancos de dados externos de maneira diferente, mas as duas abordagens básicas podem ser resumidas desta forma:
1. As tabelas de origem inteiras são movidas para o Databricks e, em seguida, a lógica é executada no cluster ativo no momento
1. A query é feita com pushdown no banco de dados SQL externo e apenas os resultados são transferidos de volta para o Databricks
-- MAGIC
Em ambos os casos, trabalhar com datasets muito grandes em bancos de dados SQL externos pode gerar uma sobrecarga significativa devido a estes fatores:
1. Latência de transferência de rede associada à movimentação de todos os dados pela Internet pública
1. Execução de lógica de query em sistemas de origem não otimizados para queries de big data

<hr>--i18n-c973af61-2b79-4c55-8e32-f6a8176ea9e8
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

