# /Includes/Workspace-Setup
<hr>--i18n-d55711b9-31d9-44a4-a64b-9a80e704adb5
# MAGIC
# Configuração do workspace
Este notebook deve ser executado pelos instrutores para preparar o workspace para uma aula.
# MAGIC
As principais mudanças feitas pelo notebook incluem:
* Atualizações de concessões específicas do usuário para que possam criar bancos de dados/esquemas no catálogo atual quando não forem administradores do workspace.
* Configuração de três políticas de cluster:
    * **DBAcademy** - deve ser usada em clusters que executam notebooks padrão.
    * **DBAcademy Jobs** - deve ser usada em fluxos de trabalho/jobs
    * **DBAcademy DLT** - deve ser usada em pipelines do DLT (aplicada automaticamente)
* Criação ou atualização do arquivo compartilhado **DBAcademy Warehouse** para uso em exercícios do Databricks SQL
* Criação do pool de instâncias **DBAcademy** a ser usado pelos estudantes e das políticas de "estudantes" e "jobs".

<hr>--i18n-e181db51-2e1b-403c-8a24-4804cfd935ed
# Obter configuração da classe
As três variáveis definidas por estes widgets são utilizadas para configurar o ambiente e controlar o custo da aula.

<hr>--i18n-f0fa4b17-6a50-4fb1-9792-006194671ee3
# MAGIC
# Init script e instalação de datasets
O principal efeito desta chamada é instalar previamente os datasets.
# MAGIC
Ela tem o efeito colateral de criar o objeto DA que inclui o cliente REST.

<hr>--i18n-0e8ed29d-2dba-4814-bbdb-33ad8bf9a4be
# MAGIC
## Criar pools de instâncias de classe
A célula a seguir configura o pool de instâncias usado para esta classe

<hr>--i18n-3247e3f2-2c46-4fb9-ae6c-eb26f15c963d
# MAGIC
## Criar as três políticas de cluster específicas da classe
As células a seguir criam as diversas políticas de cluster usadas pela classe

<hr>--i18n-4b9690bf-081b-4f09-bd30-d892286b2592
# MAGIC
## Criar endpoint/Databricks SQL warehouse compartilhado pela classe
Criar um único warehouse para ser usado por todos os alunos.
# MAGIC
A configuração é derivada do número de alunos especificado acima.

<hr>--i18n-908300af-d69a-4a8a-b842-b67d3dafdb6b
# MAGIC
## Configurar direitos do usuário
# MAGIC
Esta tarefa simplesmente adiciona o direito "**databricks-sql-access**" ao grupo "**users**" garantindo que eles possam acessar a view do Databricks SQL.

<hr>--i18n-e95863bc-993b-46af-b96e-6f5fb01796ee
# MAGIC
## Atualizar concessões
Esta operação executa **`GRANT CREATE ON CATALOG TO users`** para garantir que os alunos possam criar bancos de dados conforme exigido por este curso quando não forem administradores.
# MAGIC
Observação: A implementação exige que isso seja executado em outro job e, como tal, pode levar cerca de três minutos para ser concluído.

<hr>--i18n-99369c09-72d7-40f1-8081-3a6603156e12
A execução da operação abaixo garante que os alunos possam criar catálogos conforme exigido pelo conteúdo do Unity Catalog neste curso quando não forem administradores.

