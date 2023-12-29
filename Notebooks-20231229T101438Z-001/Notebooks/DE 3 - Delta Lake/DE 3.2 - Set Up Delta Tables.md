# /DE 3 - Delta Lake/DE 3.2 - Set Up Delta Tables
<hr>--i18n-c6ad28ff-5ff6-455d-ba62-30880beee5cd
-- MAGIC
-- MAGIC
# Configurando tabelas Delta
-- MAGIC
Depois de extrair dados de fontes de dados externas, carregue-os no Lakehouse para garantir que todos os benefícios da plataforma Databricks possam ser totalmente aproveitados.
-- MAGIC
Embora diferentes organizações possam ter políticas variadas sobre o carregamento inicial de dados no Databricks, normalmente recomendamos que as primeiras tabelas representem uma versão majoritariamente bruta dos dados, e que a validação e o enriquecimento ocorram em fases posteriores. Esse padrão garante que, mesmo que os tipos de dados ou os nomes das colunas não sejam como esperado, nenhum dado será descartado, e a intervenção programática ou manual ainda poderá salvar dados parcialmente corrompidos ou inválidos.
-- MAGIC
Esta lição se concentrará principalmente no padrão usado para criar a maioria das tabelas, as instruções **`CREATE TABLE _ AS SELECT`** (CTAS).
-- MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
- Usar instruções CTAS para criar tabelas do Delta Lake
- Criar tabelas a partir de views ou tabelas existentes
- Enriquecer os dados carregados com metadados adicionais
- Declarar o esquema da tabela com colunas geradas e comentários descritivos
- Definir opções avançadas para controlar a localização dos dados, a aplicação de medidas de qualidade e o particionamento
- Criar clones superficiais e profundos

<hr>--i18n-fe1b3b29-37fb-4e5b-8a50-e2baf7381d24
-- MAGIC
-- MAGIC
## Executar a configuração
-- MAGIC
O script de configuração criará os dados e declarará os valores necessários para a execução do restante deste notebook.

<hr>--i18n-36f64593-7d14-4841-8b1a-fabad267ba22
-- MAGIC
-- MAGIC
-- MAGIC
## Create Table as Select (CTAS)
-- MAGIC
As instruções **`CREATE TABLE AS SELECT`** criam e preenchem tabelas Delta usando dados recuperados de uma query de entrada.

<hr>--i18n-1d3d7f45-be4f-4459-92be-601e55ff0063
-- MAGIC
 
As instruções CTAS inferem automaticamente informações de esquema a partir dos resultados da query e **não** são compatíveis com a declaração manual de esquema. 
-- MAGIC
Isso significa que as instruções CTAS são úteis para a ingestão de dados externos de fontes com esquemas bem definidos, como arquivos e tabelas Parquet.
-- MAGIC
As instruções CTAS também não são compatíveis com a especificação de opções de arquivo adicionais.
-- MAGIC
É fácil perceber que isso apresentaria limitações significativas ao tentar ingerir dados de arquivos CSV.

<hr>--i18n-0ec18c0e-c56b-4b41-8364-4826d1f34dcf
-- MAGIC
-- MAGIC
Para ingerir corretamente esses dados em uma tabela do Delta Lake, precisamos usar uma referência aos arquivos que permita especificar opções.
-- MAGIC
Na lição anterior, mostramos como fazer isso registrando uma tabela externa. Neste passo, usaremos uma ligeira evolução dessa sintaxe para especificar as opções de uma view temporária que usaremos como fonte de uma instrução CTAS para registrar a tabela Delta.

<hr>--i18n-96f21158-ccb9-4fd3-9dd2-c7c7fce1a6e9
-- MAGIC
 
## Filtrar e renomear colunas de tabelas existentes
-- MAGIC
Transformações simples, como alterar nomes de colunas ou omitir colunas de tabelas de destino, podem ser facilmente realizadas durante a criação da tabela.
-- MAGIC
A instrução a seguir cria uma tabela contendo um subconjunto de colunas da tabela **`sales`**. 
-- MAGIC
Neste passo, vamos supor que estamos omitindo intencionalmente informações que podem identificar o usuário ou que fornecem detalhes da compra. Também renomearemos os campos supondo que um sistema posterior usa convenções de nomenclatura diferentes daquela dos dados de origem.

<hr>--i18n-02026f25-a1cf-42e5-b9c3-75b9c1c7ef11
-- MAGIC
-- MAGIC
Observe que poderíamos ter alcançado o mesmo objetivo com uma view, conforme mostrado abaixo.

