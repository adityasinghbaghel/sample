# /DE 2 - ETL with Spark/DE 2.4 - Cleaning Data
<hr>--i18n-2ad42144-605b-486f-ad65-ca24b47b1924
-- MAGIC
 
# Limpando dados
-- MAGIC
À medida que inspecionamos e limpamos os dados, precisaremos construir várias expressões de coluna e queries para expressar transformações a serem aplicadas no dataset.
-- MAGIC
As expressões de coluna são construídas a partir de colunas, operadores e funções integradas existentes. Elas podem ser usadas em instruções **`SELECT`** para expressar transformações que criam novas colunas.
-- MAGIC
Há vários comandos de consulta SQL padrão (por exemplo, **`DISTINCT`**, **`WHERE`**, **`GROUP BY`** etc.) disponíveis no Spark SQL para expressar transformações.
-- MAGIC
Neste notebook, revisaremos alguns conceitos que podem diferir de outros sistemas que você conhece, além de destacar algumas funções úteis para operações comuns.
-- MAGIC
Prestaremos atenção especial aos comportamentos em torno de valores **`NULL`**, bem como à formatação de strings e campos de data e hora.
-- MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
- Resumir datasets e descrever comportamentos null
- Recuperar e remover duplicatas
- Validar datasets para verificar contagens esperadas, valores ausentes e registros duplicados
- Aplicar transformações comuns para limpar e transformar dados

<hr>--i18n-2a604768-1aac-40e2-8396-1e15de60cc96
-- MAGIC
-- MAGIC
## Executar a configuração
-- MAGIC
O script de configuração criará os dados e declarará os valores necessários para a execução do restante deste notebook.

<hr>--i18n-31202e20-c326-4fa0-8892-ab9308b4b6f0
## Visão geral dos dados
-- MAGIC
Trabalharemos com registros de novos usuários da tabela **`users_dirty`**, que tem o seguinte esquema:
-- MAGIC
| campo | tipo | descrição |
|---|---|---|
| user_id | string | identificador exclusivo |
| user_first_touch_timestamp | long | hora em que o registro do usuário foi criado em microssegundos começando pela época |
| email | string | endereço de email mais recente fornecido pelo usuário para concluir uma ação |
| updated | timestamp | hora em que este registro foi atualizado pela última vez |
-- MAGIC
Vamos começar contando os valores em cada campo dos dados.

<hr>--i18n-c414c24e-3b72-474b-810d-c3df32032c26
-- MAGIC
## Inspecionar dados ausentes
-- MAGIC
As contagens acima indicam que há alguns valores nulos em todos os campos.
-- MAGIC
**OBSERVAÇÃO:** Valores nulos se comportam incorretamente em algumas funções matemáticas, incluindo **`count()`**.
-- MAGIC
A função - **`count(col)`** ignora valores **`NULL`** ao contar colunas ou expressões específicas.
- **`count(*)`** é um caso especial que conta o número total de linhas (incluindo linhas que são apenas valores **`NULL`**).
-- MAGIC
Podemos contar valores nulos em um campo filtrando os registros onde o campo é nulo usando:  
**`count_if(col IS NULL)`** ou **`count(*)`** com um filtro em que **`col IS NULL`**. 
-- MAGIC
Ambas as instruções abaixo contam corretamente os registros com emails ausentes.

<hr>--i18n-ea1ca35c-6421-472b-b70b-4f36bdab6d79
 
## Desduplicar linhas
Podemos usar **`DISTINCT *`** para remover registros duplicados verdadeiros onde linhas inteiras contêm os mesmos valores.

<hr>--i18n-5da6599b-756c-4d22-85cd-114ff02fc19d
-- MAGIC
  
## Desduplicar linhas com base em colunas específicas
-- MAGIC
O código abaixo usa **`GROUP BY`** para remover registros duplicados com base nos valores de coluna **`user_id`** e **`user_first_touch_timestamp`**. (Lembre-se de que esses campos são gerados quando um determinado usuário é encontrado pela primeira vez, formando assim tuplas exclusivas.)
-- MAGIC
No passo a seguir, usamos a função agregada **`max`** como uma solução alternativa para:
- Manter os valores das colunas **`email`** e **`updated`** no resultado do agrupamento
- Capturar emails não nulos quando houver múltiplos registros presentes

<hr>--i18n-5e2c98db-ea2d-44dc-b2ae-680dfd85c74b
-- MAGIC
Vamos confirmar se temos a contagem esperada de registros restantes após a desduplicação com base em valores **`user_id`** e **`user_first_touch_timestamp`** distintos.

<hr>--i18n-776b4ee7-9f29-4a19-89da-1872a1f8cafa
-- MAGIC
## Validar datasets
Com base na revisão manual acima, confirmamos visualmente que temos as contagens esperadas.
 
Também podemos fazer validações programáticas usando filtros simples e cláusulas **`WHERE`**.
-- MAGIC
Valide que o **`user_id`** em cada linha é único.

<hr>--i18n-d405e7cd-9add-44e3-976a-e56b8cdf9d83
-- MAGIC
-- MAGIC
-- MAGIC
Confirme que cada email está associado a no máximo um **`user_id`**.

<hr>--i18n-8630c04d-0752-404f-bfd1-bb96f7b06ffa
-- MAGIC
 
## Formato de data e regex
Agora que removemos os campos nulos e eliminamos as duplicatas, podemos extrair mais valor dos dados.
-- MAGIC
O código abaixo:
- Dimensiona e converte corretamente **`user_first_touch_timestamp`** em um carimbo de data/hora válido
- Extrai a data do calendário e a hora do relógio para o carimbo de data/hora em um formato legível por humanos
- Usa **`regexp_extract`** para extrair os domínios da coluna de email usando regex

<hr>--i18n-c9e02918-f105-4c12-b553-3897fa7387cc
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

