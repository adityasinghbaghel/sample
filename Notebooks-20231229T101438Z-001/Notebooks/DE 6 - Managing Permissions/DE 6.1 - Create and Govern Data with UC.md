# /DE 6 - Gerenciando permissões/DE 6.1 - Criar e governar dados com o UC
<hr>--i18n-b5d0b5d6-287e-4dcd-8a4b-a0fe2b12b615
# Criar e governar objetos de dados com o Unity Catalog
-- MAGIC
Neste notebook você aprenderá a:
* Criar catálogos, esquemas, tabelas, views e funções definidas pelo usuário
* Controlar o acesso a esses objetos
* Usar views dinâmicas para proteger colunas e linhas em tabelas
* Explorar concessões em vários objetos no Unity Catalog

<hr>--i18n-63c92b27-28a0-41f8-8761-a9ec5bb574e0
## Pré-requisitos
-- MAGIC
Se quiser acompanhar este laboratório, você precisa:
* Ter permissões de administrador do metastore para criar e gerenciar um catálogo
* Ter um SQL warehouse que possa ser acessado pelo usuário mencionado acima
  * Consulte o notebook: Criando recursos de compute para acesso ao Unity Catalog

<hr>--i18n-67495bbd-0f5b-4f81-9974-d5c6360fb86c
## Configuração
-- MAGIC
Execute a célula a seguir para realizar a configuração. Para evitar conflitos em um ambiente de treinamento compartilhado, essa ação gerará um nome de catálogo exclusivo para seu uso, que utilizaremos em breve.

<hr>--i18n-a0720484-97de-4742-9c38-6c5bd312f3d7
## Namespace de três níveis do Unity Catalog
-- MAGIC
Qualquer pessoa com experiência em SQL provavelmente estará familiarizada com o tradicional namespace de dois níveis usado em tabelas ou views dentro de um esquema, conforme mostrado no exemplo de query a seguir:
-- MAGIC
    SELECT * FROM myschema.mytable;
-- MAGIC
O Unity Catalog introduz o conceito de um **catálogo** na hierarquia. Como se fosse um contêiner para esquemas, o catálogo oferece uma nova opção de segregação de dados para as organizações. Você pode ter quantos catálogos quiser, que, por sua vez, podem conter quantos esquemas desejar (o conceito de um **esquema** permanece inalterado no Unity Catalog: esquemas contêm objetos de dados como tabelas, views e funções definidas pelo usuário).
-- MAGIC
Para lidar com esse nível adicional, as referências completas de tabela/view no Unity Catalog usam namespaces de três níveis. A query a seguir exemplifica isso:
-- MAGIC
    SELECT * FROM mycatalog.myschema.mytable;
-- MAGIC
Isso pode ser útil em muitos casos de uso. Por exemplo:
-- MAGIC
* Separar dados relacionados às unidades de negócios da organização (vendas, marketing, recursos humanos etc.)
* Satisfazer os requisitos do ciclo de desenvolvimento do software (desenvolvimento, teste, produção etc.)
* Estabelecer sandboxes contendo datasets temporários para uso interno

<hr>--i18n-aa4c68dc-6dc3-4aca-a352-affc98ac8089
### Criar um novo catálogo
Vamos criar um catálogo no metastore. A variável **`${DA.my_new_catalog}`**, exibida pela célula de configuração acima, contém uma string exclusiva gerada com base no seu nome de usuário.
-- MAGIC
Execute a instrução **`CREATE`** abaixo e clique no ícone **Dados** na barra lateral esquerda para confirmar que o novo catálogo foi criado.

<hr>--i18n-e1f478c8-bbf2-4368-9cdd-e130d2fb7410
### Selecionar um catálogo default
-- MAGIC
Os desenvolvedores de SQL provavelmente também estarão familiarizados com a instrução **`USE`** para selecionar um esquema default. Esse método encurta as queries, já que não é necessário especificar sempre o esquema. Para ampliar essa conveniência ao lidar com o nível extra no namespace, o Unity Catalog expande a linguagem com duas instruções adicionais, mostradas nos exemplos abaixo:
-- MAGIC
    USE CATALOG mycatalog;
    USE SCHEMA myschema;  
    
