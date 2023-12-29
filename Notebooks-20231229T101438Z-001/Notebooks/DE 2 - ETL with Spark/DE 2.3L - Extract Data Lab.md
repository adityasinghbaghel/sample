# /DE 2 - ETL with Spark/DE 2.3L - Extract Data Lab
<hr>--i18n-037a5204-a995-4945-b6ed-7207b636818c
-- MAGIC
-- MAGIC
-- MAGIC
# Laboratório de extração de dados
-- MAGIC
Neste laboratório, você extrairá dados brutos de arquivos JSON.
-- MAGIC
## Objetivos de aprendizado
Ao final deste laboratório, você será capaz de:
- Registrar uma tabela externa para extrair dados de arquivos JSON

<hr>--i18n-3b401203-34ae-4de5-a706-0dbbd5c987b7
-- MAGIC
-- MAGIC
-- MAGIC
## Executar a configuração
-- MAGIC
Execute a célula a seguir para configurar variáveis e datasets para esta lição.

<hr>--i18n-9adc06e2-7298-4306-a062-7ff70adb8032
-- MAGIC
-- MAGIC
-- MAGIC
## Visão geral dos dados
-- MAGIC
Trabalharemos com uma amostra de dados brutos do Kafka gravados em arquivos JSON. 
-- MAGIC
Cada arquivo contém todos os registros consumidos durante 5- segundo intervalo, armazenado com o esquema Kafka completo como um múltiplo- gravar arquivo JSON. 
-- MAGIC
O esquema da tabela:
-- MAGIC
| campo  | tipo | descrição |
| ------ | ---- | ----------- |
| key    | BINARY | O**`user_id`** campo é usado como chave; este é um campo alfanumérico exclusivo que corresponde às informações da sessão/cookie |
| offset | LONG | Este é um valor único, aumentando monotonicamente para cada partição |
| partition | INTEGER | Nossa implementação atual do Kafka usa apenas 2 partições (0 e 1) |
| timestamp | LONG    | Este carimbo de data/hora é registrado em milissegundos desde a época e representa o momento em que o produtor anexa um registro a uma partição |
| topic | STRING | Embora o serviço Kafka hospede vários tópicos, apenas os registros do**`clickstream`** o tópico está incluído aqui |
| value | BINARY | Esta é a carga completa de dados (a ser discutida mais tarde), enviada como JSON |

<hr>--i18n-8cca978a-3034-4339-8e6e-6c48d389cce7
-- MAGIC
-- MAGIC
 
## Extrair eventos brutos de arquivos JSON
Para carregar corretamente os dados no Delta, primeiro precisamos extrair os dados JSON usando o esquema correto.
-- MAGIC
Crie uma tabela externa a partir de arquivos JSON localizados no caminho de arquivo fornecido abaixo. Nomeie essa tabela **`events_json`** e declare o esquema acima.

<hr>--i18n-33231985-3ff1-4f44-8098-b7b862117689
-- MAGIC
-- MAGIC
-- MAGIC
**OBSERVAÇÃO**: Usaremos Python para executar verificações ocasionais durante o laboratório. A célula a seguir retornará um erro com uma mensagem informando o que precisa ser alterado caso você não tenha seguido as instruções. A execução da célula não gerará nenhuma saída se você tiver concluído este passo.

<hr>--i18n-6919d58a-89e4-4c02-812c-98a15bb6f239
-- MAGIC
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

