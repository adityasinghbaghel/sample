# /DE 2 - ETL with Spark/DE 2.6L - Reshape Data Lab
<hr>--i18n-af5bea55-ebfc-4d31-a91f-5d6cae2bc270
-- MAGIC
-- MAGIC
# Laboratório de remodelagem de dados
-- MAGIC
Neste laboratório, você criará uma tabela **`clickpaths`** que agrega o número de vezes que cada usuário realizou uma determinada ação em **`events`** e unirá essas informações em uma view simplificada de **`transactions`** para criar um registro das ações e compras finais de cada usuário.
-- MAGIC
A tabela **`clickpaths`** deve conter todos os campos de **`transactions`**, bem como uma contagem de cada **`event_name`** de **`events`** em sua própria coluna. Essa tabela deve conter uma única linha para cada usuário que concluiu uma compra.
-- MAGIC
## Objetivos de aprendizado
Ao final deste laboratório, você será capaz de:
- Dinamizar e unir tabelas para criar caminhos de clique para cada usuário

<hr>--i18n-5258fa9b-065e-466d-9983-89f0be627186
-- MAGIC
-- MAGIC
## Executar a configuração
-- MAGIC
O script de configuração criará os dados e declarará os valores necessários para a execução do restante deste notebook.

<hr>--i18n-082bfa19-8e4e-49f7-bd5d-cd833c471109
-- MAGIC
-- MAGIC
-- MAGIC
Usaremos Python para executar verificações ocasionalmente durante o laboratório. As funções auxiliares abaixo retornarão um erro com uma mensagem informando o que precisa ser alterado caso você não tenha seguido as instruções. Nenhuma saída será gerada se você tiver concluído este passo.

<hr>--i18n-2799ea4f-4a8e-4ad4-8dc1-a8c1f807c6d7
-- MAGIC
## Dinamizar eventos para obter contagens de eventos para cada usuário
-- MAGIC
Vamos começar dinamizando a tabela **`events`** para obter contagens para cada **`event_name`**.
-- MAGIC
Queremos agregar o número de vezes que cada usuário realizou um evento específico, especificado na coluna **`event_name`**. Para fazer isso, agrupe por **`user_id`** e dinamize por **`event_name`** para fornecer uma contagem de cada tipo de evento em uma coluna separada, resultando no esquema abaixo. Observe que **`user_id`** é renomeado para **`user`** no esquema de destino.
-- MAGIC
| campo | tipo | 
| --- | --- | 
| user | STRING |
| cart | BIGINT |
| pillows | BIGINT |
| login | BIGINT |
| main | BIGINT |
| careers | BIGINT |
| guest | BIGINT |
| faq | BIGINT |
| down | BIGINT |
| warranty | BIGINT |
| finalize | BIGINT |
| register | BIGINT |
| shipping_info | BIGINT |
| checkout | BIGINT |
| mattresses | BIGINT |
| add_item | BIGINT |
| press | BIGINT |
| email_coupon | BIGINT |
| cc_info | BIGINT |
| foam | BIGINT |
| reviews | BIGINT |
| original | BIGINT |
| delivery | BIGINT |
| premium | BIGINT |
-- MAGIC
Uma lista dos nomes dos eventos é fornecida nas células TODO abaixo.

<hr>--i18n-0d995af9-e6f3-47b0-8b78-44bda953fa37
-- MAGIC
### Resolver com SQL

<hr>--i18n-afd696e4-049d-47e1-b266-60c7310b169a
-- MAGIC
### Resolver com Python

<hr>--i18n-2fe0f24b-2364-40a3-9656-c124d6515c4d
-- MAGIC
### Verifique seu trabalho
Execute a célula abaixo para confirmar se a view foi criada corretamente.

<hr>--i18n-eaac4506-501a-436a-b2f3-3788c689e841
-- MAGIC
-- MAGIC
-- MAGIC
## Unir contagens de eventos e transações para todos os usuários
-- MAGIC
A seguir, una **`events_pivot`** a **`transactions`** para criar a tabela **`clickpaths`**. Essa tabela deve ter as mesmas colunas de nome de evento da tabela **`events_pivot`** criada acima, seguidas pelas colunas da tabela **`transactions`**, conforme mostrado abaixo.
-- MAGIC
| campo | tipo | 
| --- | --- | 
| user | STRING |
| cart | BIGINT |
| ... | ... |
| user_id | STRING |
| order_id | BIGINT |
| transaction_timestamp | BIGINT |
| total_item_quantity | BIGINT |
| purchase_revenue_in_usd | DOUBLE |
| unique_items | BIGINT |
| P_FOAM_K | BIGINT |
| M_STAN_Q | BIGINT |
| P_FOAM_S | BIGINT |
| M_PREM_Q | BIGINT |
| M_STAN_F | BIGINT |
| M_STAN_T | BIGINT |
| M_PREM_K | BIGINT |
| M_PREM_F | BIGINT |
| M_STAN_K | BIGINT |
| M_PREM_T | BIGINT |
| P_DOWN_S | BIGINT |
| P_DOWN_K | BIGINT |

<hr>--i18n-03571117-301e-4c35-849a-784621656a83
-- MAGIC
### Resolva com SQL

<hr>--i18n-e0ad84c7-93f0-4448-9846-4b56ba71acf8
-- MAGIC
### Resolva com Python

<hr>--i18n-ac19c8e1-0ab9-4558-a0eb-a6c954e84167
-- MAGIC
### Verifique seu trabalho
Execute a célula abaixo para confirmar se a visualização foi criada corretamente.

<hr>--i18n-f352b51d-72ce-48d9-9944-a8f4c0a2a5ce
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