<hr>--i18n-23f77f7e-21d5-4977-93bc-45800b28535f
-- MAGIC
 
## Declarar o esquema com colunas geradas
-- MAGIC
Conforme observado anteriormente, as instruções CTAS não são compatíveis com declarações de esquema. No exemplo acima, a coluna de carimbo de data/hora parece ser uma variante de um carimbo de data/hora Unix, que pode não ser muito útil para a obtenção de percepções por analistas. Nessa situação, seria melhor usar colunas geradas.
-- MAGIC
Colunas geradas são um tipo especial de coluna cujos valores são gerados automaticamente com base em uma função especificada pelo usuário a partir de outras colunas na tabela Delta (introduzida no DBR 8.3).
-- MAGIC
O código abaixo demonstra a criação de uma tabela ao:
1. Especificar nomes e tipos de colunas
1. Adicionar uma <a href="https://docs.databricks.com/delta/delta-batch.html#deltausegeneratedcolumns" target="_blank">coluna gerada</a> para calcular a data
1. Fornecer um comentário de coluna descritivo para a coluna gerada

<hr>--i18n-33e94ae0-f443-4cc9-9691-30b8b08179aa
-- MAGIC
 
-- MAGIC
Como **`date`** é uma coluna gerada, se gravarmos dados em **`purchase_dates`** sem fornecer valores para a coluna **`date`**, o Delta Lake os calculará automaticamente.
-- MAGIC
**OBSERVAÇÃO**: A célula abaixo define uma configuração que permite gerar colunas usando uma instrução **`MERGE`** do Delta Lake. Falaremos mais sobre essa sintaxe posteriormente no curso.

<hr>--i18n-8da2bdf3-a0e1-4c5d-a016-d9bc38167f50
-- MAGIC
 
O resultado abaixo mostra que todas as datas foram calculadas corretamente à medida que os dados eram inseridos, embora nem os dados de origem nem a query de inserção tenham especificado os valores para o campo.
-- MAGIC
Assim como com qualquer fonte do Delta Lake, a query lê automaticamente o snapshot mais recente da tabela para qualquer query, e você não precisa executar **`REFRESH TABLE`**.

<hr>--i18n-3bb038ec-4e33-40a1-b8ff-33b388e5dda1
-- MAGIC
-- MAGIC
-- MAGIC
É importante observar que, se um campo que seria gerado de outra forma for inserido em uma tabela, essa inserção falhará se o valor fornecido não corresponder exatamente ao valor que seria derivado pela lógica de definição da coluna gerada.
-- MAGIC
Podemos ver esse erro descomentando e executando a célula abaixo:

<hr>--i18n-43e1ab8b-0b34-4693-9d49-29d1f899c210
-- MAGIC
-- MAGIC
## Adicionar uma restrição de tabela
-- MAGIC
A mensagem de erro acima se refere a um **`CHECK constraint`**. As colunas geradas são uma implementação especial de restrições de verificação.
-- MAGIC
Como o Delta Lake impõe o esquema na gravação, o Databricks pode oferecer suporte a cláusulas padrão de gerenciamento de restrições SQL para garantir a qualidade e a integridade dos dados adicionados a uma tabela.
-- MAGIC
Atualmente, o Databricks é compatível com dois tipos de restrições:
* <a href="https://docs.databricks.com/delta/delta-constraints.html#not-null-constraint" target="_blank">restrições **`NOT NULL`**</a>
* <a href="https://docs.databricks.com/delta/delta-constraints.html#check-constraint" target="_blank">restrições **`CHECK`**</a>
-- MAGIC
Em ambos os casos, você precisa garantir que nenhum dado que viole a restrição já existe na tabela antes de definir a restrição. Depois que a restrição for adicionada a uma tabela, os dados que a violarem resultarão em falha de gravação.
-- MAGIC
No passo abaixo, adicionaremos uma restrição **`CHECK`** à coluna **`date`** da tabela. Observe que as restrições **`CHECK`** se parecem com as cláusulas **`WHERE`** padrão que você pode usar para filtrar um dataset.

<hr>--i18n-32fe077c-4e4d-4830-9a80-9a6a2b5d2a61
-- MAGIC
-- MAGIC
As restrições da tabela são mostradas no campo **`TBLPROPERTIES`**.

