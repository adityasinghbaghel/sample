# /DE 2 - ETL with Spark/DE 2.7A - SQL UDFs
<hr>--i18n-5ec757b3-50cf-43ac-a74d-6902d3e18983
-- MAGIC
-- MAGIC
# UDFs SQL e fluxo de controle
-- MAGIC
-- MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
* Definir e registrar UDFs SQL
* Descrever o modelo de segurança usado para compartilhar UDFs SQL
* Usar instruções **`CASE`**/**`WHEN`** em código SQL
* Usar instruções **`CASE`**/**`WHEN`** em UDFs SQL para personalizar o fluxo de controle

<hr>--i18n-fd5d37b8-b720-4a88-a2cf-9b3c43f697eb
-- MAGIC
-- MAGIC
## Executar a configuração
Execute a célula a seguir para configurar o ambiente.

<hr>--i18n-e8fa445b-db52-43c4-a649-9904526c6a04
-- MAGIC
## Funções definidas pelo usuário
-- MAGIC
As funções definidas pelo usuário (UDFs) no Spark SQL permitem registrar lógica SQL personalizada como funções em um banco de dados que podem ser reutilizadas em qualquer execução de SQL no Databricks. Essas funções são registradas nativamente em SQL e mantêm todas as otimizações do Spark ao aplicar lógica personalizada a grandes datasets.
-- MAGIC
No mínimo, a criação de uma UDF SQL requer um nome de função, parâmetros opcionais, o tipo a ser retornado e uma lógica personalizada.
-- MAGIC
A função simples chamada **`sale_announcement`**, mostrada abaixo, utiliza **`item_name`** e **`item_price`** como parâmetros. Essa função retorna uma string que anuncia a venda de um item por 80% do preço original.

<hr>--i18n-5a5dfa8f-f9e7-4b5f-b229-30bed4497009
-- MAGIC
Observe que essa função é aplicada a todos os valores da coluna de forma paralela dentro do mecanismo de processamento do Spark. As UDFs SQL são uma forma eficiente de definir lógica personalizada otimizada para execução no Databricks.

<hr>--i18n-f9735833-a4f3-4966-8739-eb351025dc28
-- MAGIC
## Escopo e permissões de UDFs SQL
As funções SQL definidas pelo usuário:
- Persistem entre ambientes de execução (que podem incluir notebooks, queries DBSQL e jobs).
- Existem como objetos no metastore e são governadas pelas mesmas ACLs de tabela usadas por bancos de dados, tabelas ou views.
- Para **criar** uma UDF SQL, você precisa definir **`USE CATALOG`** no catálogo e **`USE SCHEMA`** e **`CREATE FUNCTION`** no esquema.
- Para **usar** uma UDF SQL, você precisa definir **`USE CATALOG`** no catálogo, **`USE SCHEMA`** no esquema e **`EXECUTE`** na função.
-- MAGIC
Podemos usar **`DESCRIBE FUNCTION`** para verificar onde uma função foi registrada e obter informações básicas sobre as entradas esperadas e o retorno dado (é possível obter ainda mais informações com **`DESCRIBE FUNCTION EXTENDED`**).

<hr>--i18n-091c02b4-07b5-4b2c-8e1e-8cb561eed5a3
-- MAGIC
Observe que o campo **`Body`** na parte inferior da descrição da função mostra a lógica SQL usada na própria função.

<hr>--i18n-bf549dbc-edb7-465f-a310-0f5c04cfbe0a
-- MAGIC
-- MAGIC
## Funções simples de fluxo de controle
-- MAGIC
Combinar UDFs SQL e fluxo de controle usando cláusulas **`CASE`**/**`WHEN`** otimizada a execução de fluxos de controle em cargas de trabalho SQL. A construção sintática de SQL padrão **`CASE`**/**`WHEN`** permite avaliar múltiplas instruções condicionais com resultados alternativos baseados no conteúdo da tabela.
-- MAGIC
No passo a seguir, demonstramos o agrupamento dessa lógica de fluxo de controle em uma função que poderá ser reutilizada em qualquer execução de SQL. 

<hr>--i18n-14f5f9df-d17e-4e6a-90b0-d22bbc4e1e10
-- MAGIC
-- MAGIC
Embora os exemplos fornecidos aqui sejam simples, esses mesmos princípios básicos podem ser usados para adicionar cálculos e lógica personalizados para execução nativa no Spark SQL. 
-- MAGIC
Especialmente no caso de empresas migrando usuários de sistemas com muitos procedimentos definidos ou fórmulas personalizadas, as UDFs SQL podem permitir que alguns usuários definam a lógica complexa necessária para relatórios comuns e queries analíticas.

<hr>--i18n-451ef10d-9e38-4b71-ad69-9c2ed74601b5
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

