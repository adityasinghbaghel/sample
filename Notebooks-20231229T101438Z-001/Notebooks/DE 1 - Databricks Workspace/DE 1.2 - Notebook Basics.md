# /DE 1 - Databricks Workspace/DE 1.2 - Noções básicas de notebooks
<hr>--i18n-2d4e57a0-a2d5-4fad-80eb-98ff30d09a37
# MAGIC
# Noções básicas de notebooks
# MAGIC
Os notebooks são o método principal de desenvolvimento e execução de código de forma interativa no Databricks. Esta lição fornece uma introdução básica ao uso de notebooks do Databricks.
# MAGIC
Se você já usou notebooks do Databricks, mas esta é a primeira vez que executa um notebook no Databricks Repos, notará que a funcionalidade básica é a mesma. Na próxima lição, analisaremos algumas das funcionalidades que o Databricks Repos acrescenta aos notebooks.
# MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
* Anexar um notebook a um cluster
* Executar uma célula em um notebook
* Definir a linguagem de um notebook
* Descrever e usar comandos mágicos
* Criar e executar uma célula SQL
* Criar e executar uma célula Python
* Criar uma célula Markdown
* Exportar um notebook do Databricks
* Exportar uma coleção de notebooks do Databricks

<hr>--i18n-e7c4cc85-5ab7-46c1-9da3-f9181d77d118
# MAGIC
## Anexar a um cluster
# MAGIC
Na lição anterior, você criou um cluster ou identificou um cluster que uma pessoa que administra seu workspace configurou para seu uso.
# MAGIC
No canto superior direito da tela, clique no seletor de cluster (botão "Conectar") e escolha um cluster no menu dropdown. Quando o notebook está conectado a um cluster, esse botão mostra o nome do cluster.
# MAGIC
**OBSERVAÇÃO**: A implantação de um cluster pode levar vários minutos. Um círculo verde sólido aparecerá à esquerda do nome do cluster assim que os recursos forem implantados. Se o cluster tiver um círculo cinza vazio à esquerda, você precisará seguir as instruções para <a href="https://docs.databricks.com/clusters/clusters-manage.html#start-a-cluster" target="_blank">iniciar um cluster</a>.

<hr>--i18n-23aed87f-d375-4b81-8c3c-d4375cda0384
# MAGIC
## Noções básicas de notebooks
# MAGIC
Os notebooks permitem executar o código célula por célula. É possível combinar várias linguagens em um notebook. Os usuários podem adicionar gráficos, imagens e texto de marcação para aprimorar o código.
# MAGIC
Os notebooks que usamos ao longo deste curso se destinam a ser instrumentos de aprendizagem. É fácil implantar notebooks como código de produção com o Databricks, que ainda fornece um conjunto robusto de ferramentas para exploração de dados, geração de relatórios e criação de painéis.
# MAGIC
### Executando uma célula
* Execute a célula abaixo usando uma das seguintes opções:
  * **CTRL+ENTER** ou **CTRL+RETURN**
  * **SHIFT+ENTER** ou **SHIFT+RETURN** para executar a célula e ir para a próxima
  * Usando as opções **Executar célula**, **Executar todos acima** ou **Executar todos abaixo**, como mostrado aqui<br/><img style="box-shadow: 5px 5px 5px 0px rgba(0,0,0,0.25); border: 1px solid rgba(0,0,0,0.25);" src="https://files.training.databricks.com/images/notebook-cell-run-cmd.png"/>

<hr>--i18n-5b14b4c2-c009-4786-8058-a3ddb61fa41d
# MAGIC
**OBSERVAÇÃO**: Quando o código é executado célula por célula, as células podem ser executadas várias vezes ou fora de ordem. A menos que receba uma instrução explícita, você deve sempre presumir que os notebooks deste curso se destinam à execução de uma célula por vez, de cima para baixo. Se você encontrar um erro, leia o texto antes e depois de uma célula para ter certeza de que o erro não foi gerado como parte intencional do aprendizado antes de tentar solucionar o problema. A maioria dos erros pode ser resolvida executando células anteriores em um notebook que foram puladas ou executando novamente todo o notebook desde o início.

