# /DE 6 - Managing Permissions/DE 6.99 - OPTIONAL Administration/DE 6.99.3 - OPTIONAL Upgrade a Table to Unity Catalog
<hr>--i18n-5d2baeeb-370f-413f-8eb2-5138b4bf046c
# Atualizar uma tabela para o Unity Catalog
-- MAGIC
Neste notebook você aprenderá como:
* Migrar uma tabela do Hive metastore herdado existente para o Unity Catalog
* Criar concessões apropriadas para permitir que outras pessoas acessem a tabela
* Executar transformações simples em uma tabela durante a migração para o Unity Catalog

<hr>--i18n-4111c8be-0a37-43fe-9003-be8c9751760b
## Configurar
-- MAGIC
Execute as células a seguir para realizar alguma configuração. Para evitar conflitos em um ambiente de treinamento compartilhado, isso criará um banco de dados com nome exclusivo e exclusivo para seu uso. Isso também criará uma tabela de origem de exemplo chamada **movies** dentro do Hive metastore herdado. 

<hr>--i18n-96a90022-a7bb-40db-8429-30c31cbbf215
Dentro do Hive metastore legado local deste workspace, agora temos uma tabela chamada **movies**, que reside em um banco de dados específico do usuário descrito na saída da célula acima. Para facilitar o trabalho, o nome do banco de dados é armazenado em uma variável Hive chamada *DA.my_schema_name*. Vamos visualizar os dados armazenados na tabela usando essa variável.
-- MAGIC
Usamos esse nome de banco de dados exclusivo no hive metastore para evitar interferência potencial com outras pessoas no ambiente de treinamento compartilhado.

<hr>--i18n-7b2f4105-35f5-4a00-8a19-b4db0c11e045
## Configurar destino
-- MAGIC
Com uma tabela de origem implementada, vamos configurar um destino no Unity Catalog para o qual migrar a tabela.

<hr>--i18n-24602317-c35b-4876-9f46-952df2c101cf
### Selecionar o metastore do Unity Catalog a ser usado
-- MAGIC
Um catálogo foi criado para você com um nome específico ao usuário, armazenado na variável **DA.catalog_name**. Vamos começar selecionando esse catálogo no metastore do Unity Catalog. Isso elimina a necessidade de especificar um catálogo nas referências da tabela.

<hr>--i18n-179e80a6-3a69-4506-89b0-c8b26d86fada
### Criar e selecionar o banco de dados a ser usado
-- MAGIC
Como estamos usando um catálogo exclusivo, não precisamos nos preocupar em usar um banco de dados exclusivo para evitar interferir em outros.
-- MAGIC
Vamos criar um banco de dados chamado **bronze_datasets**.

<hr>--i18n-df709457-d477-48eb-9758-54f4ecf17556
Agora vamos selecionar o banco de dados recém-criado para simplificar ainda mais o trabalho com a tabela de destino. Novamente, esse passo não é necessário, mas simplificará ainda mais as referências às tabelas atualizadas.

<hr>--i18n-adecfe89-ab61-4392-92ea-b3847d0e3d17
## Atualizar a tabela
-- MAGIC
A cópia da tabela se resume a uma simples operação **CREATE TABLE AS SELECT** (CTAS), usando o namespace de três níveis para especificar a tabela de origem. Não precisamos especificar o destino usando três níveis devido às instruções **USE CATALOG** e **USE** executadas anteriormente.
-- MAGIC
Observação: esta operação pode ser demorada em tabelas grandes, pois todos os dados da tabela são copiados.

<hr>--i18n-64c49876-3551-4528-be7f-aa9973aac059
A tabela foi copiada, e a nova tabela está sob o controle do Unity Catalog. Vamos verificar rapidamente se há concessões na nova tabela.

<hr>--i18n-2ec10faa-827d-41c7-a697-de4c4ab5ba79
Atualmente não há concessões.
-- MAGIC
Agora vamos examinar as concessões na tabela original. Remova o comentário do código na célula a seguir e execute-o.

