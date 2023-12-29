# /DE 3 - Delta Lake/DE 3.3 - Load Data into Delta Lake
<hr>--i18n-a518bafd-59bd-4b23-95ee-5de2344023e4
-- MAGIC
 
# Carregando dados no Delta Lake
As tabelas do Delta Lake fornecem atualizações compatíveis com ACID para tabelas com suporte de arquivos de dados no armazenamento de objetos em nuvem.
-- MAGIC
Neste notebook, exploraremos a sintaxe SQL para processar atualizações com o Delta Lake. Embora muitas operações sejam em SQL padrão, existem pequenas variações para acomodar a execução do Spark e do Delta Lake.
-- MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
- Substituir tabelas de dados usando **`INSERT OVERWRITE`**
- Acrescentar dados a uma tabela usando **`INSERT INTO`**
- Acrescentar, atualizar e excluir dados de uma tabela usando **`MERGE INTO`**
- Ingerir dados incrementalmente em tabelas usando **`COPY INTO`**

<hr>--i18n-af486892-a86c-4ef2-9996-2ace24b5737c
-- MAGIC
-- MAGIC
## Executar a configuração
-- MAGIC
O script de configuração criará os dados e declarará os valores necessários para a execução do restante deste notebook.

<hr>--i18n-04a35896-fb09-4a99-8d00-313480e5c6a1
-- MAGIC
-- MAGIC
 
## Substituições completas
-- MAGIC
Podemos substituir atomicamente todos os dados de uma tabela. Substituir uma tabela, em vez de excluí-la e recriá-la, tem vários benefícios:
- Substituir uma tabela é muito mais rápido porque não é necessário listar o diretório recursivamente ou excluir nenhum arquivo.
- A versão antiga da tabela ainda existe, e os dados antigos podem ser recuperados facilmente usando a viagem do tempo.
- A operação é atômica. Queries concorrentes ainda podem ler a tabela enquanto ela está sendo excluída.
- Devido às garantias da transação ACID, se a substituição falhar, a tabela permanecerá no estado anterior.
-- MAGIC
O Spark SQL fornece dois métodos fáceis para realizar substituições completas.
-- MAGIC
Alguns alunos podem ter notado que a lição anterior sobre instruções CTAS na verdade usou instruções CRAS (para evitar possíveis erros se uma célula fosse executada várias vezes).
-- MAGIC
As instruções **`CREATE OR REPLACE TABLE`** (CRAS) substituem totalmente o conteúdo de uma tabela sempre que são executadas.

<hr>--i18n-8f767697-33e6-4b5b-ac09-862076f77033
-- MAGIC
-- MAGIC
A revisão da história da tabela mostra que uma versão anterior da tabela foi substituída.

<hr>--i18n-bb68d513-240c-41e1-902c-3c3add9c0a75
-- MAGIC
-- MAGIC
**`INSERT OVERWRITE`** fornece um resultado quase idêntico ao acima: os dados na tabela de destino são substituídos pelos dados da query. 
-- MAGIC
**`INSERT OVERWRITE`**:
-- MAGIC
- Só pode substituir uma tabela existente, mas não pode criar uma tabela como a instrução CRAS
- Só pode substituir por novos registros que correspondam ao esquema da tabela atual e, portanto, pode ser uma técnica mais segura para substituir uma tabela existente sem interromper consumidores posteriores
- Pode substituir partições individuais

<hr>--i18n-cfefb85f-f762-43db-be9b-cb536a06c842
-- MAGIC
-- MAGIC
Observe que as métricas exibidas são diferentes de uma instrução CRAS. A história da tabela também registra a operação de forma diferente.

<hr>--i18n-40769b04-c72b-4740-9d27-ea2d1b8700f3
-- MAGIC
-- MAGIC
A principal diferença tem a ver com a forma como o Delta Lake impõe o esquema na gravação.
-- MAGIC
Enquanto uma instrução CRAS permite redefinir completamente o conteúdo da tabela de destino, **`INSERT OVERWRITE`** falhará se tentarmos alterar o esquema (a menos que sejam fornecidas configurações opcionais). 
-- MAGIC
Remova o comentário e execute a célula abaixo para gerar uma mensagem de erro esperada.

