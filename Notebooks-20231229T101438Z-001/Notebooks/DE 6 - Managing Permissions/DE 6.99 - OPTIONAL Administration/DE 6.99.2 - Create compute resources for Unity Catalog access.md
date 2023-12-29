# /DE 6 - Managing Permissions/DE 6.99 - OPTIONAL Administration/DE 6.99.2 - Create compute resources for Unity Catalog access
<hr>--i18n-c5b7f955-3176-4a14-be70-f6d4d9e0e980
# Criar cluster para acesso ao Unity Catalog
-- MAGIC
Nesta demonstração, você aprenderá a:
* Configurar um cluster para acesso ao Unity Catalog
* Configurar um SQL warehouse para acesso ao Unity Catalog
* Usar o Data Explorer para navegar no namespace de três níveis do Unity Catalog

<hr>--i18n-40ccd6a9-9aff-4ffc-a7bc-d76dda043c08
## Visão geral
-- MAGIC
Antes do Unity Catalog, uma solução completa de governança de dados exigia uma configuração cuidadosa das configurações do workspace, listas de controle de acesso, além de configurações e políticas de cluster. Se não fosse configurado corretamente, esse sistema cooperativo poderia permitir que os usuários contornassem todo o controle de acesso.
-- MAGIC
Embora o Unity Catalog introduza algumas novas configurações, o sistema não requer nenhuma configuração específica para ser seguro. Sem configurações adequadas, os clusters não conseguem acessar nenhum dado seguro, tornando o Unity Catalog seguro por default. Essa característica e os recursos aprimorados de auditoria e controle fino de acesso tornam o Unity Catalog uma solução de governança de dados notavelmente evoluída.

<hr>--i18n-7cd4e2cb-6aac-4e8b-9429-e1dfbd04b23a
## Pré-requisitos
-- MAGIC
Para acompanhar esta demonstração, você precisará do direito **Permitir a criação irrestrita de clusters**. Peça ajuda ao administrador do workspace se não puder criar clusters ou SQL warehouses.

<hr>--i18n-5050600a-65f6-415e-a2ea-9d466ad259a7
## Workspace de ciência de dados e engenharia
-- MAGIC
Além dos SQL warehouses, os clusters são a porta de entrada para seus dados, pois são os elementos responsáveis pela execução do código nos notebooks. Nesta seção, veremos como configurar um cluster all-purpose para acessar o Unity Catalog. Os mesmos princípios podem ser aplicados à configuração de clusters de jobs para executar cargas de trabalho automatizadas.

<hr>--i18n-4f4c9e5e-4056-454f-92ed-183ffd745f67
### Criando um cluster
-- MAGIC
Vamos criar um cluster all-purpose capaz de acessar o Unity Catalog. Esse procedimento é familiar aos usuários atuais do Databricks, mas também vamos apresentar e explorar algumas configurações novas.
-- MAGIC
1. No workspace de ciência de dados e engenharia, clique em **Compute** na barra lateral esquerda.
1. Clique em **Criar cluster**.
1. Especifique um nome para o cluster. O nome deve ser único no workspace.
1. Agora vamos dar uma olhada na configuração **Modo cluster**. Observe que a opção *High Concurrency*, que os usuários do Databricks podem ter usado no passado, foi preterida no Unity Catalog. Um dos principais casos de uso dessa opção estava relacionado ao controle de acesso da tabela antes do Unity Catalog, mas um novo parâmetro chamado **Modo de segurança** foi introduzido para lidar com isso. Assim, temos as opções *Standard* e *Single node*, que só será usada em uma necessidade específica de um cluster de nó único. Por isso, deixaremos o modo cluster definido como *Padrão*.
1. Agora vamos escolher uma **Versão do Databricks Runtime** que seja a 11.1 ou superior.
1. Para reduzir o custo de computação desta demonstração, desabilitarei o dimensionamento automático e usarei menos workers, embora isso não seja necessário.
1. Agora precisamos configurar o **Modo de segurança**, que pode ser acessado nas **Opções avançadas**. Há cinco opções disponíveis, mas três delas não são compatíveis com o Unity Catalog. As duas opções que precisamos considerar são:
    * Os clusters *User isolation* podem ser compartilhados por vários usuários, mas sua funcionalidade geral é reduzida para promover um ambiente seguro e isolado. Só podemos usar Python e SQL, e alguns recursos avançados de cluster, como instalação de bibliotecas, init scripts e montagens do DBFS Fuse, também não estão disponíveis.
    * Os clusters *Single user* são compatíveis com todos os recursos e linguagens, mas o cluster é de uso exclusivo de um único usuário (por default, o proprietário do cluster). Essa é a escolha recomendada para clusters de jobs e clusters interativos, se você precisar de algum dos recursos não oferecidos no modo *User isolation*.
   
   Vamos escolher *Single user*. Observe que quando escolhemos esse modo, um novo campo chamado **Acesso de usuário único** aparece.
