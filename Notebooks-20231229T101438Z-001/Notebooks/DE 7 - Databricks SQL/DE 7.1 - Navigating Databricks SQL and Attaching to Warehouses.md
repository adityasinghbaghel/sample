# /DE 7 - Databricks SQL/DE 7.1 - Navegando no Databricks SQL e conectando-se a warehouses
<hr>--i18n-2396d183-ba9d-4477-a92a-690506110da6
# MAGIC
# MAGIC
# Navegando no Databricks SQL e anexando a SQL Warehouses
# MAGIC
* Navegue até o Databricks SQL  
  * Certifique-se de que o SQL esteja selecionado na opção de workspace na barra lateral (diretamente abaixo do logotipo do Databricks)
* Certifique-se de que um SQL warehouse esteja ativado e acessível
  * Navegue até o SQL Warehouses na barra lateral
  * Se houver um SQL warehouse e seu estado for **`Running`**, use-o
  * Se houver um SQL warehouse, mas for no estado **`Stopped`**, clique no botão **`Start`** se ele estiver habilitado (**OBSERVAÇÃO**: inicie o menor SQL warehouse disponível) 
  * Se não houver nenhum SQL warehouse e a opção **`Create SQL Warehouse`** estiver habilitada; , clique nela, dê ao SQL warehouse um nome facilmente reconhecível e defina o tamanho do cluster como 2X-Small. Mantenha a definição default de todas as outras opções.
  * Se não for possível criar ou se anexar a um SQL warehouse, você precisará entrar em contato com um administrador do workspace e solicitar acesso aos recursos de computação do Databricks SQL para continuar.
* Navegue até a página inicial no Databricks SQL
  * Clique no logotipo do Databricks na parte superior da barra de navegação lateral
* Localize os **Painéis de amostra** e clique em **`Visit gallery`** (Visitar galeria)
* Clique em **`Import`** (Importar) próximo à opção **Retail Revenue & Supply Chain**
  * Se houver um SQL warehouse disponível para você, essa ação deverá carregar um painel que exibirá imediatamente os resultados
  * Clique em **Refresh** no canto superior direito (os dados subjacentes não foram alterados, mas esse é o botão que seria usado para obter alterações)
# MAGIC
# Atualizando um painel do DBSQL
# MAGIC
* Use o navegador da barra lateral para encontrar os **Painéis**
  * Localize o painel de amostra que você acabou de carregar. O nome do painel deve ser **Retail Revenue & Supply Chain** e seu nome de usuário será indicado sob o campo **`Created By`** (Criado por). **OBSERVAÇÃO**: a opção **Meus painéis** no lado direito pode servir como um atalho para filtrar outros painéis no workspace
  * Clique no nome do painel para visualizá-lo
* Visualize a query que gerou o gráfico **Mudanças nas prioridades de preços**
  * Passe o mouse sobre o gráfico para exibir três pontos verticais. Clique nos pontos verticais
  * Selecione **Visualizar consulta** no menu mostrado
* Revise o código SQL usado para preencher o gráfico
  * Observe que o namespace de três níveis é usado para identificar a tabela de origem, que é a prévia de uma nova funcionalidade a ser oferecida pelo Unity Catalog
  * Clique em **`Run`** (Executar) no canto superior direito da tela para visualizar os resultados da query
* Revise a visualização
  * Na query, uma tab chamada **Tabela** deve estar selecionada; clique em **Preço por prioridade ao longo do tempo** para mudar para uma visualização do gráfico
  * Clique em **Editar visualização** na parte inferior da tela para revisar as configurações
  * Explore o efeito de configurações diferentes na visualização
  * Se desejar aplicar as alterações, clique em **Salvar**; caso contrário, clique em **Cancelar**
* De volta ao editor de queries, clique no botão **Adicionar visualização** à direita do nome da visualização
  * Crie um gráfico de barras
  * Defina a **Coluna X** como **`Date`**
  * Defina a **Coluna Y** como **`Total Price`**
  * **Agrupe por** **`Priority`**
  * Defina **Empilhamento** como **`Stack`**
  * Deixe todas as outras configurações como por default
  * Clique em **Salvar**
* De volta ao editor de queries, clique no nome default da visualização para editá-la e altere seu nome para **`Stacked Price`**
* Na parte inferior da tela, clique nos três pontos verticais à esquerda do botão **`Edit Visualization`** (Editar visualização)
  * Selecione **Adicionar ao painel** no menu
  * Selecione o painel **`Retail Revenue & Supply Chain`**
* Navegue de volta ao painel para visualizar essa alteração
# MAGIC
# Criar uma query
# MAGIC
* Use a barra lateral para navegar até **Queries**
* Clique no botão **`Create Query`** (criar query)
* Certifique-se de que há um SQL warehouse conectado. No **Navegador de esquema**, clique no metastore atual e selecione **`samples`**. 
  * Selecione o banco de dados **`tpch`**
  * Clique na tabela **`partsupp`** para obter uma visualização do esquema
  * Enquanto passa o mouse sobre a tabela **`partsupp`**, clique no botão **>>** para inserir o nome da tabela no texto da query
* Escreva a primeira query:
  * **`SELECT * FROM`** a tabela **`partsupp`** utilizando o nome completo importado no último passo e clique em **Executar** para visualizar os resultados
  * Modifique essa query para **`GROUP BY ps_partkey`** com os retornos **`ps_partkey`** e **`sum(ps_availqty)`** e clique em **Executar** para visualizar os resultados
  * Atualize a query para dar o alias da segunda coluna chamada **`total_availqty`** e execute a query novamente
* Salve a query
  * Clique no botão **Salvar** ao lado de **Executar**, próximo ao canto superior direito da tela
  * Dê à query um nome fácil de lembrar
* Adicione a query ao seu painel
  * Clique nos três botões verticais na parte inferior da tela
  * Clique em **Adicionar ao painel**
  *Selecione seu**`Retail Revenue & Supply Chain`** painel
*Navegue de volta ao seu painel para visualizar esta alteração
  * Caso queira alterar a organização das visualizações, clique nos três botões verticais no canto superior direito da tela e clique em **Editar** no menu mostrado; assim você poderá arrastar e redimensionar as visualizações
