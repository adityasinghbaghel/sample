# /DE 7 - Databricks SQL/DE 7.2L - Etapa final de ETL com o DBSQL
<hr>--i18n-9390aad6-4d8b-4e7f-8851-18ca0b6bf7c6
# MAGIC
# MAGIC
# MAGIC
# Etapa final de ETL com o Databricks SQL
# MAGIC
Antes de continuarmos, vamos recapitular alguns dos pontos que aprendemos até agora:
1. O Databricks workspace contém um conjunto de ferramentas para simplificar o ciclo de vida de desenvolvimento de data engineering
1. Os notebooks do Databricks permitem que os usuários combinem SQL com outras linguagens de programação para definir cargas de trabalho de ETL
1. O Delta Lake fornece transações compatíveis com ACID e facilita o processamento incremental de dados no Lakehouse
1. O Delta Live Tables estende a sintaxe SQL para oferecer suporte a diversos padrões de design no Lakehouse, além de simplificar a implantação de infraestrutura
1. Jobs multitarefas permitem a orquestração completa de tarefas, adicionando dependências e programando uma combinação de notebooks e pipelines do DLT
1. O Databricks SQL permite aos usuários editar e executar consultas SQL, criar visualizações e definir painéis
1. O Data Explorer simplifica o gerenciamento de ACLs em tabelas, disponibilizando os dados do Lakehouse para analistas de SQL (que em breve serão amplamente expandidos pelo Unity Catalog)
# MAGIC
Nesta seção, nos concentraremos na exploração de funcionalidades adicionais do DBSQL para dar suporte a cargas de trabalho de produção. 
# MAGIC
Para começar, vamos nos concentrar no uso do Databricks SQL para configurar queries que dão suporte à etapa final de ETL para análises. Vamos usar a IU do Databricks SQL para esta demonstração, mas os SQL warehouses <a href="https://docs.databricks.com/integrations/partners.html" target="_blank">se integram a várias outras ferramentas para permitir a execução de queries externas</a> e ainda têm <a href="https://docs.databricks.com/sql/api/index.html" target="_blank">suporte completo de API para executar queries arbitrárias de forma programática</a>.
# MAGIC
A partir desses resultados de query, geraremos várias visualizações que combinaremos em um painel.
# MAGIC
Por fim, examinaremos a programação de atualizações para queries e painéis e demonstraremos a configuração de alertas para ajudar a monitorar o estado dos datasets de produção ao longo do tempo.
# MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
* Usar o Databricks SQL como uma ferramenta para dar suporte a tarefas de ETL de produção que apoiam cargas de trabalho analíticas
* Configurar queries e visualizações SQL com o Editor do Databricks SQL
* Criar painéis no Databricks SQL
* Programar atualizações para queries e painéis
* Definir alertas para queries SQL

<hr>--i18n-9ff94613-d58b-48fe-b32d-3718cbcb2f30
# MAGIC
# MAGIC
# MAGIC
## Executar o script de configuração
As células a seguir executam um notebook que define a classe que usaremos para gerar queries SQL.

<hr>--i18n-c384931f-c934-440f-9f0c-a7b04c1c28b0
# MAGIC
## Criar um banco de dados de demonstração 
Execute a célula a seguir e copie os resultados no Editor do Databricks SQL.
# MAGIC
Essas queries:
* Criam um novo banco de dados
* Declaram duas tabelas (que usaremos para carregar dados)
* Declaram duas funções (que usaremos para gerar dados)
# MAGIC
Copie a query e clique no botão **Executar** para executá-la.

<hr>--i18n-9cd7f410-eead-4759-bd99-83eafb03d0df
# MAGIC
# MAGIC
**OBSERVAÇÃO**: as queries acima foram projetadas para serem executadas apenas uma vez após redefinir completamente a demonstração para reconfigurar o ambiente. 
# MAGIC
Os usuários precisarão ter as permissões **`CREATE`** e **`USAGE`** no catálogo para executá-las.

