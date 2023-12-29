# /DE 2 - ETL with Spark/DE 2.99 - OPTIONAL Higher Order Functions
<hr>--i18n-a51f84ef-37b4-4341-a3cc-b85c491339a8
-- MAGIC
-- MAGIC
# Funções de ordem superior no Spark SQL
-- MAGIC
As funções de ordem superior no Spark SQL permitem transformar tipos de dados complexos, como objetos do tipo matriz ou mapa, preservando suas estruturas originais. Exemplos:
- **`FILTER()`** filtra uma matriz usando a função lambda determinada.
- **`EXIST()`** testa se uma instrução é verdadeira para um ou mais elementos em uma matriz. 
- **`TRANSFORM()`** usa a função lambda determinada para transformar todos os elementos em uma matriz.
- **`REDUCE()`** usa duas funções lambda para reduzir os elementos de uma matriz a um único valor, mesclando os elementos em um buffer e aplicando uma função de finalização no buffer final. 
-- MAGIC
-- MAGIC
## Objetivos de aprendizado
Ao final desta lição, você deverá ser capaz de:
* Usar funções de ordem superior para trabalhar com matrizes

<hr>--i18n-b295e5de-82bb-41c0-a470-2d8c6bbacc09
-- MAGIC
-- MAGIC
## Executar a configuração
Execute a célula a seguir para configurar seu ambiente.

<hr>--i18n-bc1d8e11-d1ff-4aa0-b4e9-3c2703826cd1
## Filter
Podemos usar a função **`FILTER`** para criar uma coluna que exclui valores de cada matriz com base em uma condição fornecida.  
Vamos usar esse método para remover os produtos que não são do tamanho king da coluna **`items`** em todos os registros do dataset **`sales`**. 
-- MAGIC
**`FILTER (items, i -> i.item_id LIKE "%K") AS king_items`**
-- MAGIC
Na instrução acima:
- **`FILTER`** : é o nome da função de ordem superior <br>
- **`items`** : é o nome da matriz de entrada <br>
- **`i`** : é o nome da variável iteradora. Escolha o nome e use-o na função lambda. Essa variável itera na matriz, alternando cada valor na função, um de cada vez.<br>
- **`->`** :  indica o início de uma função <br>
- **`i.item_id LIKE "%K"`** : é a função. A função verifica a presença da letra K maiúscula no final de cada valor. Se a letra for encontrada, o valor será filtrado para a nova coluna **`king_items`**.
-- MAGIC
**OBSERVAÇÃO:** Você pode escrever um filtro que produza muitas matrizes vazias na coluna criada. Quando isso acontece, pode ser útil usar uma cláusula **`WHERE`** para mostrar apenas valores de matriz não vazios na coluna retornada. 

<hr>--i18n-3e2f5be3-1f8b-4a54-9556-dd72c3699a21
-- MAGIC
## Transform
-- MAGIC
A função de ordem superior **`TRANSFORM()`** pode ser particularmente útil quando você deseja aplicar uma função existente a cada elemento de uma matriz.  
Vamos aplicar essa função para criar uma nova coluna de matriz chamada **`item_revenues`** transformando os elementos contidos na coluna de matriz **`items`**.
-- MAGIC
Na query abaixo: **`items`** é o nome da matriz de entrada, **`i`** é o nome da variável iteradora (com o nome que você escolhe e usa na função lambda; ela itera na matriz, alternando cada valor na função, um de cada vez) e **`->`** indica o início de uma função.  

<hr>--i18n-ccfac343-4884-497a-a759-fc14b1666d6b
-- MAGIC
A função lambda especificada acima lê o subcampo **`item_revenue_in_usd`** de cada valor, multiplica-o por 100, converte o resultado em um número inteiro que coloca na nova coluna de matriz **`item_revenues`**

<hr>--i18n-9a5d0a06-c033-4541-b06e-4661804bf3c5
-- MAGIC
## Laboratório da função Exists
Neste passo, você usará a função de ordem superior **`EXISTS`** com dados da tabela **`sales`** para criar as colunas booleanas **`mattress`** e **`pillow`** que indicam se o item adquirido é um colchão ou um travesseiro.
-- MAGIC
Por exemplo, se **`item_name`** da coluna **`items`** terminar com a string **`"Mattress"`**, o valor da coluna para **`mattress`** deverá ser **`true`**, e o valor para **`pillow`** deverá ser **`false`**. Alguns exemplos de itens e valores resultantes são mostrados a seguir.
-- MAGIC
|  itens  | colchão | travesseiro |
| ------- | -------- | ------ |
| **`[{..., "item_id": "M_PREM_K", "item_name": "Premium King Mattress", ...}]`** | true | false |
| **`[{..., "item_id": "P_FOAM_S", "item_name": "Standard Foam Pillow", ...}]`** | false | true |
| **`[{..., "item_id": "M_STAN_F", "item_name": "Standard Full Mattress", ...}]`** | true | false |
-- MAGIC
Consulte a documentação sobre a função <a href="https://docs.databricks.com/sql/language-manual/functions/exists.html" target="_blank">exists</a>.  
Você pode usar a expressão de condição **`item_name LIKE "%Mattress"`** para verificar se a string **`item_name`** termina com a palavra "Mattress" (colchão).

<hr>--i18n-3dbc22b0-1092-40c9-a6cb-76ed364a4aae
-- MAGIC
-- MAGIC
-- MAGIC
A função auxiliar abaixo retornará um erro com uma mensagem informando o que precisa ser alterado caso você não tenha seguido as instruções. Nenhuma saída significa que você concluiu esta etapa.

<hr>--i18n-caed8962-3717-4931-8ed2-910caf97740a
-- MAGIC
Execute a célula abaixo para confirmar se a tabela foi criada corretamente.

<hr>--i18n-ffcde68f-163a-4a25-85d1-c5027c664985
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

