# /DE 4 - Delta Live Tables/DE 4.1B - Pipelines Python/DE 4.1.3 - Status Pipeline
<hr>--i18n-17f3cfcf-2ad7-48f2-bf4e-c2adcb372926
# Solução de problemas da sintaxe Python do DLT
# MAGIC
Agora que passamos pelo processo de configuração e execução de um pipeline com 2 notebooks, simularemos o desenvolvimento e a adição de um terceiro notebook.
# MAGIC
**NÃO ENTRE EM PÂNICO!**
# MAGIC
O código fornecido abaixo contém alguns pequenos erros de sintaxe intencionais. Ao solucionar esses erros, você aprenderá como desenvolver código DLT de forma iterativa e identificar erros em sua sintaxe.
# MAGIC
Esta lição não pretende fornecer uma solução robusta para desenvolvimento e teste de código; em vez disso, seu objetivo é ajudar os usuários a começar a usar DLT e a lidar com uma sintaxe desconhecida.
# MAGIC
## Objetivos de aprendizado
Ao final desta lição, os alunos deverão se sentir confortáveis:
*Identificando e solucionando problemas de sintaxe DLT 
*Desenvolvimento iterativo de pipelines DLT com notebooks

<hr>--i18n-b6eb7861-af09-4009-a272-1c5c91f87a8b
## Adicione este notebook a um pipeline DLT
# MAGIC
Neste ponto do curso, você deverá ter um pipeline do DLT configurado com duas bibliotecas de notebook.
# MAGIC
Você deve ter processado vários lotes de registros por meio desse pipeline e entender como disparar uma nova execução do pipeline e adicionar uma biblioteca adicional.
# MAGIC
Para começar esta lição, siga o processo de adição deste notebook ao seu pipeline usando a UI DLT e, em seguida, acione uma atualização.
# MAGIC
<img src="https://files.training.databricks.com/images/icon_hint_24.png">O link para este notebook pode ser encontrado em [DE 4.1- Passo a passo da UI DLT]($../DE 4.1- DLT UI Walkthrough)<br/>
nas instruções impressas da **Tarefa 3** na seção **Gerar a configuração do pipeline**

<hr>--i18n-fb7a717a-7921-45c3-bb1d-4c2e1bff55ab
## Solução de erros
# MAGIC
Cada uma das três funções abaixo contém um erro de sintaxe, e cada erro será detectado e relatado de maneira ligeiramente diferente pelo DLT.
# MAGIC
Alguns erros de sintaxe serão detectados durante o** Inicializando** estágio, pois o DLT não é capaz de analisar corretamente os comandos.
# MAGIC
Outros erros de sintaxe são detectados durante o estágio de **configuração de tabelas**.
# MAGIC
Observe que, devido à maneira como o DLT resolve a ordem das tabelas no pipeline em etapas diferentes, às vezes você pode ver erros gerados primeiro em estágios posteriores.
# MAGIC
Uma abordagem que pode funcionar bem é corrigir uma tabela por vez, começando no conjunto de dados mais antigo e trabalhando até o final. O código comentado será ignorado automaticamente, para que você possa remover com segurança o código de uma execução de desenvolvimento sem removê-lo completamente.
# MAGIC
Mesmo que você consiga identificar imediatamente os erros no código abaixo, tente usar as mensagens de erro da UI para orientar a identificação desses erros. O código da solução segue na célula abaixo.

<hr>--i18n-5bb5f3ef-7a9e-4ea5-a6c3-f85cac306e04
## Soluções
# MAGIC
A sintaxe correta para cada uma das funções acima é fornecida em um bloco de notas com o mesmo nome na pasta Soluções.
# MAGIC
Há várias opções para lidar com esses erros:
*Resolva cada problema, resolvendo você mesmo os problemas acima
*Copie e cole a solução no**`# ANSWER`** célula do bloco de notas Soluções de mesmo nome
*Atualize seu pipline para usar diretamente o bloco de notas de soluções de mesmo nome
# MAGIC
**OBSERVAÇÃO**: Você não poderá ver nenhum outro erro até adicionar a instrução **`import dlt`** à célula acima.
# MAGIC
Os problemas em cada consulta:
1. O decorador **`@dlt.table`** está faltando antes da definição da função
1. O argumento de palavra-chave correto para fornecer um nome de tabela personalizado é **`name`**, e não **`table_name`**
1. Para ler uma tabela no pipeline, use **`dlt.read`**, e não **`spark.read`**

<hr>--i18n-f8fb12d5-c515-43fc-ab55-0d1f97baf05c
## Resumo
# MAGIC
Ao revisar este caderno, você agora deve se sentir confortável:
*Identificando e solucionando problemas de sintaxe DLT 
*Desenvolvimento iterativo de pipelines DLT com notebooks
