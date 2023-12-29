# /DE 4 - Delta Live Tables/DE 4.1A - Pipelines SQL/DE 4.1.1 - Orders Pipeline
<hr>--i18n-c457f8c6-b318-46da-a68f-36f67d8a1b9c
# Fundamentos da sintaxe SQL do DLT
-- MAGIC
Este notebook demonstra o uso de Delta Live Tables (DLT) para processar dados brutos de arquivos JSON que chegam ao armazenamento de objetos em nuvem por meio de uma série de tabelas para processar cargas de trabalho analíticas no lakehouse. A seguir, demonstraremos uma arquitetura medalhão, onde os dados são transformados e enriquecidos de forma incremental à medida que fluem pelo pipeline. Este notebook se concentra na sintaxe SQL do DLT, e não nessa arquitetura, mas esta é uma breve visão geral do seu desenho:
-- MAGIC
* A tabela bronze contém registros brutos carregados de JSON enriquecidos com dados que descrevem como os registros foram ingeridos
* A tabela silver valida e enriquece os campos de interesse
* A tabela gold contém dados agregados para gerar percepções comerciais e criar painéis
-- MAGIC
## Objetivos de aprendizado
-- MAGIC
Ao final deste notebook, os alunos deverão ser capazes de realizar estas tarefas com confiança:
* Declarar Delta Live Tables
* Ingerir dados com o Auto Loader
* Usar parâmetros em pipelines do DLT
* Impor qualidade dos dados com restrições
* Adicionar comentários às tabelas
* Descrever diferenças na sintaxe e na execução de live tables e streaming live tables

<hr>--i18n-736da23b-5e31-4b7c-9f58-b56f3a133714
## Sobre os notebooks da biblioteca DLT
-- MAGIC
A sintaxe do DLT não se destina à execução interativa em um notebook. Este notebook precisa ser programado como parte de um pipeline do DLT para ser executado corretamente. 
-- MAGIC
Se você executar uma célula do notebook de DLT interativamente, deverá ver uma mensagem informando que a instrução é sintaticamente válida. Observe que embora algumas verificações de sintaxe sejam realizadas antes dessa mensagem ser gerada, isso não é uma garantia de que a query terá o desempenho desejado. Discutiremos o desenvolvimento e a solução de problemas de código do DLT posteriormente neste curso.
-- MAGIC
## Parametrização
-- MAGIC
Durante a configuração do pipeline do DLT, diversas opções são especificadas. Uma delas é um par key-value adicionado ao campo **Configurações**.
-- MAGIC
As configurações em pipelines do DLT são semelhantes a parâmetros em jobs do Databricks ou a widgets em notebooks do Databricks.
-- MAGIC
Ao longo destas lições, usaremos **`${source}`** para realizar a substituição de strings do caminho do arquivo definido durante a configuração nas queries SQL.

<hr>--i18n-e47d07ed-d184-4108-bc22-53fd34678a67
## Tabelas como resultados de query
-- MAGIC
O Delta Live Tables adapta queries SQL padrão para combinar DDL (linguagem de definição de dados) e DML (linguagem de manipulação de dados) em uma sintaxe declarativa unificada.
-- MAGIC
Há dois tipos distintos de tabelas persistentes que podem ser criadas com o DLT:
* **Live tables** são views materializadas para o lakehouse que retornarão os resultados atuais de qualquer query a cada refresh
* **Live tables de transmissão** são projetadas para processamento de dados incremental em tempo quase real
-- MAGIC
Observe que ambos os objetos são persistidos como tabelas armazenadas com o protocolo Delta Lake (fornecendo transações ACID, controle de versão e muitos outros benefícios). Falaremos mais sobre as diferenças entre live tables e streaming live tables posteriormente neste notebook.
-- MAGIC
Para ambos os tipos de tabelas, o DLT adota a abordagem de uma instrução CTAS (Create Table as Select) ligeiramente modificada. Os engenheiros só precisam se preocupar em escrever queries para transformar os dados, e o DLT cuida do restante.
-- MAGIC
A sintaxe básica para uma query SQL do DLT é:
-- MAGIC
**`CREATE OR REFRESH [STREAMING] LIVE TABLE table_name`**<br/>
**`AS select_statement`**<br/>