<hr>--i18n-9be4ac54-8411-45a0-ad77-7173ec7402f8
# MAGIC
### Configurando a linguagem default do notebook
# MAGIC
A célula acima executa um comando Python, que é a linguagem default atual definida no notebook.
# MAGIC
Os notebooks do Databricks são compatíveis com Python, SQL, Scala e R. A linguagem pode ser selecionada quando o notebook é criado, mas isso pode ser alterado a qualquer momento.
# MAGIC
A linguagem default é mostrada à direita do título do notebook, na parte superior da página. Ao longo deste curso, usaremos uma combinação de notebooks SQL e Python.
# MAGIC
Vamos alterar a linguagem default do notebook para SQL.
# MAGIC
Passos:
* Clique em **Python** ao lado do título do notebook na parte superior da tela
* Na IU que aparece, selecione **SQL** na lista dropdown 
# MAGIC
**OBSERVAÇÃO**: Na célula imediatamente anterior a esta, você deve ver uma nova linha aparecer com <strong><code>&#37;python</code></strong>. Falaremos sobre isso em breve.

<hr>--i18n-3185e9b5-fcba-40aa-916b-5f3daa555cf5
# MAGIC
### Criar e executar uma célula SQL
# MAGIC
* Destaque esta célula e pressione o botão **B** no teclado para criar uma nova célula abaixo dela
* Copie o código a seguir na célula abaixo e execute-a
# MAGIC
**`%sql`**<br/>
**`SELECT "I'm running SQL!"`**
# MAGIC
**OBSERVAÇÃO**: Há vários métodos diferentes para adicionar, mover e excluir células, incluindo opções de GUI e atalhos de teclado. Consulte a <a href="https://docs.databricks.com/notebooks/notebooks-use.html#develop-notebooks" target="_blank">documentação</a> para obter detalhes.

<hr>--i18n-5046f81c-cdbf-42c3-9b39-3be0721d837e
# MAGIC
## Comandos mágicos
* Os comandos mágicos são específicos para os notebooks do Databricks
* Eles são muito semelhantes aos comandos mágicos encontrados em notebooks comparáveis
* São comandos integrados que fornecem o mesmo resultado, independentemente da linguagem do notebook
* Um único símbolo de porcentagem (%) no início de uma célula identifica um comando mágico
  * Você só pode ter um comando mágico por célula
  * O comando mágico deve ser o primeiro elemento em uma célula

<hr>--i18n-39d2c50e-4b92-46ef-968c-f358114685be
# MAGIC
### Comandos mágicos de linguagem
Os comandos mágicos de linguagem permitem executar código em linguagens diferentes da default do notebook. Neste curso, veremos os seguintes comandos mágicos de linguagem:
* <strong><code>&#37;python</code></strong>
* <strong><code>&#37;sql</code></strong>
# MAGIC
Não é necessário adicionar um comando mágico de linguagem ao tipo de notebook definido no momento.
# MAGIC
Quando mudamos a linguagem do notebook de Python para SQL acima, o comando <strong><code>&#37;python</code></strong> foi adicionado às células existentes escritas em Python.
# MAGIC
**OBSERVAÇÃO**: Em vez de alterar constantemente a linguagem default de um notebook, você deve manter uma linguagem primária como default e usar apenas comandos mágicos de linguagem quando precisar executar o código em outra linguagem.

