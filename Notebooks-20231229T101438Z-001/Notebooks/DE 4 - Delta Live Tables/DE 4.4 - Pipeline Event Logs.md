# /DE 4 - Delta Live Tables/DE 4.4 - Pipeline Event Logs
<hr>--i18n-fea707eb-314a-41a8-8da5-fdac27ebe622
# Explorando os logs de eventos do pipeline
# MAGIC
O DLT usa os logs de eventos para armazenar várias informações importantes usadas para gerenciar, relatar e entender o que acontece durante a execução do pipeline.
# MAGIC
Abaixo fornecemos uma série de queries úteis para explorar o log de eventos e obter mais informações sobre os pipelines do DLT.

<hr>--i18n-db58d66a-73bf-412a-ae17-b00f98338f56
## Consultar o log de eventos
O log de eventos é gerenciado como uma tabela do Delta Lake com alguns dos campos mais importantes armazenados como dados JSON aninhados.
# MAGIC
A query abaixo mostra como é simples ler essa tabela e criar um DataFrame e uma view temporária para queries interativas.

<hr>--i18n-b5f6dcac-b958-4809-9942-d45e475b6fb7
## Definir o ID da última atualização
# MAGIC
Em muitos casos, você pode querer saber qual foi a atualização mais recente (ou as últimas n atualizações) do pipeline.
# MAGIC
Podemos capturar facilmente o ID da atualização mais recente com uma query SQL.

<hr>--i18n-de7c7817-fcfd-4994-beb0-704099bd5c30
## Executar registro em log de auditoria
# MAGIC
Os eventos relacionados à execução de pipelines e edição de configurações são capturados como **`user_action`**.
# MAGIC
O seu nome deve ser o único **`user_name`** no pipeline que você configurou durante esta lição.

<hr>--i18n-887a16ce-e1a5-4d27-bacb-7e6c84cbaf37
## Examinar linhagem
# MAGIC
O DLT fornece informações integradas de linhagem sobre o fluxo de dados na tabela.
# MAGIC
Embora a query abaixo indique apenas os predecessores diretos de cada tabela, essas informações podem ser facilmente combinadas para rastrear dados em qualquer tabela até o momento de entrada no lakehouse.

<hr>--i18n-1b1c0687-163f-4684-a570-3cf4cc32c272
## Examinar as métricas de qualidade de dados
# MAGIC
Por fim, as métricas de qualidade de dados podem ser extremamente úteis para gerar percepções de longo e curto prazo sobre seus dados.
# MAGIC
No passo abaixo, capturamos as métricas de cada restrição ao longo de toda a vida útil da tabela.

