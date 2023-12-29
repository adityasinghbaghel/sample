# /DE 4 - Delta Live Tables/DE 4.1A - Pipelines SQL/DE 4.1.2 - Customers Pipeline
<hr>--i18n-ff0a44e1-46a3-4191-b583-5c657053b1af
# Sintaxe SQL adicional do DLT
-- MAGIC
Os pipelines do DLT facilitam a combinação de vários datasets em uma única carga de trabalho escalável usando um ou vários notebooks.
-- MAGIC
No último notebook, revisamos algumas das funcionalidades básicas da sintaxe DLT durante o processamento de dados do armazenamento de objetos em cloud por meio de uma série de queries para validar e enriquecer registros em cada etapa. Este notebook segue a arquitetura medallion, mas introduz uma série de novos conceitos.
* Os registros brutos representam informações de captura de dados de alterações (CDC) sobre clientes 
* A tabela bronze usa novamente o Auto Loader para ingerir dados JSON do armazenamento de objetos em cloud
* Uma tabela é definida para impor restrições antes de passar os registros para a camada prata
* **`APPLY CHANGES INTO`** é usado para processar automaticamente dados de CDC na camada prata como uma <a href="https://en.wikipedia.org/wiki/Slowly_changing_dimension" target="_blank">tabela de dimensões que mudam lentamente (SCD)<a/> de Tipo 1
* Uma tabela ouro é definida para calcular um agregado da versão atual da tabela de Tipo 1
* É definida uma view que é unida a tabelas definidas em outro notebook
-- MAGIC
## Objetivos de aprendizado
-- MAGIC
Ao final desta lição, os alunos deverão ser capazes de realizar estas tarefas com confiança:
* Processar dados de CDC com **`APPLY CHANGES INTO`**
* Declarar views ativas
* Unir live tables
* Descrever como os notebooks da biblioteca DLT funcionam juntos em um pipeline
* Programar vários notebooks em um pipeline do DLT

<hr>--i18n-959211b8-33e7-498b-8da9-771fdfb0978b
## Ingerir dados com o Auto Loader
-- MAGIC
Como no último notebook, definimos uma tabela bronze a partir de uma fonte de dados configurada com o Auto Loader.
-- MAGIC
Observe que o código abaixo omite a opção Auto Loader para inferir o esquema. Quando os dados são ingeridos do JSON sem um esquema fornecido ou inferido, os campos terão os nomes corretos, mas todos serão armazenados como tipo **`STRING`**.
-- MAGIC
O código abaixo também fornece um comentário simples e adiciona campos para o horário da ingestão de dados e o nome do arquivo para cada registro.

<hr>--i18n-43d96582-8a29-49d0-b57f-0038334f6b88
## Imposição da qualidade continuada
-- MAGIC
A query abaixo demonstra:
* As três opções de comportamento quando as restrições são violadas
* Uma query com múltiplas restrições
* Múltiplas condições fornecidas para uma restrição
* O uso de uma função SQL integrada em uma restrição
-- MAGIC
Sobre a fonte de dados:
* Os dados são um feed do CDC que contém as operações **`INSERT`**, **`UPDATE`** e **`DELETE`**. 
* As operações update e insert devem conter entradas válidas para todos os campos.
* As operações delete devem conter valores **`NULL`** para todos os campos, exceto os campos de carimbo de data/hora, **`customer_id`** e os campos de operação.
-- MAGIC
Para garantir que apenas dados válidos cheguem à tabela prata, escreveremos várias regras de imposição de qualidade que ignoram os valores nulos esperados nas operações delete.
-- MAGIC
Descreveremos cada uma dessas restrições abaixo:
-- MAGIC
##### **`valid_id`**
Esta restrição fará a transação falhar se um registro tiver um valor nulo no campo **`customer_id`**.
-- MAGIC
##### **`valid_operation`**
Esta restrição descartará registros com um valor nulo no campo **`operation`**.
-- MAGIC
##### **`valid_address`**
Esta restrição verifica se o campo **`operation`** é **`DELETE`**; em caso negativo, verifica se há valores nulos em qualquer um dos quatro campos que compõem um endereço. Como não há instruções adicionais sobre o que fazer com registros inválidos, as linhas que violarem serão registradas nas métricas, mas não descartadas.
-- MAGIC
##### **`valid_email`**
Esta restrição usa correspondência de padrões regex para verificar se o valor no campo **`email`** é um endereço de email válido. A lógica da restrição define que ela não será aplicada aos registros se o campo **`operation`** for **`DELETE`** (porque o campo **`email`** terá um valor nulo). Os registros que violarem a restrição serão descartados.

<hr>--i18n-5766064f-1619-4468-ac05-2176451e11c0
## Processar dados de CDC com **`APPLY CHANGES INTO`**
-- MAGIC
O DLT introduz uma nova estrutura sintática para simplificar o processamento de feeds do CDC.
-- MAGIC
**`APPLY CHANGES INTO`** tem as seguintes garantias e requisitos:
* Executa a ingestão incremental/de transmissão de dados de CDC
* Fornece uma sintaxe simples para especificar um ou mais campos como key primária de uma tabela
* A suposição default é que as linhas terão inserções e atualizações
* Opcionalmente pode aplicar exclusões
* Ordena automaticamente registros de pedidos atrasados usando uma key de sequenciamento fornecida pelo usuário
* Usa uma sintaxe simples para especificar colunas a serem ignoradas com a palavra-chave **`EXCEPT`**
* O comportamento default é aplicar alterações como SCD de Tipo 1
-- MAGIC
O código abaixo:
* Cria a tabela **`customers_silver`**; **`APPLY CHANGES INTO`** exige que a tabela de destino seja declarada em uma instrução separada
* Identifica a tabela **`customers_silver`** como o destino no qual as alterações serão aplicadas
* Especifica a tabela **`customers_bronze_clean`** como fonte de transmissão
* Identifica **`customer_id`** como a key primária
* Especifica que os registros onde o campo **`operation`** é **`DELETE`** devem ser aplicados como exclusões
* Especifica o campo **`timestamp`** para ordenar como as operações devem ser aplicadas
* Indica que todos os campos devem ser adicionados à tabela de destino, exceto **`operation`**, **`source_file`** e **`_rescued_data`**

