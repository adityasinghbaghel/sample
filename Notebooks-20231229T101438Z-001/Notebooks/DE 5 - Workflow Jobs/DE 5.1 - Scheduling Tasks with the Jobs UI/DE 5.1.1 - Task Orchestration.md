# /DE 5 - Workflow Jobs/DE 5.1 - Scheduling Tasks with the Jobs UI/DE 5.1.1 - Task Orchestration
<hr>--i18n-4603b7f5-e86f-44b2-a449-05d556e4769c
# MAGIC
# MAGIC
# Orquestração de jobs com o Databricks Workflows
# MAGIC
Novas atualizações na UI de jobs do Databricks adicionaram a capacidade de programar várias tarefas como parte de um job. Assim, os jobs do Databricks podem lidar com toda a orquestração da maioria das cargas de trabalho de produção.
# MAGIC
Para começar, vamos revisar as etapas da programação de uma tarefa de notebook como um job autônomo acionado e, em seguida, adicionar uma tarefa dependente usando um pipeline do DLT. 
# MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
* Programar uma tarefa de notebook em um job do fluxo de trabalho do Databricks
* Descrever opções de programação de jobs e as diferenças entre os tipos de cluster
* Revisar as execuções de jobs para acompanhar o progresso e ver os resultados
* Programar uma tarefa de pipeline do DLT em um Workflow Job do Databricks
* Configurar dependências lineares entre tarefas usando a UI do Databricks Workflows

<hr>--i18n-fb1b72e2-458f-4930-b070-7d60a4d3b34f
# MAGIC
## Gerar a configuração do job
# MAGIC
A configuração deste job exige parâmetros exclusivos para um determinado usuário.
# MAGIC
Execute a célula abaixo para imprimir os valores que você usará para configurar seu pipeline nas etapas subsequentes.

<hr>--i18n-b3634ee0-e06e-42ca-9e70-a9a02410f705
# MAGIC
## Configurar o job com uma única tarefa de notebook
# MAGIC
Ao usar a UI de jobs para orquestrar uma carga de trabalho com diversas tarefas, você sempre começa criando um job com uma única tarefa.
# MAGIC
Passos:
1. Clique no botão **Workflows** na barra lateral, clique na tab **Jobs** e no botão **Criar job**.
2. Configure o job e a tarefa conforme especificado abaixo. Você precisará dos valores fornecidos na saída da célula acima para esta etapa.
# MAGIC
| Configuração | Instruções |
|--|--|
| Nome da tarefa | Insira **Reset** |
| Tipo | Escolha **Notebook** |
| Fonte | Escolha **Workspace** |
| Caminho | Use o navegador para especificar o **caminho do notebook Reset** fornecido acima |
| Cluster | No menu dropdown, em **Clusters All Purpose existentes**, selecione seu cluster |
| Nome do job | No canto superior esquerdo da tela, digite o **nome do job** fornecido acima para nomear o job (não a tarefa) |
# MAGIC
<br>
# MAGIC
3. Clique no** Criar** botão.
4. Clique no botão azul **Executar agora** no canto superior direito para iniciar o job.
# MAGIC
<img src="https://files.training.databricks.com/images/icon_note_24.png"> **Observação**: Ao selecionar o cluster all-purpose, você receberá um aviso informando que isso será cobrado como um compute all-purpose. Os jobs de produção devem sempre ser programados em novos clusters de job com a dimensão apropriada à carga de trabalho, pois isso é cobrado a uma taxa muito mais baixa.

<hr>--i18n-eb8d7811-b356-41d2-ae82-e3762add19f7
# MAGIC
# MAGIC
## Explorar as opções de programação
Passos:
1. No lado direito da UI de jobs, localize a seção **Detalhes do job**.
1. Na seção **Gatilho**, selecione o botão **Adicionar gatilho** para conhecer as opções de programação.
1. Alterar o **Tipo de gatilho** de **Nenhum (Manual)** para **Programado** abrirá uma UI de programação cronológica.
   - Essa UI oferece diversas opções para configurar a programação cronológica dos jobs. As configurações definidas na UI também podem ser geradas na sintaxe cronológica, que você poderá editar se precisar usar uma configuração personalizada não disponível na UI.
1. Neste momento, deixaremos o job definido com a programação **Manual**. Selecione **Cancelar** para retornar aos detalhes do job.

<hr>--i18n-eb585218-f5df-43f8-806f-c80d6783df16
# MAGIC
# MAGIC
## Revisar a execução
# MAGIC
Para revisar a execução do job:
1. Na página de detalhes de jobs, selecione a tab **Execuções** no canto superior esquerdo da tela (você provavelmente está na tab **Tarefas**)
1. Encontre o job.
    - **Jobs que estão em execução** podem ser encontrados na seção **Execuções ativas**. 
    - **Jobs que terminaram de ser executados** são colocados na seção **Execuções concluídas**