1. A configuração **Acesso de usuário único** nos permite designar quem pode se anexar a esse cluster. Essa configuração é independente e não está relacionada aos controles de acesso ao cluster, com os quais os usuários atuais do Databricks podem estar familiarizados. Essa configuração é aplicada no cluster e somente o usuário designado pode se anexar a ele; todos os outros serão rejeitados. Por default, essa configuração é preenchida com o usuário criando o cluster, e a deixaremos como está. Considere alterar essa configuração quando uma das seguintes situações ocorrer:
    * Você está criando um cluster all-purpose para outra pessoa
    * Você está configurando um cluster de jobs e, nesse caso, a configuração deve indicar a entidade de serviço que executará o job
1. Vamos clicar em **Criar cluster**. Essa operação leva alguns minutos, pois precisamos aguardar que o cluster seja iniciado.

<hr>--i18n-eead080e-8ccd-402b-b1fc-46abd4b0221e
### Navegando nos dados
-- MAGIC
Vamos aproveitar para explorar a página **Dados** atualizada no workspace de ciência de dados e engenharia.
-- MAGIC
1. Clique em **Dados** na barra lateral esquerda. Observe que agora há três colunas, enquanto que antes eram apenas duas:
    * **Catálogos** permite selecionar um catálogo, o contêiner de nível superior na hierarquia de objetos de dados do Unity Catalog e a primeira parte do namespace de três níveis.
    * **Bancos de dados**, também chamados de esquemas, permite selecionar um esquema no catálogo selecionado. Essa é a segunda parte do namespace de três níveis (ou a primeira parte do namespace tradicional de dois níveis com o qual a maioria está familiarizada).
    * **Tabelas** permite selecionar uma tabela para explorar. A partir daqui, podemos visualizar os metadados e a história de uma tabela, além de obter uma fotografia de dados de amostra.
1. Vamos explorar a hierarquia. Os catálogos, esquemas e tabelas que você pode selecionar dependem da quantidade de dados no metastore e das permissões.
1. O catálogo rotulado *main* é criado por default como parte da criação do metastore (semelhante a um esquema *default*)
1. O catálogo rotulado *hive_metastore* corresponde ao Hive metastore local do workspace.

<hr>--i18n-72996dbb-5c79-4968-87ff-83e9022365e7
### Explorar dados usando um notebook
-- MAGIC
Vamos fazer uma verificação rápida usando um notebook.
-- MAGIC
1. Vamos criar um notebook SQL no workspace para executar um breve teste. Vamos anexar o notebook ao cluster que criamos.
1. Crie uma célula com a query `SHOW GRANTS ON SCHEMA main.default` e execute-a.

<hr>--i18n-6709cfe7-6a4f-48e5-b990-31e9303a87fc
## Databricks SQL
-- MAGIC
Os SQL warehouses são o outro meio principal de acesso aos dados, pois são os recursos de compute responsáveis pela execução das queries do Databricks SQL. Se quiser habilitar a conectividade com ferramentas externas de business intelligence (BI), você também precisará canalizá-las por um SQL warehouse.

<hr>--i18n-88589f0f-720d-4b9c-8e16-23554ccc4b71
### Criando um SQL warehouse
-- MAGIC
Como os SQL warehouses são configurações de cluster criadas para um propósito específico, a configuração do Unity Catalog é ainda mais fácil, como veremos aqui.
-- MAGIC
1. Mude para a persona **SQL**.
1. Clique em **SQL Warehouses** na barra lateral esquerda.
1. Clique em **Criar SQL Warehouse**.
1. Vamos especificar um nome para o warehouse. Deve ser exclusivo no espaço de trabalho.
1. Para reduzir o custo deste ambiente de laboratório, escolherei um cluster *2X-Small* e definirei a escala máxima como 1.
1. Em **Opções avançadas**, vamos confirmar que o **Unity Catalog** está habilitado.
1. Depois de concluir a configuração, clique em **Criar**.
1. Vamos tornar o cluster acessível a todos no workspace. Na **caixa de diálogo Gerenciar permissões**, vamos adicionar uma nova permissão:
    1. Clique no campo de pesquisa e selecione *Todos os usuários*.
    1. Deixe a permissão definida como *Pode usar* e clique em **Adicionar**.
    
Essa operação leva alguns minutos, pois precisamos aguardar que o warehouse seja iniciado.

<hr>--i18n-e65265fd-de24-487b-b02c-82f357e3092f
### Navegando nos dados
-- MAGIC
Vamos aproveitar para explorar a página **Data Explorer** atualizada.
-- MAGIC
1. Clique em** Dados** na barra lateral esquerda. Essa página tem uma disposição de interface do usuário diferente do workspace de ciência de dados e engenharia. Observe que temos uma visualização hierárquica do metastore, mas continuamos vemos os mesmos elementos.
1. Vamos explorar a hierarquia. Como antes, os catálogos, esquemas e tabelas que você pode selecionar dependem da quantidade de dados no metastore e das permissões.
1. Daqui também podemos criar objetos ou gerenciar permissões, porém deixaremos essas tarefas para um laboratório posterior.

<hr>--i18n-3ed19095-4056-4b1e-b95c-9e10689ee5da
### Explorar dados usando queries
-- MAGIC
Vamos repetir as queries que fizemos anteriormente no workspace de ciência de dados e engenharia, desta vez criando uma nova query DBSQL e executando-a no SQL warehouse que acabamos de criar. Recapitulando, a query era `SHOW GRANTS ON SCHEMA main.default`.