<hr>--i18n-3652d1e3-fc73-44f9-9b07-6b1b8b5343d6
# MAGIC
**AVISO:** Certifique-se de selecionar seu banco de dados antes de prosseguir, já que a instrução **`USE`**<br/>ainda não altera o banco de dados no qual as queries serão executadas

<hr>--i18n-a0b2608d-d22e-4085-b02f-be43b744ecf2
# MAGIC
# MAGIC
# MAGIC
## Criar uma query para carregar dados
Passos:
1. Execute a célula abaixo para imprimir uma query SQL formatada para carregar dados na tabela **`user_ping`** criada no passo anterior.
1. Salve essa query com o nome **Carregar dados de ping**.
1. Execute essa query para carregar um lote de dados.

<hr>--i18n-48fc3085-b405-450f-9463-ee1501cd56aa
# MAGIC
# MAGIC
# MAGIC
A execução da query deve carregar alguns dados e retornar uma visualização dos dados na tabela.
# MAGIC
**OBSERVAÇÃO**: Como números aleatórios estão sendo usados para definir e carregar dados, cada usuário verá valores ligeiramente diferentes.

<hr>--i18n-7dc1c1c9-96c5-49f7-a1ab-deb44594cddf
# MAGIC
# MAGIC
# MAGIC
## Definir uma programação de atualização de queries
# MAGIC
Passos:
1. Localize o botão **Programar** no canto superior direito da caixa do editor de queries SQL
1. Use o menu dropdown para selecionar atualizar a cada **1 semana** e defina a hora como **12:00**
1. No dia da semana, selecione **Domingo**.
1. Clique em **OK**
# MAGIC
**OBSERVAÇÃO:** Estamos usando uma programação de atualização de uma semana para fins de sala de aula, mas você provavelmente verá intervalos de acionamento mais curtos na produção, como atualizações a cada minuto.

<hr>--i18n-58aa7a7b-4b43-429d-8603-0cc1001a5ae6
# MAGIC
# MAGIC
# MAGIC
## Criar uma query para rastrear o total de registros
Passos:
1. Execute a célula abaixo.
1. Salve a query com o nome **User Counts**.
1. Execute a query para calcular os resultados atuais.

<hr>--i18n-c43c8f47-c8c3-4232-9a76-f82bdd204317
# MAGIC
# MAGIC
# MAGIC
## Criar uma visualização de gráfico de barras
# MAGIC
Passos:
1. Clique no sinal **+** ao lado da tab de resultados no meio da janela e selecione **Visualização** na caixa de diálogo
1. Clique no nome (o default deve ser algo como **`Bar 1`**) e altere o nome para **Total de registros de usuários**
1. Defina **`user_id`** para a **Coluna X**
1. Defina **`total_records`** para a **Coluna Y**
1. Clique em **Salvar**

<hr>--i18n-644501a5-c5fc-45e0-bd34-728413b5d267
# MAGIC
# MAGIC
# MAGIC
## Criar um painel
# MAGIC
Passos:
1. Clique no botão com três pontos verticais na parte inferior da tela e selecione **Adicionar ao painel**.
1. Clique na opção **Criar novo painel**
1. Nomeie o painel como <strong>Resumo de pings do usuário **`<your_initials_here>`**</strong>
1. Clique em **Salvar** para criar o painel
1. O painel recém-criado agora deve estar selecionado como destino; clique em **OK** para adicionar sua visualização

<hr>--i18n-400bfcee-6c58-4863-b876-609950543f6f
# MAGIC
# MAGIC
# MAGIC
## Criar uma consulta para calcular o ping médio recente
Passos:
1. Execute a célula abaixo para imprimir a query SQL formatada.
1. Salve essa consulta com o nome **Ping médio**.
1. Execute a consulta para calcular os resultados atuais.