<hr>--i18n-32b81589-fb23-45c0-8317-977a7d4c722a
-- MAGIC
## Ingestão de transmissão com o Auto Loader
-- MAGIC
A Databricks desenvolveu a funcionalidade [Auto Loader](https://docs.databricks.com/ingestion/auto-loader/index.html) para proporcionar execução otimizada para carregamento incremental de dados do armazenamento de objetos em nuvem para o Delta Lake. Usar o Auto Loader com o DLT é simples: basta configurar um diretório de dados de origem, definir algumas configuração e escrever uma query para consultar os dados de origem. O Auto Loader detectará automaticamente novos arquivos de dados à medida que eles chegam ao local de armazenamento de objetos na nuvem de origem, processando novos registros de forma incremental sem a necessidade de realizar varreduras caras e recalcular resultados para datasets em crescimento infinito.
-- MAGIC
O método **`cloud_files()`** permite que o Auto Loader seja usado nativamente com SQL. Esse método usa os seguintes parâmetros posicionais:
* O local de origem, que deve ser um armazenamento de objetos baseado em cloud
* O formato dos dados de origem, que neste caso é JSON
* Uma lista de tamanho arbitrário separada por vírgulas de definições opcionais do leitor. Neste caso, definimos **`cloudFiles.inferColumnTypes`** como **`true`**
-- MAGIC
Na query abaixo, além dos campos contidos na fonte, as funções **`current_timestamp()`** e **`input_file_name()`** do Spark SQL capturam as informações do momento em que o registro foi ingerido e a origem de arquivo específica para cada registro.

<hr>--i18n-4be3b288-fd3b-4380-b32a-29fdb0d499ac
## Validando, enriquecendo e transformando dados
-- MAGIC
O DLT permite que os usuários declarem facilmente tabelas a partir de resultados de qualquer transformação padrão do Spark. O DLT aproveita recursos usados em outras partes do Spark SQL para documentar datasets, ao mesmo tempo que adiciona novas funcionalidades para verificar a qualidade dos dados.
-- MAGIC
Vamos detalhar a sintaxe da query abaixo.
-- MAGIC
### A instrução Select
-- MAGIC
A instrução select contém a lógica central da query. Neste exemplo, vamos:
* Converter o campo **`order_timestamp`** no tipo de carimbo de data/hora
* Selecionar todos os campos restantes (exceto três que não são de nosso interesse, incluindo o **`order_timestamp`** original)
-- MAGIC
Observe que a cláusula **`FROM`** tem duas construções que talvez você não conheça:
* A palavra-chave **`LIVE`** é usada no lugar do nome do esquema para se referir ao esquema de destino configurado para o pipeline do DLT atual
* O método **`STREAM`** permite que os usuários declarem uma fonte de dados streaming para queries SQL
-- MAGIC
Observe que se nenhum esquema de destino for declarado durante a configuração do pipeline, as tabelas não serão publicadas (ou seja, não serão registradas no metastore ou disponibilizadas para queries em outro lugar). O esquema de destino pode ser facilmente alterado na transição entre diferentes ambientes de execução, ou seja, é fácil implantar o mesmo código em cargas de trabalho regionais ou promover um ambiente de desenvolvimento para um ambiente de produção sem precisar incorporar os nomes dos esquemas ao código.
-- MAGIC
### Restrições de qualidade de dados
-- MAGIC
O DLT usa instruções booleanas simples para permitir <a href="https://docs.databricks.com/delta-live-tables/expectations.html#delta-live-tables-data-quality-constraints&language-sql" target="_blank">verificações de imposição de qualidade</a> em dados. Na instrução abaixo, vamos:
* Declarar uma restrição chamada **`valid_date`**
* Definir a verificação condicional de que o campo **`order_timestamp`** deve conter um valor superior a 1º de janeiro de 2021
* Instruir o DLT para falhar na transação atual se algum registro violar a restrição
-- MAGIC
Cada restrição pode ter diversas condições, e é possível definir diversas restrições para uma única tabela. Além de falhar na atualização, a violação de restrição também pode descartar registros automaticamente ou apenas registrar o número de violações e continuar processando os registros inválidos.
-- MAGIC
### Comentários da tabela
-- MAGIC
Os comentários da tabela são padrão no SQL e podem ser usados para fornecer informações úteis aos usuários em toda a organização. Neste exemplo, elaboramos uma breve descrição legível por humanos da tabela que explica como os dados são ingeridos e aplicados (o que também pode ser compreendido revisando outros metadados da tabela).
-- MAGIC
### Propriedades da tabela
-- MAGIC
O campo **`TBLPROPERTIES`** pode ser usado para passar qualquer número de pares key-value para a marcação personalizada de dados. Neste passo, definimos o value **`silver`** para a key **`quality`**.
-- MAGIC
Observe que, embora este campo permita que tags personalizadas sejam definidas arbitrariamente, ele também é usado para definir várias configurações que controlam o desempenho de uma tabela. Ao revisar os detalhes da tabela, você também poderá encontrar diversas configurações que são ativadas por default sempre que uma tabela é criada.

<hr>--i18n-c88a50db-2c24-4117-be3f-193da33e4a5b
## Live tables versus streaming live tables
-- MAGIC
As duas queries que analisamos até agora criaram streaming live tables. Abaixo vemos uma query simples que retorna uma live table (ou view materializada) de alguns dados agregados.
-- MAGIC
Historicamente, o Spark diferenciou queries em batch de queries em streaming. As live tables e as streaming live tables têm diferenças semelhantes.
-- MAGIC
Observe que as únicas diferenças sintáticas entre streaming live tables e live tables são a falta da palavra-chave **`STREAMING`** na cláusula create e o não agrupamento da tabela de origem no método **`STREAM()`**.
-- MAGIC
Algumas das diferenças entre esses tipos de tabelas são listadas abaixo.
-- MAGIC
### Live tables
* Estão sempre "corretas", ou seja, o conteúdo da tabela corresponderá à sua definição após qualquer atualização.
* Retornam os mesmos resultados como se a tabela tivesse sido definida pela primeira vez em todos os dados.
* Não devem ser modificadas por operações fora do pipeline do DLT (você obterá respostas indefinidas ou a alteração será desfeita).
-- MAGIC
### Streaming Live tables
* São compatíveis apenas com a leitura de fontes streaming do tipo somente inserção.
* Leem cada lote de entrada apenas uma vez, não importa o que aconteça (mesmo que as dimensões da junção mudem ou que a definição da query seja alteradas etc.).
* Podem realizar operações na tabela fora do pipeline do DLT gerenciado (acrescentar dados, executar GDPR etc.).

<hr>--i18n-e15b4f61-b33a-4ac5-8b81-c6e7578ce28f
## Resumo
-- MAGIC
Ao concluir a revisão deste notebook, você deverá ser capaz de realizar estas tarefas com confiança:
*Declarando tabelas Delta Live
*Ingestão de dados com o Auto Loader
*Usando parâmetros em pipelines DLT
*Aplicando a qualidade dos dados com restrições
*Adicionando comentários às tabelas
*Descrever diferenças na sintaxe e execução de live tables e streaming live tables
-- MAGIC
No próximo notebook, continuaremos aprendendo sobre essas construções sintáticas enquanto exploramos alguns conceitos novos.