<hr>--i18n-5956effc-3fb5-4473-b2e7-297e5a3e1103
## Consultando tabelas com alterações aplicadas
-- MAGIC
**`APPLY CHANGES INTO`**, por default, cria uma tabela SCD de Tipo 1, o que significa que cada key exclusiva terá no máximo um registro e que as atualizações substituirão as informações originais.
-- MAGIC
Embora o destino da nossa operação na célula anterior tenha sido definido como uma live table de transmissão, os dados estão sendo atualizados e excluídos da tabela (o que não atende aos requisitos de fontes de live tables de transmissão do tipo somente inserção). Como tal, as operações posteriores não podem realizar queries de transmissão na tabela. 
-- MAGIC
Esse padrão garante que, se alguma atualização ocorrer fora de ordem, os resultados posteriores poderão ser recalculados adequadamente para refletir as atualizações. Além disso, quando os registros são excluídos de uma tabela de origem, esses valores não serão mais refletidos nas tabelas no pipeline posteriormente.
-- MAGIC
No passo abaixo, definimos uma query agregada simples para criar uma live table a partir dos dados na tabela **`customers_silver`**.

<hr>--i18n-cd430cf6-927e-4a2e-a68d-ba87b9fdf3f6
## Views do DLT
-- MAGIC
A query abaixo define uma view do DLT substituindo **`TABLE`** pela palavra-chave **`VIEW`**.
-- MAGIC
As views no DLT diferem das tabelas persistentes e podem ser definidas como **`STREAMING`**.
-- MAGIC
As views têm as mesmas garantias de atualização que as live tables, mas os resultados das queries não são armazenados em disco.
-- MAGIC
Ao contrário das views usadas em outros lugares no Databricks, as views do DLT não são persistidas no metastore, ou seja, elas só podem ser referenciadas no pipeline do DLT do qual fazem parte. (Esse é um escopo semelhante às views temporárias na maioria dos sistemas SQL.)
-- MAGIC
As views ainda podem ser usadas para impor a qualidade dos dados, e as métricas das views são coletadas e relatadas como acontece com as tabelas.
-- MAGIC
## Junções e tabelas de referência entre bibliotecas de notebooks
-- MAGIC
O código que analisamos até agora mostrou dois datasets de origem se propagando por meio de uma série de etapas em notebooks separados.
-- MAGIC
O DLT oferece suporte à programação de vários notebooks como parte de uma única configuração de pipeline do DLT. Você pode editar pipelines do DLT existentes para adicionar notebooks.
-- MAGIC
Dentro de um pipeline do DLT, o código em qualquer biblioteca de notebook pode fazer referência a tabelas e views criadas em qualquer outra biblioteca de notebook.
-- MAGIC
Essencialmente, podemos considerar o escopo da referência do esquema pela palavra-chave **`LIVE`** como estando no nível do pipeline do DLT, e não de um notebook individual.
-- MAGIC
Na query abaixo, criamos uma nova view juntando as tabelas pratas dos datasets **`orders`** e **`customers`**. Observe que esta view não está definida como transmissão. Assim, o **`email`** válido atual de cada cliente será sempre capturado e os registros dos clientes serão descartados automaticamente depois que forem excluídos da tabela **`customers_silver`**.

<hr>--i18n-d4a1dd79-e8e6-4d18-bf00-c5d49cd04b04
## Adicionando este notebook a um pipeline do DLT
-- MAGIC
Com a UI do DLT, é fácil adicionar bibliotecas de notebook a um pipeline existente.
-- MAGIC
1. Navegue até o pipeline do DLT que você configurou anteriormente no curso
1. Clique no botão **Configurações** no canto superior direito
1. Em **Bibliotecas de notebooks**, clique em **Adicionar biblioteca de notebooks**
   * Use o seletor de arquivos para selecionar este notebook e clique em **Selecionar**
1. Clique no botão **Salvar** para salvar as atualizações
1. Clique no botão azul **Começar** no canto superior direito da tela para atualizar o pipeline e processar novos registros
-- MAGIC
<img src="https://files.training.databricks.com/images/icon_hint_24.png"> O link para este notebook pode ser encontrado em [DE 4.1 - Passo a passo da IU do DLT]($../DE 4.1 - Passo a passo da IU do DLT)<br/>
nas instruções impressas da **Tarefa 2** na seção **Gerar a configuração do pipeline**

<hr>--i18n-5c0ccf2a-333a-4d7c-bfef-b4b25e56b3ca
## Resumo
-- MAGIC
Ao revisar este caderno, você agora deve se sentir confortável:
*Processando dados do CDC com**`APPLY CHANGES INTO`**
*Declarando visualizações ao vivo
*Juntando-se a mesas ao vivo
*Descrevendo como os notebooks da biblioteca DLT funcionam juntos em um pipeline
*Agendando vários notebooks em um pipeline DLT
-- MAGIC
No próximo notebook, vamos analisar a saída do pipeline. Em seguida, veremos como desenvolver iterativamente e solucionar problemas de código do DLT.