Vamos selecionar o catálogo recém-criado como o default. Agora, qualquer referência a esquema será considerada como estando presente nesse catálogo, a menos que seja explicitamente substituída por uma referência de catálogo.

<hr>--i18n-bc4ad8ed-6550-4457-92f7-d88d22709b3c
### Criar e usar um esquema
A seguir, vamos criar um esquema neste novo catálogo. Não precisaremos gerar outro nome exclusivo para esse esquema, pois agora estamos usando um catálogo exclusivo isolado do restante do metastore. Vamos também definir esse esquema como o default. Agora, qualquer referência de dados será considerada como estando presente no catálogo e no esquema que criamos, a menos que seja explicitamente substituída por uma referência de dois ou três níveis.
-- MAGIC
Execute o código abaixo e clique no ícone **Dados** na barra lateral esquerda para confirmar que o esquema foi criado no catálogo que criamos.

<hr>--i18n-87b6328c-4641-4d40-b66c-f166a4166902
### Configurar tabelas e views
-- MAGIC
Com todos os níveis hierárquicos necessários criados, vamos configurar tabelas e views. Neste exemplo, usaremos dados simulados para criar e preencher uma tabela *silver* gerenciada com dados sintéticos de frequência cardíaca de pacientes e uma view *gold* que calcula diariamente a média dos dados de frequência cardíaca por paciente.
-- MAGIC
Execute as células abaixo e clique no ícone **Dados** na barra lateral esquerda para explorar o conteúdo do esquema de *exemplo*. Observe que não precisamos especificar três níveis nos nomes da tabela ou da view abaixo, pois selecionamos um catálogo e um esquema default.

<hr>--i18n-3ab35bc3-e89b-48d0-bcef-91a268688a19
Consultar a tabela acima funciona conforme o esperado, pois somos proprietários dos dados. Ou seja, somos proprietários do objeto de dados sendo consultado. Consultar a view também funciona porque somos os proprietários da view e da tabela à qual ela faz referência. Assim, nenhuma permissão no nível de objeto é necessária para acessar esses recursos.

<hr>--i18n-9ce42a62-c95a-45fb-9d6b-a4d3725110e7
## O grupo _account users_
-- MAGIC
Accounts com o Unity Catalog habilitado têm um grupo _account users_. Esse grupo contém todos os usuários que foram atribuídos ao workspace a partir da account do Databricks. Usaremos esse grupo para mostrar como o acesso a objetos de dados pode ser diferente para usuários em grupos diferentes.

<hr>--i18n-29eeb0ee-94e0-4b95-95be-a8776d20dc6c
-- MAGIC
## Conceder acesso a objetos de dados
-- MAGIC
O Unity Catalog emprega um modelo de permissão explícita por default, e nenhuma permissão é implícita ou herdada dos elementos contendo os dados. Portanto, para acessar qualquer objeto de dados, o usuário precisa da permissão **USAGE** a todos os elementos que o contêm, ou seja, o esquema e o catálogo onde o objeto reside.
-- MAGIC
Neste passo, vamos permitir que o grupo *account users* consulte a view *ouro*. Para isso, precisamos conceder as seguintes permissões:
1. USAGE no catálogo e no esquema
1. SELECT no objeto de dados (por exemplo, view)

<hr>--i18n-dabadee1-5624-4330-855d-d0c0de1f76b4
### Consultar a view
-- MAGIC
Com a hierarquia de objetos de dados e todas as concessões apropriadas implementadas, vamos executar uma query na view *ouro*.
-- MAGIC
Como todos nós somos membros do grupo **account users**, podemos usá-lo para verificar a configuração e observar o impacto de alterações feitas.
-- MAGIC
1. Na canto superior esquerdo, clique no alternador de aplicativos para abri-lo.
1. Clique com o botão direito em **SQL** e selecione **Abrir link em uma nova tab**.
1. Vá para a página **Queries** e clique em **Criar query**.
1. Selecione o SQL warehouse compartilhado que foi criado na demonstração *Criando recursos de compute para acesso ao Unity Catalog*.
1. Volte a esse notebook e continue acompanhando as instruções. Quando houver uma solicitação, passaremos à sessão do Databricks SQL e executaremos queries.
-- MAGIC
A célula a seguir gera uma instrução de query totalmente qualificada que especifica todos os três níveis da view, já que a executaremos em um ambiente sem variáveis e sem uma configuração default de catálogo e esquema. Execute a query gerada abaixo na sessão do Databricks SQL. Como todas as concessões apropriadas foram implementadas para dar os usuários da account acesso à view, o resultado deve ser semelhante ao que vimos anteriormente ao consultar a view *ouro*.

