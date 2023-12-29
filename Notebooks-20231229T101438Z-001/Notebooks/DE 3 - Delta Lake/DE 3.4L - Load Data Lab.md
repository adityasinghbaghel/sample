# /DE 3 - Delta Lake/DE 3.4L - Load Data Lab
<hr>--i18n-02080b66-d11c-47fb-a096-38ce02af4dbb
-- MAGIC
-- MAGIC
-- MAGIC
# Laboratório de carregamento de dados
-- MAGIC
Neste laboratório, você carregará dados em tabelas Delta novas e existentes.
-- MAGIC
## Objetivos de aprendizado
Ao final deste laboratório, você será capaz de:
- Criar uma tabela Delta vazia com um esquema fornecido
- Inserir registros de uma tabela existente em uma tabela Delta
- Usar uma instrução CTAS para criar uma tabela Delta a partir de arquivos

<hr>--i18n-50357195-09c5-4ab4-9d60-ca7fd44feecc
-- MAGIC
-- MAGIC
-- MAGIC
## Executar a configuração
-- MAGIC
Execute a célula a seguir para configurar variáveis ​​e conjuntos de dados para esta lição.

<hr>--i18n-5b20c9b2-6658-4536-b79b-171d984b3b1e
-- MAGIC
-- MAGIC
-- MAGIC
## Visão geral dos dados
-- MAGIC
Trabalharemos com uma amostra de dados brutos do Kafka escritos como arquivos JSON. 
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

<hr>--i18n-5140f012-898a-43ac-bed9-b7e01a916505
-- MAGIC
## Definir o esquema para uma tabela Delta vazia
Crie uma tabela Delta gerenciada vazia chamada **`events_raw`** usando o mesmo esquema.

<hr>--i18n-70f4dbf1-f4cb-4ec6-925a-a939cbc71bd5
-- MAGIC
-- MAGIC
-- MAGIC
-- MAGIC
Execute a célula abaixo para confirmar se a tabela foi criada corretamente.

<hr>--i18n-7b4e55f2-737f-4996-a51b-4f18c2fc6eb7
-- MAGIC
-- MAGIC
## Inserir eventos brutos na tabela Delta
-- MAGIC
Assim que os dados extraídos e a tabela Delta estiverem prontos, insira os registros JSON da tabela **`events_json`** na nova tabela Delta **`events_raw`**.

<hr>--i18n-65ffce96-d821-4792-b545-5725814003a0
-- MAGIC
-- MAGIC
-- MAGIC
Revise manualmente o conteúdo da tabela para conferir se os dados foram gravados conforme o esperado.

<hr>--i18n-1cbff40d-e916-487c-958f-2eb7807c5aeb
-- MAGIC
-- MAGIC
-- MAGIC
-- MAGIC
Execute a célula abaixo para confirmar que os dados foram carregados corretamente.

<hr>--i18n-b3c62fea-b75d-41d6-8214-660f9cfa3acd
-- MAGIC
-- MAGIC
## Criar uma tabela Delta a partir dos resultados da query
-- MAGIC
Além dos novos dados de eventos, também carregaremos uma pequena tabela de pesquisa com detalhes de produtos que usaremos posteriormente no curso.
Use uma instrução CTAS para criar uma tabela Delta gerenciada chamada **`item_lookup`** que extrai dados do diretório Parquet fornecido abaixo. 

<hr>--i18n-5a971532-0003-4665-9064-26196cd31e89
-- MAGIC
-- MAGIC
-- MAGIC
-- MAGIC
Execute a célula abaixo para confirmar que a tabela de pesquisa foi carregada corretamente.

<hr>--i18n-4db73493-3920-44e2-a19b-f335aa650f76
-- MAGIC
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