1. Abra os detalhes de saída clicando no campo de carimbo de data/hora abaixo da coluna **Hora de início**
    - Se **o job ainda estiver em execução**, o estado ativo do notebook será um **Status** de **`Pending`** ou **`Running`**, como mostrado no painel lateral direito. 
    - Se **o job tiver sido concluído**, a execução completa do notebook terá um **Status** de **`Succeeded`** ou **`Failed`** mostrado no painel lateral direito
  
O notebook emprega o comando mágico **`%run`** para chamar um notebook adicional usando um caminho relativo. Observe que, embora o tópico não seja abordado neste curso, <a href="https://docs.databricks.com/repos.html#work-with-non-notebook-files-in-a-databricks-repo" target="_blank">uma nova funcionalidade adicionada ao Databricks Repos permite carregar módulos Python usando caminhos relativos</a>.
# MAGIC
O resultado real do notebook programado é redefinir o ambiente para o novo job e o pipeline.

<hr>--i18n-bc61c131-7d68-4633-afd7-609983e43e17
## Gerar um pipeline
# MAGIC
Neste passo, adicionaremos um pipeline do DLT a ser executado após a conclusão bem-sucedida da tarefa configurada no início desta lição.
# MAGIC
Para concentrar a operação em jobs, e não em pipelines, usaremos o seguinte comando utilitário para criar um pipeline simples.

<hr>--i18n-19e4daea-c893-4871-8937-837970dc7c9b
# MAGIC
## Configurar uma tarefa de pipeline do DLT
# MAGIC
A seguir, precisamos adicionar uma tarefa para executar este pipeline.
# MAGIC
Passos:
1. Na página Detalhes do job, clique na tab **Tarefas**.
1. Clique no botão azul **+ Adicionar tarefa**, na parte inferior central da tela, e selecione **Pipeline do Delta Live Tables** no menu dropdown.
1. Configure a tarefa conforme especificado abaixo.
# MAGIC
| Configuração | Instruções |
|--|--|
| Nome da tarefa | Insira **DLT** |
| Tipo | Escolha **Pipeline do Delta Live Tables** |
| Pipeline | Escolha o pipeline do DLT configurado acima |
| Depende de | Escolha **Reset**, que é a tarefa anterior que definimos |
# MAGIC
<br>
# MAGIC
4. Clique no botão azul **Criar tarefa**
    - Agora você deverá ver uma tela com duas caixas e uma seta para baixo entre elas. 
    - A tarefa **`Reset`** está no alto, conduzindo até a tarefa **`DLT`**. 
    - Essa visualização representa as dependências entre as tarefas.
5. Valide a configuração executando o comando abaixo.
    - Se forem relatados erros, repita o procedimento a seguir até que todos os erros tenham sido removidos.
      - Corrija os erros.
      - Clique no botão **Criar tarefa**.
      - Valide a configuração.

<hr>--i18n-1c949168-e917-455d-8a54-2768592a16f1
# MAGIC
## Executar o job
Depois que o job tiver sido configurado corretamente, clique no botão azul **Executar agora** no canto superior direito para iniciá-lo.
<img src="https://files.training.databricks.com/images/icon_note_24.png"> **Observação**: Ao selecionar tudo- cluster de finalidade, você receberá um aviso sobre como isso será cobrado como todos- cálculo de propósito. Os trabalhos de produção devem sempre ser agendados em relação a novos clusters de trabalhos dimensionados adequadamente para a carga de trabalho, pois isso é cobrado a uma taxa muito mais baixa.
# MAGIC
**OBSERVAÇÃO**: Talvez seja necessário aguardar alguns minutos enquanto a infraestrutura do job e do pipeline é implantada.

<hr>--i18n-666a45d2-1a19-45ba-b771-b47456e6f7e4
# MAGIC
# MAGIC
## Revisar os resultados da execução multitarefa
# MAGIC
Para revisar os resultados da execução:
1. Na página Detalhes do job, selecione a tab **Execuções** novamente e, em seguida, selecione a execução mais recente em **Execuções ativas** ou **Execuções concluídas**, a depender do status de conclusão do job.
    - As visualizações das tarefas serão atualizadas em tempo real para refletir as tarefas sendo executadas e mudarão de cor se ocorrerem falhas nas tarefas. 
1. Clique em uma caixa de tarefa para renderizar o notebook programado na UI. 
    - Esse passo pode ser considerado apenas como uma camada adicional de orquestração além da UI anterior de jobs do Databricks, se isso ajudar.
    - Observe que, se você cargas de trabalho programando jobs com a CLI ou a API REST, <a href="https://docs.databricks.com/dev-tools/api/latest/jobs.html" target="_blank">a estrutura JSON usada para configurar e obter resultados dos jobs também passou por atualizações semelhantes de UI</a>.
# MAGIC
**OBSERVAÇÃO**: No momento, os pipelines do DLT programados como tarefas não geram resultados diretamente na GUI de execuções. Em vez disso, você retornará à GUI do pipeline do DLT programado.