<hr>--i18n-7fb51344-7eb4-49a2-916c-6cdee06e534a
### Consultar a tabela prata
De volta à mesma query da sessão do Databricks SQL, vamos substituir *gold* por *silver* e executar a query. Como somos proprietários dos dados, essa execução deve retornar todos os resultados da tabela conforme esperado. No entanto, se for executada por outro usuário, a query falhará porque não configuramos permissões na tabela *silver*. 
-- MAGIC
A query *ouro* funciona para todos os membros do grupo _account users_ porque a query representada por uma view é executada essencialmente como o proprietário da view. Essa propriedade importante permite alguns casos de uso de segurança interessantes. Com ela, as views podem fornecer aos usuários uma visualização restrita de dados confidenciais, sem fornecer acesso aos dados subjacentes em si. Falaremos mais sobre isso em breve.
-- MAGIC
Por enquanto, você pode fechar e descartar a query *silver* na sessão do Databricks SQL, que não será mais usada.

<hr>--i18n-bea5cb66-3642-45b1-8906-906588b99b06
### Criar e conceder acesso a uma função definida pelo usuário
-- MAGIC
O Unity Catalog também é capaz de gerenciar funções definidas pelo usuário dentro de esquemas. O código abaixo configura e testa uma função simples que mascara todos os caracteres de uma string, exceto os dois últimos. Mais uma vez, somos os proprietários dos dados, portanto, nenhuma concessão é necessária.

<hr>--i18n-d0945f12-4045-471b-a319-376f5f8f25dd
-- MAGIC
Para ter permissão de executar a função, os membros do grupo *account users* precisam ter **EXECUTE** na função e as concessões necessárias **USAGE** no esquema e no catálogo, como mencionamos antes.

<hr>--i18n-e74e14a8-d372-44e0-a301-94cc046efd29
### Executar uma função
-- MAGIC
Agora vamos experimentar a função no Databricks SQL. Cole a instrução de query totalmente qualificada gerada abaixo em uma nova query para executar a função no Databricks SQL. Como todas as concessões apropriadas de acesso à função estão implementadas, a saída deve ser semelhante ao que acabamos de ver acima.

<hr>--i18n-8697f10a-6924-4bac-9c9a-7d91285eb9f5
## Proteger colunas e linhas da tabela com views dinâmicas
-- MAGIC
Vimos que o Unity Catalog permite usar views para proteger o acesso às tabelas. É possível conceder aos usuários acesso a views que manipulam, transformam ou ocultam dados de uma tabela de origem, sem precisar fornecer acesso direito à tabela de origem.
-- MAGIC
As views dinâmicas permitem o controle fino de acesso a colunas e linhas em uma tabela, condicionado à entidade que executa a query. As views dinâmicas são uma extensão das views padrão que permitem ações como:
* Ocultar parcial ou completamente os valores de colunas
* Omitir linhas com base em critérios específicos
-- MAGIC
O controle de acesso com views dinâmicas é obtido usando funções dentro da definição da view. Essas funções incluem:
* **`current_user()`**: retorna o endereço de email do usuário que consulta a view
* **`is_account_group_member()`**: retorna TRUE quando o usuário que consulta a view é membro do grupo especificado
-- MAGIC
Observação: evite usar a função herdada **`is_member()`**, que faz referência aos grupos no nível do workspace. Essa não é uma prática recomendada no Unity Catalog.

<hr>--i18n-8fc52e53-927e-4d6b-a340-7082c94d4e6e
### Ocultar colunas
-- MAGIC
Vamos supor que queremos que os usuários da account possam ver as tendências de dados agregados da view *ouro*, mas não queremos revelar informações pessoais identificáveis (PII) dos pacientes. Vamos redefinir a view para ocultar as colunas *mrn* e *name* usando **`is_account_group_member()`**.
-- MAGIC
Observação: esse é um exemplo simples de treinamento que não está necessariamente alinhado com as práticas recomendadas gerais. Em um sistema de produção, uma abordagem mais segura seria ocultar os valores das colunas para todos os usuários que *não* são membros de um grupo específico.
-- MAGIC