<hr>--i18n-dc1999a4-7957-40ec-8a99-cf3cab5c3f77
Isso gera um erro, pois essa tabela reside no metastore herdado e nós não estamos executando em um cluster com o controle de acesso da tabela habilitado. Isso destaca um dos principais benefícios do Unity Catalog: nenhuma configuração adicional é necessária para obter uma solução segura. O Unity Catalog é seguro por default.

<hr>--i18n-bcd994a7-e48a-4c4c-9d8a-df65ea2577d4
## Conceder acesso à tabela [optional]
-- MAGIC
Com a nova tabela implementada, vamos permitir que os usuários no grupo **analysts** a leiam.
-- MAGIC
Observe que você só poderá executar esta seção se tiver acompanhado o exercício *Gerenciar usuários e grupos* e criado um grupo chamado **analysts** no Unity Catalog.
-- MAGIC
Execute esta seção removendo o comentário das células de código e executando-as em sequência. Você também verá uma solicitação para executar algumas queries como um usuário secundário. Para fazer isso:
-- MAGIC
1. Abra uma sessão de navegação privada separada e faça login no Databricks SQL usando o ID de usuário que você criou ao executar *Gerenciar usuários e grupos*.
1. Crie um SQL endpoint seguindo as instruções em *Criar um SQL endpoint no Unity Catalog*.
1. Prepare-se para inserir consultas conforme as instruções abaixo nesse ambiente.

<hr>--i18n-1f594ac2-2b40-451a-a5da-3415c3fe7492
### Conceder o privilégio SELECT na tabela
-- MAGIC
O primeiro requisito é conceder o privilégio **SELECT** na nova tabela ao grupo **analysts**.

<hr>--i18n-1e6e597a-349b-4f83-9ee5-509d6d65db1f
### Conceder o privilégio USAGE no banco de dados
-- MAGIC
O privilégio **USAGE** também é obrigatório no banco de dados.

<hr>--i18n-76a94a64-a0f8-4ed6-914f-0081fe5e0527
### Acessar a tabela como usuário
-- MAGIC
Com as concessões apropriadas implementadas, tente ler a tabela no ambiente do Databricks SQL do usuário secundário. 
-- MAGIC
Execute a célula a seguir para gerar uma instrução de query que lê a tabela recém-criada. Copie e cole a saída em uma nova query no ambiente SQL do usuário secundário e execute-a.

<hr>--i18n-44e957a6-fa12-42d7-b4e3-fce1d9fad020
-- MAGIC
## Transformar a tabela durante a atualização
-- MAGIC
A migração de uma tabela para o Unity Catalog é em si uma operação simples, mas a mudança geral para o Unity Catalog é um passo importante para qualquer organização. A migração é um ótima oportunidade para analisar atentamente as tabelas e os esquemas e verificar se eles ainda atendem aos requisitos comerciais da organização, que podem ter mudado com o tempo.
-- MAGIC
O exemplo que vimos anteriormente utiliza uma cópia exata da tabela de origem. Como a migração de uma tabela é uma operação simples de **CREATE TABLE AS SELECT**, podemos realizar qualquer transformação que possa ser realizada com **SELECT** durante a migração. Por exemplo, vamos expandir o exemplo anterior para fazer as seguintes transformações:
* Atribuir o nome *idx* à primeira coluna
* Além disso, selecionar apenas as colunas **title**, **year**, **budget** e **rating**
* Converter **year** e **budget** em **INT** (substituindo qualquer instância da string *NA* por 0)
* Converter **rating** em **DOUBLE**

<hr>--i18n-b477ffe4-c5de-4fe7-8fec-559daf39e100
Se você estiver executando queries como usuário secundário, execute a query anterior no ambiente do Databricks SQL do usuário secundário. Verifique se:
1. A tabela ainda pode ser acessada
1. O esquema da tabela foi atualizado

<hr>--i18n-c1b1fada-41b8-41e8-af6f-2454369231db
## Limpar
Execute a célula a seguir para remover o banco de dados e a tabela de origem usados neste exemplo.