<hr>--i18n-ceb78e46-6362-4c3b-b63d-54f42d38dd1f
-- MAGIC
-- MAGIC
-- MAGIC
## Acrescentar linhas
-- MAGIC
Podemos usar **`INSERT INTO`** para acrescentar atomicamente novas linhas a uma tabela Delta existente. Essa ação permite fazer atualizações incrementais em tabelas existentes, o que é muito mais eficiente do que substituições repetidas.
-- MAGIC
Acrescente novos registros de venda à tabela **`sales`** usando **`INSERT INTO`**.

<hr>--i18n-171f9cf2-e0e5-4f8d-9dc7-bf4770b6d8e5
-- MAGIC
-- MAGIC
-- MAGIC
Observe que **`INSERT INTO`** não tem nenhuma garantia integrada para evitar a inserção dos mesmos registros várias vezes. Reexecutar a célula acima gravaria os mesmos registros na tabela de destino, resultando em registros duplicados.

<hr>--i18n-5ad4ab1f-a7c1-439d-852e-ff504dd16307
-- MAGIC
-- MAGIC
-- MAGIC
## Mesclar atualizações
-- MAGIC
Você pode atualizar ou inserir dados de uma tabela, view ou DataFrame de origem em uma tabela Delta de destino usando a operação SQL **`MERGE`**. O Delta Lake oferece suporte a inserções, atualizações e exclusões em **`MERGE`** e oferece suporte à sintaxe estendida além dos padrões SQL para facilitar casos de uso avançados.
-- MAGIC
<strong><code>
MERGE INTO target a<br/>
USING source b<br/>
ON {merge_condition}<br/>
WHEN MATCHED THEN {matched_action}<br/>
WHEN NOT MATCHED THEN {not_matched_action}<br/>
</code></strong>
-- MAGIC
Usaremos a operação **`MERGE`** para atualizar dados históricos de usuários com emails atualizados e novos usuários.

<hr>--i18n-4732ea19-2857-45fe-9ca2-c2475015ef47
-- MAGIC
 
Principais benefícios de **`MERGE`**:
* As atualizações, inserções e exclusões são concluídas como uma única transação
* Além dos campos correspondentes, é possível adicionar condicionais
* Há inúmeras opções para implementar lógica personalizada
-- MAGIC
No passo abaixo, só atualizaremos os registros se o email for **`NULL`** na linha atual, mas não na nova linha. 
-- MAGIC
Todos os registros sem correspondência do novo lote serão inseridos.

<hr>--i18n-5cae1734-7eaf-4a53-a9b5-c093a8d73cc9
-- MAGIC
-- MAGIC
Observe que especificamos explicitamente o comportamento da função em ambas as condições, **`MATCHED`** e **`NOT MATCHED`**. O exemplo mostrado aqui é apenas uma maneira de aplicar a lógica, e não um indicativo de todo o comportamento de **`MERGE`**.

<hr>--i18n-d7d2c7fd-2c83-4ed2-aa78-c37992751881
-- MAGIC
-- MAGIC
## Merge somente de inserção para desduplicação
-- MAGIC
Um caso de uso comum de ETL é coletar logs ou outros datasets de inserção contínua em uma tabela Delta usando uma série de operações de inserção. 
-- MAGIC
Muitos sistemas de origem podem gerar registros duplicados. Com a operação de merge, você pode evitar inserir registros duplicados executando uma merge somente de inserção.
-- MAGIC
Este comando otimizado usa a mesma sintaxe **`MERGE`**, mas só forneceu uma cláusula **`WHEN NOT MATCHED`**.
-- MAGIC
No passo abaixo, usamos isso para confirmar que os registros com o mesmo **`user_id`** e **`event_timestamp`** ainda não estão na tabela **`events`**.

<hr>--i18n-75891a95-c6f2-4f00-b30e-3df2df858c7c
-- MAGIC
 
## Carregar incrementalmente
-- MAGIC
**`COPY INTO`** fornece aos engenheiros de SQL uma opção idempotente para ingerir dados de forma incremental de sistemas externos.
-- MAGIC
Observe que essa operação tem algumas expectativas:
- O esquema de dados deve ser consistente
- Deve-se tentar excluir ou lidar com registros duplicados em etapas posteriores
-- MAGIC
Esta operação é potencialmente muito mais econômica do que varreduras completas de tabelas em busca de dados que crescem de forma previsível.
-- MAGIC
Aqui mostramos uma execução simples em um diretório estático, mas o valor real desse método está em múltiplas execuções ao longo do tempo, em que novos arquivos são selecionados automaticamente na fonte.

<hr>--i18n-fd65fe71-cdaf-47a8-85ec-fa9769c11708
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

