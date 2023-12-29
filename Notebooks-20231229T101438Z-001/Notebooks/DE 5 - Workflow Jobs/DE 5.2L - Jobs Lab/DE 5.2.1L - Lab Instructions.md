# /DE 5 - Workflow Jobs/DE 5.2L - Jobs Lab/DE 5.2.1L - Lab Instructions
<hr>--i18n-7b85ffd8-b52c-4ea8-84db-b3668e13b402
# MAGIC
# MAGIC
# Laboratório: Orquestrando jobs com o Databricks
# MAGIC
Neste laboratório, você configurará um job multitarefa composto por:
* Um notebook que transfere um novo lote de dados para um diretório de armazenamento
* Um pipeline do Delta Live Tables que processa esses dados em várias tabelas
* Um notebook que consulta a tabela gold produzida pelo pipeline, bem como várias métricas geradas pelo DLT
# MAGIC
## Objetivos de aprendizado
Ao final deste laboratório, você será capaz de:
* Programar um notebook como uma tarefa em um job do Databricks
* Programar um pipeline do DLT como uma tarefa em um job do Databricks
*Configurar dependências lineares entre tarefas usando a UI do Databricks Workflows

<hr>--i18n-003b65fd-018a-43e5-8d1d-1ce2bee52fe3
# MAGIC
# MAGIC
## Obter dados iniciais
Preencha a zona de destino com alguns dados antes de continuar. 
# MAGIC
Você executará novamente esse comando para obter dados adicionais mais tarde.

<hr>--i18n-cc5b4584-59c4-49c9-a6d2-efcdadb98dbe
# MAGIC
## Gerar configuração de trabalho
# MAGIC
A configuração deste trabalho exigirá parâmetros exclusivos para um determinado usuário.
# MAGIC
Execute a célula abaixo para imprimir os valores que você usará para configurar seu pipeline nas etapas subsequentes.

<hr>--i18n-542fd1c6-0322-4c8f-8719-fe980f2a8013
# MAGIC
## Configurar trabalho com uma única tarefa de notebook
# MAGIC
Para começar, vamos programar o primeiro notebook.
# MAGIC
Passos:
1. Clique no** Workflows** na barra lateral, clique no botão** Jobs** guia e clique no** Criar Job** botão.
2. Configure o job e a tarefa conforme especificado abaixo. Você precisará dos valores fornecidos na saída da célula acima para esta etapa.
# MAGIC
| Configuração | Instruções |
|--|--|
| Nome da tarefa | Insira **Batch-Job** |
| Tipo | Escolha **Notebook** |
| Fonte | Escolha **Workspace** |
| Caminho | Use o navegador para especificar o **caminho do notebook de lotes** fornecido acima |
| Cluster | Selecione seu cluster no menu dropdown, em **Clusters all-purpose existentes** |
| Nome do job | No topo- esquerda da tela, digite o** Nome do Job** fornecido acima para adicionar um nome para o job (não para a tarefa) |
# MAGIC
<br>
# MAGIC
3. Clique no** Criar** botão.
4. Clique no azul** Run agora** botão no canto superior direito para iniciar o job.
# MAGIC
<img src="https://files.training.databricks.com/images/icon_note_24.png"> **Observação**: Ao selecionar o- cluster all purpose, você receberá um aviso sobre como isso será cobrado como all- purpose. Os jobs de produção devem sempre ser agendados em relação a novos clusters de jobs dimensionados adequadamente para a carga de trabalho, pois isso é cobrado a uma taxa muito mais baixa.

<hr>--i18n-4678fc9d-ab2f-4f8c-b4be-a67774b2afd4
## Gerar um pipeline
# MAGIC
Neste passo, adicionaremos um pipeline do DLT a ser executado após a conclusão bem-sucedida da tarefa configurada acima.
# MAGIC
Para focar em jobs e não em pipelines, usaremos o seguinte comando utilitário para criar um pipeline simples para nós.

<hr>--i18n-8ab4bc2a-e08e-47ca-8507-9565342acfb6
# MAGIC
## Adicionar uma tarefa de pipeline
# MAGIC
Passos:
1. Na página Detalhes do Job, clique no botão** Tarefas** aba.
1. Clique no azul** + Adicionar tarefa** botão na parte inferior central da tela e selecione** Pipeline Delta Live Tables** no menu suspenso.
1. Configure a tarefa:
# MAGIC
| Configuração | Instruções |
|--|--|
| Nome da tarefa | Insira **DLT** |
| Tipo | Escolher** Pipeline Delta Live Tables** |
| Pipeline | Escolha o pipeline DLT configurado acima |
| Depende de | Escolha **Batch-Job**, a tarefa que definimos anteriormente |
# MAGIC
<br>
# MAGIC
4. Clique no azul** Criar tarefa** botão
    -Agora você deverá ver uma tela com 2 caixas e uma seta para baixo entre elas. 
    - A tarefa **`Batch-Job`** está no alto, conduzindo até a tarefa **`DLT`**. 

<hr>--i18n-1cdf590a-6da0-409d-b4e1-bd5a829f3a66
# MAGIC
# MAGIC
## Adicionar outra tarefa de notebook
# MAGIC
Foi fornecido um notebook adicional que consulta algumas métricas do DLT e a tabela ouro definida no pipeline do DLT. Adicionaremos essa tarefa ao final do job.
# MAGIC
Passos:
1. Na página Detalhes do Job, clique no botão** Tarefas** aba.
1. Clique no botão azul **+ Adicionar tarefa**, na parte inferior central da tela, e selecione **Notebook** no menu dropdown.
1. Configure a tarefa:
# MAGIC
| Configuração | Instruções |
|--|--|
| Nome da tarefa | Insira **Query-Results** |
| Tipo | Escolha **Notebook** |
| Fonte | Escolha **Workspace** |
| Caminho | Use o navegador para especificar o **caminho do notebook de queries** fornecido acima |
| Cluster | Selecione seu cluster no menu suspenso, em** Clusters All Purpose existentes** |
| Depende de | Escolha **DLT**, que é a tarefa anterior que definimos |
# MAGIC
<br>
# MAGIC
4. Clique no azul** Criar tarefa** botão
5. Clique no botão azul **Executar agora**, no canto superior direito, para executar este job.
    - Na tab **Execuções**, você pode clicar no horário de início da execução na seção **Execuções ativas** e acompanhar visualmente o progresso da tarefa.
    - Depois que todas as tarefas forem bem-sucedidas, revise o conteúdo de cada uma delas para confirmar que tiveram o comportamento esperado.