<hr>--i18n-82ba8c89-8e4b-4ab5-817a-1161017a1168
# MAGIC
# MAGIC
# MAGIC
## Adicionar uma visualização de gráfico de linha ao painel
# MAGIC
Passos:
1. Clique no botão **Adicionar visualização**
1. Clique no nome (o default deve ser algo como **`Visualization 1`**) e altere-o para **Ping médio do usuário**
1. Selecione **`Line`** para o **Tipo de visualização**
1. Defina **`end_time`** para a **Coluna X**
1. Defina **`avg_ping`** para as **Colunas Y**
1. Defina **`user_id`** para **Agrupar por**
1. Clique em **Salvar**
1. Clique no botão com três pontos verticais na parte inferior da tela e selecione** Adicionar ao painel **.
1. Selecione o painel que criou anteriormente
1. Clique em **OK** para adicionar a visualização

<hr>--i18n-9088bbce-cf24-4731-80c0-15972786eda1
# MAGIC
# MAGIC
# MAGIC
## Criar uma consulta para relatar estatísticas resumidas
Passos:
1. Execute a célula abaixo.
1. Salve essa consulta com o nome **Resumo de pings**.
1. Execute a consulta para calcular os resultados atuais.

<hr>--i18n-e5832d13-b8f3-40db-95cb-8c24e895cda7
# MAGIC
# MAGIC
# MAGIC
## Adicionar a tabela de resumo ao painel
# MAGIC
Passos:
1. Clique no botão com três pontos verticais na parte inferior da tela e selecione** Adicionar ao painel **.
1. Selecione o painel que você criou anteriormente
1. Clique** OK** para adicionar sua visualização

<hr>--i18n-626f8b1d-51cd-47b0-8828-b35180acb40c
# MAGIC
# MAGIC
# MAGIC
## Revisar e atualizar o painel
# MAGIC
Passos:
1. Use a barra lateral esquerda para navegar até **Painéis**
1. Encontre o painel ao qual você adicionou as queries
1. Clique no botão azul **Refresh** para atualizar o painel
1. Clique no botão **Programar** para revisar as opções de programação do painel
  * Observe que programar um painel para atualização executará todas as queries associadas ao painel
  * Não agende o painel neste momento

<hr>--i18n-4f69c53f-8bd9-48d5-9f6f-f97045581e49
# MAGIC
# MAGIC
## Compartilhar o painel
# MAGIC
Passos:
1. Clique no botão **Compartilhar**
1. Selecione **Todos os usuários** no campo superior
1. Escolha **Pode executar** no campo direito
1. Clique em **Adicionar**
1. Altere as **Credenciais** para **Executar como visualizador**
# MAGIC
**OBSERVAÇÃO**: no momento, nenhum outro usuário deve ter permissões para executar seu painel, pois eles não receberam permissões para os bancos de dados e tabelas subjacentes usando ACLs em tabelas. Se quiser que outros usuários possam acionar atualizações em seu painel, você precisará conceder a eles a permissão **Executar como proprietário** ou adicionar permissões às tabelas referenciadas em suas queries.

<hr>--i18n-facded12-10b1-4c63-a075-b7790fa3cd17
# MAGIC
# MAGIC
## Configurar um alerta
# MAGIC
Passos:
1. Use a barra lateral esquerda para navegar até **Alertas**
1. Clique em **Criar alerta** no canto superior direito
1. Selecione a query **User Counts**
1. Clique no campo no canto superior esquerdo da tela para nomear o alerta como **`<your_initials> Count Check`**
1. Nas opções **Acionar quando**, configure:
  * **Coluna de valor**: **`total_records`**
  * **Condição**: **`>`**
  * **Limite**: **`15`**
1. Em **Refresh**, selecione **Nunca**
1. Clique em **Criar alerta**
1. Na próxima tela, clique no botão azul **Refresh** no canto superior direito para avaliar o alerta

<hr>--i18n-f9f22ebd-4283-474a-ab45-0d7584a6b6ac
# MAGIC
# MAGIC
# MAGIC
## Revisar as opções de destino de alerta
# MAGIC
# MAGIC
# MAGIC
Passos:
1. Na visualização do alerta, clique no botão azul **Adicionar** à direita de **Destinos** no lado direito da tela
1. Na parte inferior da janela mostrada, localize e clique no texto azul da mensagem **Criar novos destinos de alerta**
1. Revisar as opções de alerta disponíveis