<hr>--i18n-01381247-0c36-455b-b64c-22df863d9926
-- MAGIC
Faça a concessão novamente.

<hr>--i18n-0bf9bd34-2351-492c-b6ef-e48241339d0f
-- MAGIC
Volte ao Databricks SQL e execute novamente a query na view *ouro*. Execute a célula abaixo para gerar essa query. 
-- MAGIC
Vemos que os valores das colunas *mrn* e *name* foram ocultados.

<hr>--i18n-0bb3c639-daf8-4b46-9c28-cafd32a12917
### Restringir linhas
-- MAGIC
Agora, vamos supor que queremos uma view que, em vez de agregar e ocultar colunas, simplesmente filtre as linhas da origem e as exclua. Vamos aplicar a mesma função **`is_account_group_member()`** para criar uma view que transmite apenas linhas com *device_id* menor que 30. A filtragem de linha é feita aplicando a condicional como uma cláusula **`WHERE`**.

<hr>--i18n-69bc283c-f426-4ba2-b296-346c69de1c20
-- MAGIC
Faça a concessão- novamente.

<hr>--i18n-c4f3366a-4992-4d5a-b597-e140091a8d00
-- MAGIC
Para qualquer usuário que não faça parte do grupo, a consulta da view acima exibe todos os cinco registros. Agora, revisite o Databricks SQL e execute novamente a consulta no* ouro* visualizar. Vemos que um dos registros está faltando. O registro ausente continha um valor em *device_id* que foi capturado pelo filtro.

<hr>--i18n-dffccf13-5205-44d5-beab-4d08b085f54a
### Mascaramento de dados
O último caso de uso para views dinâmicas é o mascaramento de dados ou obscurecimento parcial dos dados. No primeiro exemplo, ocultamos inteiramente as colunas. Em princípio, o mascaramento é semelhante, exceto que exibimos alguns dados em vez de substituí-los inteiramente. Neste exemplo simples, usaremos a função *mask()* definida pelo usuário que criamos anteriormente para mascarar a coluna *mrn*, embora o SQL forneça uma biblioteca bastante abrangente de funções integradas de manipulação de dados que podem ser utilizadas para mascarar dados de diversas maneiras diferentes. A prática recomendada é aproveitar as funções integradas quando possível.

<hr>--i18n-735fa71c-0d31-4484-9736-30dc098dee8d
-- MAGIC
Faça a concessão novamente-.

<hr>--i18n-9c3d9b8f-17ce-498f-b6dd-dfb470855086
-- MAGIC
Para qualquer usuário que não faça parte do grupo, a consulta exibe registros intactos. Volte ao Databricks SQL e execute novamente a query na view *ouro*. Todos os valores na coluna *mrn* serão mascarados.

<hr>--i18n-3e809063-ad1b-4a2e-8cc3-b87492c8ffc3
-- MAGIC
## Explorar objetos
-- MAGIC
Vamos explorar algumas instruções SQL para examinar os objetos de dados e as permissões. Para começar, vamos analisar os objetos que temos no esquema *examples*.

<hr>--i18n-40599e0f-47f9-46da-be2b-7b856da0cba1
Nas duas instruções acima, não especificamos um esquema, pois contamos com os esquemas default que selecionamos. Também poderíamos ter sido mais explícitos usando uma instrução como **`SHOW TABLES IN example`**.
-- MAGIC
Agora vamos subir um nível na hierarquia e fazer um inventário dos esquemas no catálogo. Mais uma vez, estamos aproveitando o fato de termos um catálogo default selecionado. Se quisermos ser mais explícitos, podemos usar algo como **`SHOW SCHEMAS IN ${DA.my_new_catalog}`**.

<hr>--i18n-943178c2-9ab3-4941-ac1a-70b63103ecb7
O esquema *example* é aquele que criamos anteriormente. O esquema *default* é criado por default segundo as convenções SQL quando um novo catálogo é criado.
-- MAGIC
Por fim, vamos listar os catálogos no metastore.

