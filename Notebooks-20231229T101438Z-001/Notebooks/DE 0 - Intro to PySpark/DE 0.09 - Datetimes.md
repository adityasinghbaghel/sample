# /DE 0 - Introdução ao PySpark/DE 0.09 - Data e hora
<hr>--i18n-0eeddecf-4f2c-4599-960f-8fefe777281f
# Funções de data e hora
# MAGIC
##### Objetivos
1. Converter para timestamp
2. Formatar data e hora
3. Extrair de timestamps
4. Converter para data
5. Manipular data e hora
# MAGIC
##### Métodos
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/column.html" target="_blank">Coluna</a>: **`cast`**
- <a href="https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/functions.html#datetime-functions" target="_blank">Funções integradas</a>: **`date_format`**, **`to_date`**, **`date_add`**, **`year`**, **`month`**, **`dayofweek`**, **`minute`**, **`second`**

<hr>--i18n-6d2e9c6a-0561-4426-ae88-a8ebca06c61b
# MAGIC
Vamos usar um subconjunto do dataset de eventos BedBricks para praticar o uso do recurso de data e hora.

<hr>--i18n-34540d5e-7a9b-496d-b9d7-1cf7de580f23
# MAGIC
### Funções integradas: Funções de data e hora
A seguir, são listadas algumas funções integradas usadas para manipular datas e horas no Spark.
# MAGIC
| Método | Descrição |
| --- | --- |
| **`add_months`** | Retorna a data em numMonths após startDate |
| **`current_timestamp`** | Retorna o timestamp no início da avaliação da consulta como uma coluna de timestamp |
| **`date_format`** | Converte uma data/timestamp/string em um valor de string no formato de data determinado pelo segundo argumento. |
| **`dayofweek`** | Extrai o dia do mês como um número inteiro de uma determinada data/timestamp/string |
| **`from_unixtime`** | Converte o número de segundos do epoch unix (1970-01-01 00:00:00 UTC) em uma string que representa o timestamp daquele momento no fuso horário atual do sistema no formato aaaa-MM-dd HH:mm:ss |
| **`minute`** | Extrai os minutos como um número inteiro de uma determinada data/timestamp/string |
| **`unix_timestamp`** | Converte uma string de hora com um determinado padrão em um timestamp Unix (em segundos) |

<hr>--i18n-fa5f62a7-e690-48c8-afa0-b446d3bc7aa6
# MAGIC
### Converter para timestamp
# MAGIC
#### **`cast()`**
Converte a coluna em um tipo de dado diferente, especificado usando representação de string ou DataType.

<hr>--i18n-6c9cb2b0-ef18-48c4-b1ed-3fad453172c1
# MAGIC
### Data e hora
# MAGIC
Há vários cenários comuns no uso de data e hora no Spark:
# MAGIC
- As fontes de dados CSV/JSON usam a string padrão para analisar e formatar o conteúdo de data e hora.
- Funções de data e hora convertem StringType em/de DateType ou TimestampType, por exemplo, **`unix_timestamp`**, **`date_format`**, **`from_unixtime`**, **`to_date`**, **`to_timestamp`** etc.
# MAGIC
#### Padrões de data e hora para formatação e análise
O Spark usa <a href="https://spark.apache.org/docs/latest/sql-ref-datetime-pattern.html" target="_blank">letras padrão para análise e formatação de datas e timestamps</a>. Um subconjunto desses padrões é mostrado abaixo.
# MAGIC
| Símbolo | Significado         | Apresentação | Exemplos               |
| ------ | --------------- | ------------ | ---------------------- |
| G      | era             | texto         | d.C.; depois de Cristo        |
| y      | ano            | ano         | 2020; 20               |
| D      | dia do ano     | número (3)    | 189                    |
| M/L    | mês do ano   | mês        | 7; 07; Jul; Julho       |
| d      | dia do mês    | número (3)    | 28                     |
| Q/q    | trimestre do ano | número/texto  | 3T; 3º trimestre |
| E      | dia da semana     | texto         | Ter; Terça-feira           |
# MAGIC
<img src="https://files.training.databricks.com/images/icon_warn_32.png" alt="Warning"> O tratamento de datas e timestamps do Spark mudou na versão 3.0, e os padrões usados para analisar e formatar esses valores também mudaram. Leia uma análise dessas mudanças <a href="https://databricks.com/blog/2020/07/22/a-comprehensive-look-at-dates-and-timestamps-in-apache-spark-3-0.html" target="_blank">nesta postagem no blog da Databricks</a>. 

<hr>--i18n-6bc9e089-fc58-4d8f-b118-d5162b747dc6
# MAGIC
#### Formatar data
# MAGIC
#### **`date_format()`**
Converte uma data/timemstamp/string em uma string formatada no padrão de data e hora determinado.

<hr>--i18n-adc065e9-e241-424e-ad6e-db1e2cb9b1e6
# MAGIC
#### Extrair o atributo de data e hora do timestamp
# MAGIC
#### **`year`**
Extrai o ano como um número inteiro de uma determinada data/timestamp/string.
# MAGIC
##### Métodos semelhantes: **`month`**, **`dayofweek`**, **`minute`**, **`second`** etc.

<hr>--i18n-f06bd91b-c4f4-4909-98dd-680fbfdf56cd
# MAGIC
#### Converter em data
# MAGIC
#### **`to_date`**
Converte a coluna em DateType convertendo regras em DateType.

<hr>--i18n-8367af41-fc35-44ba-8ab1-df721452e6f3
# MAGIC
### Manipular data e hora
#### **`date_add`**
Retorna a data que corresponde ao número determinado de dias após o início

<hr>--i18n-3669ec6f-2f26-4607-9f58-656d463308b5
# MAGIC
### Limpar sala de aula