<hr>--i18n-07f549c0-71af-4271-a8f5-91b4237d89e4
-- MAGIC
-- MAGIC
## Enriquecer tabelas com opções e metadados adicionais
-- MAGIC
Até agora, falamos muito pouco sobre as opções disponíveis para o enriquecimento de tabelas do Delta Lake.
-- MAGIC
Abaixo, mostramos a evolução de uma instrução CTAS para incluir diversas configurações e metadados adicionais.
-- MAGIC
A cláusula **`SELECT`** usa dois comandos integrados do Spark SQL úteis para a ingestão de arquivos:
* **`current_timestamp()`** registra o carimbo de data/hora quando a lógica é executada
* **`input_file_name()`** registra o arquivo de dados de origem para cada registro na tabela
-- MAGIC
Também incluímos lógica para criar uma nova coluna de data derivada dos dados de carimbo de data/hora na origem.
-- MAGIC
A cláusula **`CREATE TABLE`** contém várias opções:
* Um **`COMMENT`** é adicionado para permitir a descoberta mais fácil do conteúdo da tabela
* Um **`LOCATION`** é especificado, o que resultará em uma tabela externa (em vez de gerenciada)
* Uma tabela é **`PARTITIONED BY`** uma coluna de data, ou seja, os dados originados de cada dado existirão em seu próprio diretório no local de armazenamento de destino
-- MAGIC
**OBSERVAÇÃO**: O particionamento é mostrado aqui principalmente para demonstrar a sintaxe e o impacto. A maioria das tabelas do Delta Lake (especialmente as pequenas e médias) não se beneficiarão do particionamento. Como o particionamento separa fisicamente os arquivos de dados, essa abordagem pode causar problemas com arquivos pequenos, além de impedir a compactação de arquivos e a omissão eficiente de dados. Os benefícios observados no Hive ou no HDFS não são replicados no Delta Lake, e você deve consultar um arquiteto com experiência no Delta Lake antes de particionar tabelas.
-- MAGIC
**A prática recomendada é adotar tabelas não particionadas como default para a maioria dos casos de uso no Delta Lake.**

<hr>--i18n-431d473d-162f-4a97-9afc-df47a787f409
-- MAGIC
 
Os campos de metadados adicionados à tabela fornecem informações úteis para entender quando os registros foram inseridos e de onde vieram. Isso pode ser especialmente útil para diagnosticar problemas nos dados de origem.
-- MAGIC
Todos os comentários e as propriedades de uma determinada tabela podem ser revisados usando **`DESCRIBE TABLE EXTENDED`**.
-- MAGIC
**OBSERVAÇÃO**: O Delta Lake adiciona automaticamente várias propriedades de tabela ao criá-la.

<hr>--i18n-227fec69-ab44-47b4-aef3-97a14eb4384a
-- MAGIC
 
Listar o local usado para a tabela revela que os valores exclusivos na coluna de partição **`first_touch_date`** são usados para criar diretórios de dados.

<hr>--i18n-d188161b-3bc6-4095-aec9-508c09c14e0c
-- MAGIC
-- MAGIC
## Clonando tabelas do Delta Lake
O Delta Lake tem duas opções para a cópia eficiente de tabelas.
-- MAGIC
**`DEEP CLONE`** copia todos os dados e metadados de uma tabela de origem em um destino. Como a cópia ocorre de forma incremental, executar esse comando novamente pode sincronizar as alterações entre o local de origem e o local de destino.

<hr>--i18n-c0aa62a8-7448-425c-b9de-45284ea87f8c
-- MAGIC
-- MAGIC
Como é necessário copiar todos os arquivos de dados, essa operação pode ser um pouco demorada com grandes datasets.
-- MAGIC
Quando você quer copiar rapidamente uma tabela para testar a aplicação de alterações sem o risco de modificar a tabela atual, **`SHALLOW CLONE`** pode ser uma boa opção. Os clones superficiais apenas copiam os logs de transações Delta, o que significa que os dados não são movidos.

<hr>--i18n-045bfc09-41cc-4710-ab67-acab3881f128
-- MAGIC
-- MAGIC
Em ambos os casos, as modificações de dados aplicadas à versão clonada da tabela serão rastreadas e armazenadas separadamente da fonte. A clonagem é uma ótima maneira de configurar tabelas para testar código SQL ainda em desenvolvimento.

<hr>--i18n-32c272d6-cbe8-43ba-b051-9fe8e5586990
-- MAGIC
-- MAGIC
## Resumo
-- MAGIC
Neste notebook, nos concentramos principalmente em DDL e na sintaxe para a criação de tabelas do Delta Lake. No próximo notebook, exploraremos opções para gravar atualizações em tabelas.

<hr>--i18n-c87f570b-e175-4000-8706-c571aa1cf6e1
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

