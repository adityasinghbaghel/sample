# /DE 1 - Databricks Workspace/DE 1.3L - Laboratório de introdução à plataforma Databricks
<hr>--i18n-4212c527-b1c1-4cce-b629-b5ffb5c57d68
# MAGIC
# Introdução à plataforma Databricks
# MAGIC
Este notebook fornece uma revisão prática de algumas funcionalidades básicas do workspace de ciência de dados e engenharia do Databricks.
# MAGIC
## Objetivos de aprendizado
Ao final deste laboratório, você deverá ser capaz de:
- Renomear um notebook e alterar a linguagem default
- Anexar um cluster
- Usar o comando mágico **`%run`**
- Executar células Python e SQL
- Criar uma célula Markdown

<hr>--i18n-eb166a05-9a22-4a74-b54e-3f9e5779f342
# MAGIC
## Renomeando um notebook
# MAGIC
Alterar o nome de um notebook é fácil. Clique no nome na parte superior desta página e altere-o. Para facilitar a navegação de volta ao notebook, caso precise, acrescente uma string de teste curta ao final do nome existente.

<hr>--i18n-a975b60c-9871-4736-9b4f-194577d730f0
# MAGIC
## Anexando um cluster
# MAGIC
A execução de células em um notebook requer recursos computacionais, que são fornecidos por clusters. Ao executar uma célula em um notebook pela primeira vez, você verá uma solicitação de anexar o notebook a um cluster, caso isso ainda não tenha sido feito.
# MAGIC
Anexe um cluster ao notebook neste momento clicando no menu dropdown próximo ao canto superior direito da página. Selecione o cluster que você criou anteriormente. Essa ação limpará o estado de execução do notebook e o conectará ao cluster selecionado.
# MAGIC
Observe que o menu dropdown oferece a opção de iniciar ou reiniciar o cluster como necessário. Você também pode desanexar e reanexar o notebook a um cluster em uma única operação. Essa ação é útil para limpar o estado de execução quando necessário.

<hr>--i18n-4cd4b089-e782-4e81-9b88-5c0abd02d03f
# MAGIC
## Usando %run
# MAGIC
Projetos complexos de qualquer tipo podem ser divididos em componentes mais simples e reutilizáveis.
# MAGIC
No contexto dos notebooks do Databricks, esse recurso é fornecido por meio do comando mágico **`%run`**.
# MAGIC
Quando usados dessa forma, variáveis, funções e blocos de código tornam-se parte do contexto de programação atual.
# MAGIC
Considere este exemplo:
# MAGIC
O **`Notebook_A`** tem quatro comandos:
  1. **`name = "John"`**
  2. **`print(f"Hello {name}")`**
  3. **`%run ./Notebook_B`**
  4. **`print(f"Welcome back {full_name}`**
# MAGIC
O **`Notebook_B`** tem apenas um comando:
  1. **`full_name = f"{name} Doe"`**
# MAGIC
Executar o **`Notebook_B`** falhará porque a variável **`name`** não está definida no **`Notebook_B`**.
# MAGIC
Da mesma forma, poderíamos achar que o **`Notebook_A`** falharia porque usa a variável **`full_name`** que também não está definida no **`Notebook_A`**, mas isso não acontece.
# MAGIC
O que realmente acontece é que os dois notebooks são mesclados como mostrado abaixo, sendo **posteriormente** executados:
1. **`name = "John"`**
2. **`print(f"Hello {name}")`**
3. **`full_name = f"{name} Doe"`**
4. **`print(f"Welcome back {full_name}")`**
# MAGIC
Isso resulta no comportamento esperado:
* **`Hello John`**
* **`Welcome back John Doe`**

<hr>--i18n-40ca42ab-4275-4d92-b151-995429e54486
# MAGIC
A pasta que contém este notebook inclui uma subpasta chamada **`ExampleSetupFolder`**, que por sua vez contém um notebook chamado **`example-setup`**.
# MAGIC
Esse notebook simples faz o seguinte:
  - Declara a variável **`my_name`** e a configura como **`None`**
  - Cria o DataFrame **`example_df`** e a tabela **`nyc_taxi`** (que usaremos mais tarde)
# MAGIC
Execute a célula a seguir para executar o notebook.
# MAGIC
Em seguida, abra o notebook **`example-setup`** e altere a variável **`my_name`** de **`None`** para o seu nome (ou o nome de qualquer pessoa) entre aspas. Se feito corretamente, as duas células a seguir deverão ser executadas sem gerar um **`AssertionError`**.
# MAGIC
<img src="https://files.training.databricks.com/images/icon_note_24.png"> Você verá referências adicionais a **`_utility-methods`** e **`DBAcademyHelper`** que são usadas neste material didático de configuração e devem ser ignoradas neste exercício.

<hr>--i18n-e5ef8dff-bfa6-4f9e-8ad3-d5ef322b978d
# MAGIC
## Executar uma célula Python
# MAGIC
Execute a célula a seguir para verificar se o notebook **`example-setup`** foi executado exibindo o DataFrame **`example_df`**. Essa tabela consiste em 16 linhas de valores crescentes.

<hr>--i18n-6cb46bcc-9797-4782-931c-a7b8350146b2
# MAGIC
# MAGIC
## Mudar a linguagem
# MAGIC
Observe que a linguagem default deste notebook está definida como Python. Para usar outra linguagem, clique no botão **Python** à direita do nome do notebook e altere a linguagem default para SQL.
# MAGIC
Observe que as células Python recebem automaticamente o comando mágico <strong><code>&#37;python</code></strong> como prefixo para que sua validade seja mantida. 
# MAGIC
Observe que essa operação também limpa o estado de execução. Depois de alterar a linguagem default do notebook, execute novamente o script de configuração de exemplo acima para continuar.

<hr>--i18n-478faa69-6814-4725-803b-3414a1a803ae
# MAGIC
## Criar uma célula Markdown
# MAGIC
Adicione uma célula abaixo da atual. Preencha-a com um Markdown que inclua pelo menos os seguintes elementos:
* Um cabeçalho
* Tópicos
* Um link (usando as convenções de HTML ou Markdown de sua escolha)

<hr>--i18n-55b2a6c6-2fc6-4c57-8d6d-94bba244d86e
# MAGIC
## Executar uma célula SQL
# MAGIC
Execute a célula a seguir para consultar uma tabela Delta usando SQL. Essa ação executa uma query simples em uma tabela com suporte de um dataset de exemplo fornecido pelo Databricks e incluído em todas as instalações do DBFS.

<hr>--i18n-9993ed50-d8cf-4f37-bc76-6b18789447d6
# MAGIC
Execute a célula a seguir para visualizar os arquivos subjacentes que dão suporte a esta tabela.

<hr>--i18n-c31a3318-a114-46e8-a744-18e8f8aa071e
# MAGIC
## Limpando o estado do notebook
# MAGIC
Às vezes é útil limpar todas as variáveis definidas no notebook e começar do zero.  Essa ação pode ajudar quando você quer testar células isoladamente ou apenas porque deseja redefinir o estado de execução.
# MAGIC
Acesse o menu **Executar** e selecione a opção **Limpar estado e saídas**.
# MAGIC
Agora tente executar a célula abaixo e observe que as variáveis definidas anteriormente não estão mais presentes. Elas serão redefinidas quando você executar novamente as células anteriores acima.

<hr>--i18n-9947d429-2c10-4047-811f-3f5128527c6d
# MAGIC
## Encerrando
# MAGIC
Após concluir este laboratório, você deverá ser capaz de manipular notebooks, criar novas células e executar notebooks dentro de notebooks com confiança.
