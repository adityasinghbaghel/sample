# /DE 6 - Gerenciando permissões/DE 6.3L - Criar views e limitar o acesso a tabelas
<hr>--i18n-20e4c711-c890-4ef6-bc9b-328f8550ed08
# Criar views e limitar o acesso a tabelas
-- MAGIC
Neste caderno você aprenderá como:
* Criar views
* Gerenciar o acesso a views
* Usar recursos de views dinâmicas para restringir o acesso a colunas e linhas em uma tabela

<hr>--i18n-ede11fd8-42c7-4b13-863e-30eb9eee7fc3
## Configurar
-- MAGIC
Execute as células a seguir para realizar alguma configuração. Para evitar conflitos em um ambiente de treinamento compartilhado, essa ação criará um banco de dados com um nome exclusivo para seu uso. A ação também criará uma tabela de exemplo chamada **silver** no metastore do Unity Catalog.

<hr>--i18n-e7942e1d-097f-43a5-a1d7-d54af274b859
Observação: este notebook pressupõe que seu metastore do Unity Catalog tem um catálogo chamado *main*. Se precisar usar um catálogo diferente, edite o notebook **Configuração da sala de aula** antes de continuar.

<hr>--i18n-ef87e152-8e6a-4fd9-8cc7-579545aa01f9
Vamos examinar o conteúdo da tabela **silver**.
-- MAGIC
Observação: como a configuração selecionou o catálogo e o banco de dados default, só precisamos especificar os nomes de tabelas ou views, sem quaisquer níveis adicionais.

<hr>--i18n-f9c4630d-c04d-4ec3-8c20-8afea4ec7e37
## Criar a view ouro
-- MAGIC
Com uma tabela prata implementada, vamos criar uma view que agregue dados dessa tabela, apresentando dados adequados para a camada ouro de uma arquitetura medallion.

<hr>--i18n-2eb72987-961a-4020-b775-1e5e29c08a1a
Vamos examinar a view ouro.

<hr>--i18n-f5ccd808-ffbd-4e06-8ca2-c03dd80ad8e2
## Conceder acesso à view [optional]
-- MAGIC
Com uma nova view implementada, vamos permitir que os usuários no grupo **account users** a consultem.
-- MAGIC
Execute esta seção removendo o comentário das células de código e executando-as em sequência. Você também verá uma solicitação para executar algumas queries no Databricks SQL. Para fazer isso:
-- MAGIC
1. Abra uma nova tab e vá para o Databricks SQL.
1. Crie um warehouse SQL seguindo as instruções em* Criar SQL Warehouse no Catálogo Unity*.
1. Prepare-se para inserir consultas conforme as instruções abaixo nesse ambiente.

<hr>--i18n-9bf38439-6ad4-4db5-bb4f-fe70b8e4cfec
### Conceder o privilégio SELECT na view
-- MAGIC
O primeiro requisito é conceder o privilégio **SELECT** na view para o grupo **account users**.

<hr>--i18n-fbb44902-4662-4d9a-ab11-462a2b07a665
### Conceder o privilégio USAGE no catálogo e no banco de dados
-- MAGIC
Assim como acontece com as tabelas, o privilégio **USAGE** também é necessário no catálogo e no banco de dados para consultar a view.

<hr>--i18n-cfe19914-4f92-47da-a250-2d350a39736f
### Consultar a view como usuário
-- MAGIC
Com as concessões apropriadas implementadas, tente consultar a view no ambiente do Databricks SQL.
-- MAGIC
Execute a célula a seguir para gerar uma instrução de query que lê a view. Copie e cole a saída em uma nova query no ambiente SQL e execute-a.

<hr>--i18n-330f0448-6bef-42bc-930d-1716886c97fb
Observe que a query foi bem-sucedida e a saída é idêntica à saída acima, conforme esperado.
-- MAGIC
Agora substitua **`gold.heartrate_avgs`** por **`silver.heartrate_device`** e execute a query novamente. Observe que a query agora falha. Isso ocorre porque o usuário não tem o privilégio **SELECT** na tabela **`silver.heartrate_device`**.

<hr>--i18n-7c6916be-d36a-4655-b95c-94402222573f
-- MAGIC
Lembre-se, porém, de que **`heartrate_avgs`** é uma view que seleciona dados de **`heartrate_device`**. Sendo assim, como a query de **`heartrate_avgs`** pode ser bem-sucedida? O Unity Catalog permite que a query seja feita porque o *proprietário* dessa view tem o privilégio **SELECT** em **`silver.heartrate_device`**. Essa é uma propriedade importante, pois nos permite implementar views que podem filtrar ou mascarar linhas ou colunas de uma tabela, sem permitir acesso direto à tabela subjacente que estamos tentando proteger. Veremos esse mecanismo em ação a seguir.