<hr>--i18n-e8579147-7d9a-4b27-b0e9-3ab6c4ec9a0c
Pode haver mais entradas do que você esperava. No mínimo, você verá:
* Um catálogo começando com o prefixo *dbacademy_*, que é aquele que criamos anteriormente.
* *hive_metastore*, que não é um catálogo real no metastore, mas sim uma representação virtual do Hive metastore local do workspace. Use-o para acessar as tabelas e views locais do workspace.
* *main*, um catálogo criado por default com cada novo metastore.
* *samples*, outro catálogo virtual que apresenta exemplos de datasets fornecidos pelo Databricks
-- MAGIC
Pode haver mais catálogos presentes dependendo da atividade histórica no seu metastore.

<hr>--i18n-9477b0a2-3099-4fca-bb7d-f6e298ce254b
### Explorar permissões
-- MAGIC
Agora vamos explorar as permissões usando **`SHOW GRANTS`**, começando pela view *ouro* e subindo nos níveis.

<hr>--i18n-98d1534e-a51c-45f6-83d0-b99549ccc279
No momento, só existe a concessão **SELECT** que acabamos de configurar. Agora vamos verificar as concessões em *silver*.

<hr>--i18n-591b3fbb-7ed3-4e88-b435-3750b212521d
No momento, não há concessões nessa tabela. Somente nós, que somos proprietários dos dados, podemos acessar essa tabela diretamente. Qualquer pessoa com permissão para acessar a view *ouro*, da qual também somos proprietários, pode acessar essa tabela indiretamente.
-- MAGIC
Agora vamos conferir o esquema contendo os dados.

<hr>--i18n-ab78d60b-a596-4a19-80ae-a5d742169b6c
No momento, vemos a concessão **USAGE** que configuramos anteriormente.
-- MAGIC
Agora vamos examinar o catálogo.

<hr>--i18n-62f7e069-7260-4a48-9676-16088958cffc
Da mesma forma, vemos **USAGE**, que concedemos momentos atrás.

<hr>--i18n-200fe251-2176-46ca-8ecc-e725d8c9da01
## Revogar acesso
-- MAGIC
Nenhuma plataforma de governança de dados estaria completa sem a capacidade de revogar concessões emitidas anteriormente. Vamos começar examinando o acesso à função *mascarar()*.

<hr>--i18n-9495b822-96bd-4fe5-aed7-9796ffd722d0
Agora vamos revogar essa concessão.

<hr>--i18n-c599d523-08cc-4d39-994d-ce919799c276
Vamos examinar o acesso novamente, que agora estará vazio.

<hr>--i18n-a47d66a4-aa65-470b-b8ea-d8e7c29fce95
-- MAGIC
Volte à sessão do Databricks SQL e execute novamente a query na view *ouro*. Observe que essa ação ainda funciona como antes. Isso é uma surpresa para você? Por que ou por que não?
-- MAGIC
Lembre-se de que a view está sendo executada efetivamente por seu proprietário, que também é o proprietário da função e da tabela de origem. Assim como o exemplo de view anterior não exigia acesso direto à tabela sendo consultada, já que o proprietário da view é o proprietário da tabela, a função é bem-sucedida pelo mesmo motivo.
-- MAGIC
Agora vamos tentar algo diferente. Vamos quebrar a cadeia de permissões revogando **USAGE** no catálogo.

<hr>--i18n-b1483973-1ef7-4b6d-9a04-931c53947148
-- MAGIC
De volta ao Databricks SQL, execute novamente a query *ouro* e observe que, embora tenhamos as permissões adequadas na view e no esquema, o privilégio ausente no topo da hierarquia interromperá o acesso a esse recurso. Isso ilustra o modelo de permissão explícita do Unity Catalog em ação: nenhuma permissão é implícita ou herdada.

<hr>--i18n-1ffb00ac-7663-4206-84b6-448b50c0efe2
## Limpar
Vamos executar a célula a seguir para remover o catálogo que criamos anteriormente. O qualificador **`CASCADE`** removerá o catálogo e todos os elementos contidos nele.