<hr>--i18n-94da1696-d0cf-418f-ba5a-d105a5ecdaac
# MAGIC
### Markdown
# MAGIC
O comando mágico **&percnt;md** permite renderizar o Markdown em uma célula:
* Clique duas vezes na célula para começar a editá-la
* Em seguida, pressione **`Esc`** para interromper a edição
# MAGIC
# Título um
## Título dois
### Título três
# MAGIC
Este é um teste do sistema de transmissão de emergência. Isto é apenas um teste.
# MAGIC
Este é um texto com uma palavra **em negrito**.
# MAGIC
Este é um texto com uma palavra *em itálico*.
# MAGIC
Esta é uma lista ordenada
1. um
1. dois
1. três
# MAGIC
Esta é uma lista não ordenada
* maçãs
* pêssegos
* bananas
# MAGIC
Links/HTML incorporado: <a href="https://en.wikipedia.org/wiki/Markdown" target="_blank">Markdown - Wikipédia</a>
# MAGIC
Imagens:
![Spark Engines](https://files.training.databricks.com/images/Apache-Spark-Logo_TM_200px.png)
# MAGIC
E, claro, tabelas:
# MAGIC
| nome   | valor |
|--------|-------|
| Yi     | 1     |
| Ali    | 2     |
| Selina | 3     |

<hr>--i18n-537e86bc-782f-4167-9899-edb3bd2b9e38
# MAGIC
### %run
* Você pode executar um notebook a partir de outro notebook usando o comando mágico **%run**
* Os notebooks a serem executados são especificados com caminhos relativos
* O notebook referenciado é executado como se fizesse parte do notebook atual; portanto, views temporárias e outras declarações locais estarão disponíveis no notebook que faz a chamada

<hr>--i18n-d5c27671-b3c8-4b8c-a559-40cf7988f92f
# MAGIC
Remover o comentário e executar a seguinte célula gerará o seguinte erro:<br/>
**`Error in SQL statement: AnalysisException: Table or view not found: demo_tmp_vw`**

<hr>--i18n-d0df4a17-abb4-42d3-ba37-c9a78f4fc9c0
# MAGIC
# MAGIC
Mas podemos declarar o comentário e algumas outras variáveis e funções executando esta célula:

<hr>--i18n-6a001755-5259-4fef-a5d3-2661d5301237
# MAGIC
# MAGIC
O notebook **`../Includes/Classroom-Setup-01.2`** que referenciamos inclui lógica para criar e **`USE`** um esquema, bem como criar a view temporária **`demo_temp_vw`**.
# MAGIC
Podemos confirmar se a view temporária agora está disponível na sessão atual do notebook com a query a seguir.

<hr>--i18n-c28ecc03-8919-488f-bce7-e2fc0a451870
# MAGIC
Usaremos este padrão de notebooks de "configuração" ao longo do curso para ajudar a definir o ambiente para lições e laboratórios.
# MAGIC
As variáveis, funções e outros objetos fornecidos devem ser facilmente identificáveis, pois fazem parte do objeto **`DA`**, que é uma instância de **`DBAcademyHelper`**.
# MAGIC
Com isso em mente, a maioria das lições usará variáveis derivadas do seu nome de usuário para organizar arquivos e esquemas. 
# MAGIC
Esse padrão permite evitar colisões com outros usuários em um workspace compartilhado.
# MAGIC
A célula abaixo usa Python para imprimir algumas das variáveis previamente definidas no script de configuração deste notebook:

<hr>--i18n-1145175f-c51e-4cf5-a4a5-b0e3290a73a2
# MAGIC
Além disso, essas mesmas variáveis são "injetadas" no contexto SQL para que possamos utilizá-las em instruções SQL.
# MAGIC
Falaremos mais sobre isso posteriormente, mas a célula a seguir mostra um exemplo rápido.
# MAGIC
<img src="https://files.training.databricks.com/images/icon_note_32.png"> Observe a diferença sutil, mas importante, na diferenciação de maiúsculas e minúsculas da palavra **`da`** e **`DA`** nestes dois exemplos.

<hr>--i18n-8330ad3c-8d48-42fa-9b6f-72e818426ed4
# MAGIC
## Utilitários do Databricks
Os notebooks do Databricks incluem um objeto **`dbutils`** que fornece vários comandos de utilitários para configurar e interagir com o ambiente: <a href="https://docs.databricks.com/user-guide/dev-tools/dbutils.html" target="_blank">documentação sobre dbutils</a>
# MAGIC
Ao longo deste curso, usaremos ocasionalmente **`dbutils.fs.ls()`** para listar diretórios de arquivos a partir de células Python.

<hr>--i18n-25eb75f8-6e75-41a6-a304-d140844fa3e6
# MAGIC
## display()
# MAGIC
A execução de queries SQL a partir de células sempre exibe resultados em formato tabular renderizado.
# MAGIC
Quando dados tabulares são retornados por uma célula Python, podemos chamar **`display`** para obter o mesmo tipo de visualização.
# MAGIC
Neste passo, agruparemos o comando list anterior no sistema de arquivos com **`display`**.

<hr>--i18n-42b624ff-8cc9-4318-bedc-e3b340cb1b81
# MAGIC
O comando **`display()`** tem os seguintes recursos e limitações:
* Limita a visualização dos resultados a 10.000 registros ou 2 MB, o que for menor
* Fornece um botão para download dos dados de resultados como CSV
* Permite renderizar gráficos

<hr>--i18n-c7f02dde-9b21-4edb-bded-5ce0eed56d03
# MAGIC
## Baixando notebooks
# MAGIC
Há várias opções para baixar notebooks individuais ou coleções de notebooks.
# MAGIC
A seguir, mostraremos como baixar este notebook, bem como uma coleção de todos os notebooks deste curso.
# MAGIC
### Baixar um notebook
# MAGIC
Passos:
* No canto superior esquerdo do notebook, clique na opção **Arquivo** 
* No menu mostrado, passe o mouse sobre ** Export** e selecione **Arquivo de origem**
# MAGIC
O notebook será baixado para o computador. O arquivo terá o nome do notebook atual e a extensão de arquivo da linguagem default. Você pode abrir esse arquivo com qualquer editor de arquivos e visualizar o conteúdo bruto do notebook do Databricks.
# MAGIC
Esses arquivos de origem podem ser carregados por upload em qualquer workspace do Databricks.
# MAGIC
### Baixar uma coleção de notebooks
# MAGIC
**OBSERVAÇÃO**: As instruções a seguir pressupõem que você importou os materiais usando **Repos**.
# MAGIC
Passos:
* Clique em ![](https://files.training.databricks.com/images/repos-icon.png) **Repos** na barra lateral esquerda
  * Essa ação deve mostrar os diretórios pais do notebook.
* No lado esquerdo da visualização do diretório, deve haver uma seta para a esquerda no meio da tela. Clique nessa seta para subir na hierarquia de arquivos.
* Você deverá ver um diretório chamado **Data Engineer Learning Path**. Clique na seta para baixo para abrir um menu.
* Nesse menu, passe o mouse sobre **Export** e selecione **Arquivo DBC**.
# MAGIC
O arquivo DBC (Databricks Cloud) baixado contém uma coleção compactada dos diretórios e notebooks deste curso. Esses arquivos DBC não devem ser editado localmente, mas podem ser carregados com segurança em qualquer workspace do Databricks quando o usuário deseja mover ou compartilhar o conteúdo do notebook.
# MAGIC
**OBSERVAÇÃO**: Quado você baixa uma coleção de DBCs, as visualizações de resultados e gráficos também são exportadas. No download dos notebooks de origem, só o código é salvo.

<hr>--i18n-30e63e01-ca85-461a-b980-ea401904731f
# MAGIC
## Aprendendo mais
# MAGIC
Encorajamos você a explorar a documentação para saber mais sobre os vários recursos da plataforma e dos notebooks do Databricks.
* <a href="https://docs.databricks.com/user-guide/index.html#user-guide" target="_blank">Guia do usuário</a>
* <a href="https://docs.databricks.com/user-guide/getting-started.html" target="_blank">Introdução ao Databricks</a>
* <a href="https://docs.databricks.com/user-guide/notebooks/index.html" target="_blank">Guia do usuário/notebooks</a>
* <a href="https://docs.databricks.com/notebooks/notebooks-manage.html#notebook-external-formats" target="_blank">Importando notebooks/formatos suportados</a>
* <a href="https://docs.databricks.com/repos/index.html" target="_blank">Repos</a>
* <a href="https://docs.databricks.com/administration-guide/index.html#administration-guide" target="_blank">Guia de administração</a>
* <a href="https://docs.databricks.com/user-guide/clusters/index.html" target="_blank">Configuração de clusters</a>
* <a href="https://docs.databricks.com/api/latest/index.html#rest-api-2-0" target="_blank">API REST</a>
* <a href="https://docs.databricks.com/release-notes/index.html#release-notes" target="_blank">Notas sobre a versão</a>

<hr>--i18n-9987fd58-1023-4dbd-8319-40332f909181
# MAGIC
## Mais uma observação! 
# MAGIC
Ao final de cada lição, você verá o comando **`DA.cleanup()`**.
# MAGIC
Esse método descarta esquemas e diretórios de trabalho específicos da lição para tentar manter o workspace limpo e preservar a estabilidade da lição.
# MAGIC
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

