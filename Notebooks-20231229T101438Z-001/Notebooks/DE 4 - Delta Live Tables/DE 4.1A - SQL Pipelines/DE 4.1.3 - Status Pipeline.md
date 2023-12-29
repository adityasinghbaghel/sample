# /DE 4 - Delta Live Tables/DE 4.1A - Pipelines SQL/DE 4.1.3 - Status Pipeline
<hr>--i18n-54a6a2f6-5787-4d60-aea6-e39627954c96
# Solução de problemas da sintaxe SQL do DLT
-- MAGIC
Agora que concluímos o processo de configuração e execução de um pipeline com dois notebooks, simularemos o desenvolvimento e a adição de um terceiro notebook.
-- MAGIC
**NÃO ENTRE EM PÂNICO!** Encontraremos alguns problemas.
-- MAGIC
O código fornecido abaixo contém pequenos erros intencionais de sintaxe. Ao solucionar esses erros, você aprenderá como desenvolver código DLT de forma iterativa e identificar erros de sintaxe.
-- MAGIC
Esta lição não pretende fornecer uma solução robusta para desenvolvimento e teste de código; seu objetivo é ajudar os usuários a começar a usar o DLT e a lidar com uma sintaxe desconhecida.
-- MAGIC
## Objetivos de aprendizado
Ao final desta lição, os alunos deverão se sentir confortáveis:
* Identificar e solucionar problemas de sintaxe do DLT 
* Desenvolver iterativamente pipelines do DLT com notebooks

<hr>--i18n-dee15416-56fd-48d7-ae3c-126175503a9b
## Adicionar este notebook a um pipeline do DLT
-- MAGIC
Neste ponto do curso, você deve ter um pipeline do DLT configurado com duas bibliotecas de notebook.
-- MAGIC
Você provavelmente processou vários lotes de registros por esse pipeline e deve saber como acionar uma nova execução do pipeline e adicionar uma biblioteca.
-- MAGIC
Para começar a lição, siga o processo de adição do notebook ao pipeline usando a UI do DLT e, em seguida, acione uma atualização.
-- MAGIC
<img src="https://files.training.databricks.com/images/icon_hint_24.png">O link para este notebook pode ser encontrado em [DE 4.1- Passo a passo da UI DLT]($../DE 4.1- DLT UI Walkthrough)<br/>
nas instruções impressas da **Tarefa 3** na seção **Gerar a configuração do pipeline**

<hr>--i18n-96beb691-44d9-4871-9fa5-fcccc3e13616
## Solução de erros
-- MAGIC
Cada uma das três queries abaixo contém um erro de sintaxe, e cada erro será detectado e relatado de maneira ligeiramente diferente pelo DLT.
-- MAGIC
Alguns erros de sintaxe são detectados durante o estágio de **inicialização**, quando o DLT não consegue analisar corretamente os comandos.
-- MAGIC
Outros erros de sintaxe são detectados durante o estágio de **configuração de tabelas**.
-- MAGIC
Observe que, devido à maneira como o DLT resolve a ordem das tabelas no pipeline em passos diferentes, às vezes os erros podem ser gerados primeiro em estágios posteriores.
-- MAGIC
Uma abordagem que pode funcionar é corrigir uma tabela por vez, começando pelo dataset mais antigo e continuando até o final. Comentários no código serão ignorados automaticamente. Assim, você pode comentar o código em uma execução de desenvolvimento com segurança sem removê-lo completamente.
-- MAGIC
Mesmo que você consiga identificar imediatamente os erros no código abaixo, tente usar as mensagens de erro da UI como um guia. O código da solução é mostrado na célula abaixo.

<hr>--i18n-492901e2-38fe-4a8c-a05d-87b9dedd775f
## Soluções
-- MAGIC
A sintaxe correta para cada uma das funções acima é fornecida em um notebook com o mesmo nome na pasta Solutions.
-- MAGIC
Há várias opções para lidar com esses erros:
* Revise cada item, corrigindo você mesmo os problemas acima
* Copie e cole a solução na célula **`# ANSWER`** do notebook de soluções com o mesmo nome
* Atualize o pipeline para usar diretamente o notebook de soluções com o mesmo nome
-- MAGIC
Os problemas em cada query:
1. A palavra-chave **`LIVE`** está faltando na instrução create
1. A palavra-chave **`STREAM`** está faltando na cláusula from
1. A palavra-chave **`LIVE`** está faltando na tabela referenciada pela cláusula from

<hr>--i18n-54e251d6-b8f8-45c2-82df-60e22b127135
## Resumo
-- MAGIC
Ao revisar este caderno, você agora deve se sentir confortável:
*Identificando e solucionando problemas de sintaxe DLT 
*Desenvolvimento iterativo de pipelines DLT com notebooks
