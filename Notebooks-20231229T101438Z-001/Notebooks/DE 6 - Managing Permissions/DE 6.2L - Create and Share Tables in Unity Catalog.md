# /DE 6 - Gerenciando permissões/DE 6.2L - Criar e compartilhar tabelas no Unity Catalog
<hr>--i18n-246366e0-139b-4ee6-8231-af9ffc8b9f20
# Criar e compartilhar tabelas no Unity Catalog
-- MAGIC
Neste notebook você aprenderá como:
* Criar esquemas e tabelas
* Controlar o acesso a esquemas e tabelas
*Explorar concessões em vários objetos no Unity Catalog

<hr>--i18n-675fb98e-c798-4468-9231-710a39216650
## Configuração
-- MAGIC
Execute as células a seguir para realizar a configuração. 
-- MAGIC
Para evitar conflitos em um ambiente de treinamento compartilhado, isso gerará um nome de catálogo exclusivo para seu uso. 
-- MAGIC
Em seu próprio ambiente, você é livre para escolher os nomes dos catálogos, mas tome cuidado para não afetar outros usuários e sistemas neste ambiente.

<hr>--i18n-1f8f7a5b-7c09-4333-9057-f9e25f635f94
## Namespace de três níveis do Unity Catalog
-- MAGIC
A maioria dos desenvolvedores de SQL está familiarizada com o uso de um namespace de dois níveis para lidar com tabelas em um esquema sem ambiguidades, como descrito a seguir:
-- MAGIC
    SELECT * FROM schema.table;
-- MAGIC
O Unity Catalog introduz o conceito de um *catálogo* que reside acima do esquema na hierarquia de objetos. Os metastores podem hospedar qualquer número de catálogos, que, por sua vez, podem hospedar qualquer número de esquemas. Para lidar com esse nível adicional, as referências completas de tabela no Unity Catalog usam namespaces de três níveis. A instrução a seguir exemplifica isso:
-- MAGIC
    SELECT * FROM catalog.schema.table;
    
Os desenvolvedores de SQL provavelmente também estão familiarizados com a instrução **`USE`** para selecionar um esquema default, a fim de evitar a necessidade de sempre especificar um esquema ao fazer referência a tabelas. O Unity Catalog amplifica esse recurso com a instrução **`USE CATALOG`**, que também seleciona um catálogo default.
-- MAGIC
Para simplificar sua experiência, criamos o catálogo e o definimos como default, como você pode ver no comando a seguir.

<hr>--i18n-7608a78c-6f96-4f0b-a9fb-b303c76ad899
## Criar e usar um novo esquema
-- MAGIC
Vamos criar um esquema exclusivamente para nosso uso neste exercício e, em seguida, defini-lo como default para podermos fazer referência a tabelas apenas pelo nome.

<hr>--i18n-013687c8-6a3f-4c8b-81c1-0afb0429914f
## Criar a arquitetura do Delta
-- MAGIC
Vamos criar e preencher uma coleção simples de esquemas e tabelas de acordo com a arquitetura do Delta:
* Um esquema prata contendo dados de frequência cardíaca de pacientes lidos de um dispositivo médico
* Uma tabela de esquema ouro que calcula diariamente a média dos dados de frequência cardíaca por paciente
-- MAGIC
Por enquanto, não haverá tabela bronze neste exemplo simples.
-- MAGIC
Observe que precisamos apenas especificar o nome da tabela abaixo, pois definimos um catálogo e um esquema default acima.

<hr>--i18n-3961e85e-2bba-460a-9e00-12e37a07cb87
## Conceder acesso ao esquema ouro [optional]
-- MAGIC
Agora vamos permitir que os usuários do grupo **account users** leiam o esquema **gold**.
-- MAGIC
Execute esta seção removendo o comentário das células de código e executando-as em sequência. 
Você também verá uma solicitação para executar algumas queries. 
-- MAGIC
Para fazer isso:
1. Abra uma tab separada do navegador e carregue seu Databricks workspace.
1. Alterne para o Databricks SQL clicando no alternador de aplicativos e selecionando SQL.
1. Crie um SQL warehouse seguindo as instruções em *Criar um SQL Warehouse no Unity Catalog*.
1. Prepare-se para inserir queries no ambiente conforme as instruções abaixo.

<hr>--i18n-e4ec17fc-7dfa-4d3f-a82f-02eca61e6e53
Vamos conceder o privilégio **SELECT** na tabela **gold**.

<hr>--i18n-d20d5b94-b672-48cf-865a-7c9aa0629955
### Consultar a tabela como usuário
-- MAGIC
Com a concessão **SELECT** implementada, tente consultar a tabela no ambiente do Databricks SQL.
-- MAGIC
Execute a célula a seguir para gerar uma instrução de query que lê a tabela **gold**. Copie e cole a saída em uma nova query no ambiente SQL e execute-a.

<hr>--i18n-3309187d-5a52-4b51-9198-7fc54ea9ca84
Esse comando funciona em nosso caso, pois somos os proprietários da view. No entanto, ele não funcionará para outros membros do grupo **account users** porque o privilégio **SELECT** somente na tabela é insuficiente. O privilégio **USAGE** também é necessário nos elementos contendo os dados. Vamos corrigir isso agora executando o seguinte.

<hr>--i18n-a6fcc71b-ceb3-4067-acb2-85504ad4d6ea
-- MAGIC
Repita a query no ambiente do Databricks SQL e, com essas duas concessões em vigor, a operação deverá ser bem-sucedida.

<hr>--i18n-5dd6b7c1-6fed-4d09-b808-0615a10b2502
-- MAGIC
## Explorar concessões
-- MAGIC
Vamos explorar as concessões em alguns dos objetos na hierarquia do Unity Catalog, começando com a tabela **gold**.

<hr>--i18n-e7b6ddda-b6ef-4558-a5b6-c9f97bb7db80
No momento, há apenas a concessão **SELECT** que configuramos anteriormente. Agora vamos verificar as concessões em **silver**.

<hr>--i18n-1e9d3c88-af92-4755-a01c-6e0aefdd8c9d
No momento, não há concessões nessa tabela, e somente o proprietário pode acessá-la.
-- MAGIC
Agora vamos examinar o esquema que o contém.

<hr>--i18n-17125954-7b25-40dc-ab84-5a159198e9fc
No momento, não há concessões nesse esquema. 
-- MAGIC
Agora vamos examinar o catálogo.

<hr>--i18n-7453efa0-62d0-4af2-901f-c222dd9b2d07
Atualmente vemos a concessão de** USAGE** que configuramos anteriormente.

<hr>--i18n-b4f88042-e7cb-4a1f-b853-cb328356778c
## Limpar
Execute a célula a seguir para remover o esquema que criamos neste exemplo.