<hr>--i18n-4ea26d47-eee0-43e9-867b-4b6913679e41
## Views dinâmicas
-- MAGIC
As views dinâmicas permitem configurar um controle fino de acesso, incluindo:
* Segurança no nível de colunas ou linhas.
* Mascaramento de dados.
-- MAGIC
O controle de acesso é obtido usando funções dentro da definição da view. Essas funções incluem:
* **`current_user()`**: retorna o endereço de email do usuário atual
* **`is_account_group_member()`**: retorna TRUE quando o usuário atual é membro do grupo especificado
-- MAGIC
Observação: para compatibilidade com componentes herdados, também existe a função **`is_member()`**, que retorna TRUE quando o usuário atual é membro do grupo no workspace especificado. Evite usar essa função ao implementar views dinâmicas no Unity Catalog.

<hr>--i18n-b39146b7-362b-46db-9a59-a8c589e94392
### Restringir colunas
Vamos aplicar **`is_account_group_member()`** para mascarar colunas contendo informações pessoais identificáveis para os membros do grupo **account users** usando instruções **`CASE`** dentro de **`SELECT`**.
-- MAGIC
Observação: este é um exemplo simples alinhado com a configuração deste ambiente de treinamento. Em um sistema de produção, o método preferível seria restringir linhas para usuários que *não* são membros de um grupo específico.

<hr>--i18n-c5c4afac-2736-4a57-be57-55535654a227
Agora vamos emitir a concessão novamente na view atualizada.

<hr>--i18n-271fb0d5-34f1-435f-967e-4cc650facd01
Vamos consultar a view, o que gerará uma saída não filtrada (com base no pressuposto de que o usuário atual não foi adicionado ao grupo **analysts**).

<hr>--i18n-1754f589-d48f-499d-b352-3fa735305eb9
Execute novamente a query executada anteriormente no ambiente do Databricks SQL (alterando **`silver`** de volta para **`gold_dailyavg`**). Observe que as informações pessoais identificáveis agora estão filtradas. Os membros deste grupo não podem ter acesso às informações pessoais identificáveis, pois elas estão protegidas pela view, e não há acesso direto à tabela subjacente.

<hr>--i18n-f7759fba-c2f8-4471-96f7-aa1ea5a6a00b
### Restringir linhas
Agora vamos aplicar **`is_account_group_member()`** para filtrar e excluir linhas. Neste caso, criaremos uma view ouro que retorna o carimbo de data e hora e o valor da frequência cardíaca, restritos para membros do grupo **analysts**, em linhas com ID de dispositivo menor que 30. A filtragem de linhas pode ser feita aplicando a instrução condicional como uma cláusula **`WHERE`** em **`SELECT`**.

<hr>--i18n-63255ec6-2317-455c-9665-e1c2069546f4
Execute novamente a query executada anteriormente no ambiente do Databricks SQL (alterando **`gold_dailyavg`** para **`gold_allhr`**). Observe que as linhas com ID de dispositivo 30 ou acima são omitidas da saída.

<hr>--i18n-d11e2467-cd67-411e-bedd-4ebe55301985
### Mascaramento de dados
O último caso de uso para views dinâmicas é o mascaramento de dados, ou seja, permitir a passagem de um subset de dados, mas transformá-lo de uma forma que não seja possível deduzir o valor total do campo mascarado.
-- MAGIC
Esse método combina a abordagem de filtragem de linhas e colunas para amplificar uma view com filtragem de linhas adicionando o mascaramento de dados. Neste passo, em vez de substituir a coluna inteira pela string **REDACTED**, usamos funções de manipulação de string SQL para exibir os dois últimos dígitos de **mrn**, mascarando o restante.
-- MAGIC
Dependendo de suas necessidades, o SQL fornece uma biblioteca bastante abrangente de funções de manipulação de strings que podem ser usadas para mascarar dados de várias maneiras diferentes. A abordagem mostrada abaixo traz um exemplo simples disso.

<hr>--i18n-c379505f-ebad-474d-9dad-92fa8a7a7ee9
Execute novamente a query em **gold_allhr** pela última vez no ambiente do Databricks SQL. Observe que, além de algumas linhas serem filtradas, a coluna **mrn** é mascarada de forma que apenas os dois últimos dígitos são exibidos. Isso fornece informações suficientes para associar os registros a pacientes conhecidos, sem revelar nenhuma informação pessoal identificável.

<hr>--i18n-8828d381-084f-43fa-afc3-a5a2ba170aeb
## Limpar
Execute a célula a seguir para remover os ativos usados neste exemplo.

