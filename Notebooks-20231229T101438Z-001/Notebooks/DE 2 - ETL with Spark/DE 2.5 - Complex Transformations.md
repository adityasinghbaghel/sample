# /DE 2 - ETL with Spark/DE 2.5 - Complex Transformations
<hr>--i18n-daae326c-e59e-429b-b135-5662566b6c34
-- MAGIC
-- MAGIC
# Transformações complexas
-- MAGIC
Consultar dados tabulares armazenados no data lakehouse com o Spark SQL é fácil, eficiente e rápido.
-- MAGIC
A complexidade desse processo aumenta à medida que a estrutura de dados se torna menos regular, quando muitas tabelas precisam ser usadas em uma única query ou quando o formato dos dados precisa ser alterado drasticamente. Este notebook apresenta uma série de funções presentes no Spark SQL para ajudar os engenheiros a realizar até mesmo as transformações mais complicadas.
-- MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
- Usar a sintaxe **`.`** e **`:`** para consultar dados aninhados
- Extrair strings JSON e convertê-las em estruturas
- Nivelar e descompactar matrizes e estruturas
- Combinar datasets usando junções
- Remodelar dados usando tabelas dinâmicas

<hr>--i18n-b01af8a2-da4a-4c8f-896e-790a60dc8d0c
-- MAGIC
-- MAGIC
## Executar a configuração
-- MAGIC
O script de configuração criará os dados e declarará os valores necessários para a execução do restante deste notebook.

<hr>--i18n-a6be8b8a-1c1f-40dd-a71c-8e91ae079b5c
-- MAGIC
## Visão geral dos dados
-- MAGIC
A tabela **`events_raw`** foi registrada com base nos dados que representam uma carga do Kafka. Na maioria dos casos, os dados do Kafka são valores JSON com codificação binária. 
-- MAGIC
Vamos converter **`key`** e **`value`** em strings para visualizar esses valores em um formato legível por humanos.

<hr>--i18n-67712d1a-cae1-41dc-8f7b-cc97e933128e
## Manipular tipos complexos

<hr>--i18n-c6a0cd9e-3bdc-463a-879a-5551fa9a8449
-- MAGIC
### Trabalhar com dados aninhados
-- MAGIC
A célula de código abaixo consulta as strings convertidas para visualizar um objeto JSON de exemplo sem campos nulos (precisaremos desse resultado na próxima seção).
-- MAGIC
**OBSERVAÇÃO:** O Spark SQL tem funcionalidade integrada para interagir diretamente com dados aninhados armazenados como strings JSON ou tipos de estrutura.
- Use a sintaxe **`:`** em queries para acessar subcampos em strings JSON
- Use a sintaxe **`.`** em queries para acessar subcampos em tipos de estrutura

<hr>--i18n-914b04cd-a1c1-4a91-aea3-ecd87714ea7d
Vamos usar o exemplo de string JSON acima para derivar o esquema e, em seguida, extrair toda a coluna JSON e converter os dados em tipos de estrutura.
- **`schema_of_json()`** retorna o esquema derivado de uma string JSON de exemplo.
- **`from_json()`** extrai uma coluna contendo uma string JSON e converte-a em um tipo de estrutura usando o esquema especificado.
-- MAGIC
Depois de descompactar a string JSON como um tipo de estrutura, vamos descompactar e nivelar todos os campos de estrutura em colunas.
A descompactação de - **`*`** pode ser usada para nivelar estruturas; **`col_name.*`** extrai os subcampos de **`col_name`** em suas próprias colunas.

<hr>--i18n-5ca54e9c-dcb7-4177-99ab-77377ce8d899
### Manipular matrizes
-- MAGIC
O Spark SQL tem diversas funções para manipular dados de matrizes, incluindo:
- **`explode()`** separa os elementos de uma matriz em múltiplas linhas, o que cria uma nova linha para cada elemento.
- **`size()`** fornece uma contagem do número de elementos em uma matriz para cada linha.
-- MAGIC
O código abaixo explode o campo **`items`** (uma matriz de estruturas) em múltiplas linhas e mostra eventos contendo matrizes com três ou mais itens.

<hr>--i18n-0810444d-1ce9-4cb7-9ba9-f4596e84d895
O código abaixo combina transformações de matriz para criar uma tabela que mostra a coleção exclusiva de ações e os itens no carrinho do usuário.
- **`collect_set()`** coleta valores exclusivos para um campo, incluindo campos dentro de matrizes.
- **`flatten()`** combina várias matrizes em uma única matriz.
- **`array_distinct()`** remove elementos duplicados de uma matriz.

<hr>--i18n-8744b315-393b-4f8b-a8c1-3d6f9efa93b0
 
## Combinar e remodelar dados

<hr>--i18n-15407508-ba1c-4aef-bd40-1c8eb244ed83
 
### Unir tabelas
-- MAGIC
O Spark SQL dá suporte a operações **`JOIN`** padrão (inner, outer, left, right, anti, cross, semi).  
Neste passo, vamos unir o dataset de eventos explodidos a uma tabela de pesquisa para obter o nome do item impresso padrão.

<hr>--i18n-6c1f0e6f-c4f0-4b86-bf02-783160ea00f7
### Tabelas dinâmicas
-- MAGIC
Podemos usar **`PIVOT`** para visualizar dados de diferentes perspectivas, girando valores exclusivos em uma coluna dinâmica especificada em várias colunas com base em uma função agregada.
-A cláusula **`PIVOT`** segue o nome da tabela ou a subquery especificada em uma cláusula **`FROM`**, que é a entrada para a tabela dinâmica.
- Os valores exclusivos na coluna dinâmica são agrupados e agregados usando a expressão de agregação fornecida, criando uma coluna separada para cada valor exclusivo na tabela dinâmica resultante.
-- MAGIC
A célula de código a seguir usa **`PIVOT`** para nivelar as informações de compra do item contidas em vários campos derivados do dataset **`sales`**. O formato nivelado dos dados pode ser útil para criar painéis, mas também para aplicar algoritmos de machine learning para inferência ou previsão.

<hr>--i18n-b89c0c3e-2352-4a82-973d-7e655276bede
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

