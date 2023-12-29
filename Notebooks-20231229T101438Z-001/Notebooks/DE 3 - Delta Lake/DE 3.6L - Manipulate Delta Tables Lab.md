# /DE 3 - Delta Lake/DE 3.6L - Manipulate Delta Tables Lab
<hr>--i18n-65583202-79bf-45b7-8327-d4d5562c831d
-- MAGIC
-- MAGIC
# Laboratório de manipulação de tabelas Delta
-- MAGIC
Este notebook fornece uma revisão prática de alguns dos recursos mais especializados que o Delta Lake traz ao data lakehouse.
-- MAGIC
## Objetivos de aprendizado
Ao final deste laboratório, você será capaz de:
- Revisar a história da tabela
- Consultar versões anteriores da tabela e reverter uma tabela para uma versão específica
- Realizar a compactação do arquivo e a indexação com Z-order
- Visualizar os arquivos marcados para exclusão permanente e confirmar essas exclusões

<hr>--i18n-065e2f94-2251-4701-b0b6-f4b86323dec8
-- MAGIC
-- MAGIC
## Configuração
Execute o script a seguir para configurar as variáveis necessárias e limpar execuções anteriores deste notebook. Observe que reexecutar esta célula permitirá que você reinicie o laboratório.

<hr>--i18n-56940be8-afa9-49d8-8949-b4bcdb343f9d
-- MAGIC
-- MAGIC
## Criar a história da Bean Collection
-- MAGIC
A célula abaixo inclui várias operações de tabela, resultando no seguinte esquema para a tabela **`beans`**:
-- MAGIC
| Nome do campo | Tipo de campo |
| --- | --- |
| nome | STRING |
| color | STRING |
| grams | FLOAT |
| delicious | BOOLEAN |

<hr>--i18n-bf6ff074-4166-4d51-92e5-67e7f2084c9b
-- MAGIC
-- MAGIC
-- MAGIC
## Revisar a história da tabela
-- MAGIC
O log de transações do Delta Lake armazena informações sobre cada transação que modifica o conteúdo ou as configurações de uma tabela.
-- MAGIC
Revise a história da tabela **`beans`** abaixo.

<hr>--i18n-fb56d746-8889-41c1-ba73-576282582534
-- MAGIC
-- MAGIC
Se todas as operações anteriores foram concluídas conforme descrito, você deverá ver sete versões da tabela (**OBSERVAÇÃO**: Como o controle de versão do Delta Lake começa por 0, o número máximo da versão é 6).
-- MAGIC
As operações deverão ser:
-- MAGIC
| versão | operação |
| --- | --- |
| 0 | CREATE TABLE |
| 1 | WRITE |
| 2 | WRITE |
| 3 | UPDATE |
| 4 | UPDATE |
| 5 | DELETE |
| 6 | MERGE |
-- MAGIC
A coluna **`operationsParameters`** permite revisar os predicados usados em atualizações, exclusões e merges. A coluna **`operationMetrics`** indica quantas linhas e arquivos são adicionados em cada operação.
-- MAGIC
Revise a história do Delta Lake para identificar a versão da tabela que corresponde a uma determinada transação.
-- MAGIC
**OBSERVAÇÃO**: A coluna **`version`** designa o estado de uma tabela quando uma determinada transação é concluída. A coluna **`readVersion`** indica a versão da tabela na qual uma operação foi executada. Nesta demonstração simples (sem transações concorrentes), esse relacionamento deve sempre mostrar um incremento de 1.

<hr>--i18n-00d8e251-9c9e-4be3-b8e7-6e38b07fac55
-- MAGIC
-- MAGIC
## Consultar uma versão específica
-- MAGIC
Depois de revisar a história da tabela, você decide visualizar o estado da tabela após a inserção dos primeiros dados.
-- MAGIC
Execute a query abaixo para obter esse resultado.

<hr>--i18n-90e3c115-6bed-4b83-bb37-dd45fb92aec5
-- MAGIC
-- MAGIC
E agora revise o estado atual dos dados.

<hr>--i18n-f073a6d9-3aca-41a0-9452-a278fb87fa8c
-- MAGIC
-- MAGIC
Você deseja revisar os pesos dos grãos antes de excluir qualquer registro.
-- MAGIC
Preencha a instrução abaixo para registrar uma view temporária da versão imediatamente anterior à exclusão dos dados e, em seguida, execute a célula a seguir para consultar essa view.

<hr>--i18n-bad13c31-d91f-454e-a14e-888d255dc8a4
-- MAGIC
-- MAGIC
Execute a célula abaixo para verificar se a versão correta foi capturada.

<hr>--i18n-8450d1ef-c49b-4c67-9390-3e0550c9efbc
-- MAGIC
-- MAGIC
## Restaurar uma versão anterior
-- MAGIC
Aparentemente houve um mal-entendido. Os grãos que um amigo lhe deu e que você incorporou em sua coleção não eram para você guardar.
-- MAGIC
Reverta a tabela para a versão anterior à conclusão da instrução **`MERGE`**.

<hr>--i18n-405edc91-49e8-412b-99e7-96cc60aab32d
-- MAGIC
-- MAGIC
Revise a história da tabela. Observe que a restauração para uma versão anterior adiciona outra versão da tabela.

<hr>--i18n-d430fe1c-32f1-44c0-907a-62ef8a5ca07b
-- MAGIC
-- MAGIC
## Compactação de arquivos
Observando as métricas de transação durante a reversão, você se surpreende com o grande número de arquivos para uma coleção de dados tão pequena.
-- MAGIC
Embora seja improvável que a indexação em uma tabela desse tamanho melhore o desempenho, você decide adicionar um índice Z-order no campo **`name`** prevendo o crescimento exponencial de sua bean collection ao longo do tempo.
-- MAGIC
Use a célula abaixo para realizar a compactação do arquivo e a indexação com Z-order.

<hr>--i18n-8ef4ffb6-c958-4798-b564-fd2e65d4fa0e
-- MAGIC
-- MAGIC
Os dados devem ter sido compactados em um único arquivo. Confirme se isso ocorreu executando a célula a seguir.

<hr>--i18n-8a63081c-1423-43f2-9608-fe846a4a58bb
-- MAGIC
-- MAGIC
Execute a célula abaixo para verificar se você otimizou e indexou a tabela com sucesso.

<hr>--i18n-6432b28c-18c1-4402-864c-ea40abca50e1
-- MAGIC
-- MAGIC
## Limpando arquivos de dados obsoletos
-- MAGIC
Você sabe que embora todos os dados agora residam em um arquivo, os arquivos de dados de versões anteriores da tabela ainda estão armazenados com ele. Você deseja remover esses arquivos e o acesso a versões anteriores da tabela executando **`VACUUM`** na tabela.
-- MAGIC
Executar **`VACUUM`** remove o lixo do diretório da tabela. Por default, será aplicado um limite de retenção de sete dias.
-- MAGIC
A célula abaixo modifica algumas configurações do Spark. O primeiro comando substitui a verificação do limite de retenção para permitir a demonstração da remoção permanente de dados. 
-- MAGIC
**OBSERVAÇÃO**: Limpar uma tabela de produção com uma retenção curta pode levar à corrupção de dados e/ou à falha de queries de longa execução. Estamos usando essa configuração apenas para fins de demonstração e é preciso ter extremo cuidado ao desativá-la.
-- MAGIC
O segundo comando define **`spark.databricks.delta.vacuum.logging.enabled`** como **`true`** para garantir que a operação **`VACUUM`** seja registrada no log de transações.
-- MAGIC
**OBSERVAÇÃO**: Devido a pequenas diferenças nos protocolos de armazenamento em diversas clouds, o registro em log de comandos **`VACUUM`** não é ativado por default em algumas clouds a partir do DBR 9.1.

<hr>--i18n-04f27ab4-7848-4418-ac79-c339f9843b23
-- MAGIC
-- MAGIC
Antes de excluir permanentemente os arquivos de dados, revise-os manualmente usando a opção **`DRY RUN`**.

<hr>--i18n-bb9ce589-09ae-47b8-b6a6-4ab8e4dc70e7
-- MAGIC
-- MAGIC
Todos os arquivos de dados que não estão na versão atual da tabela são mostrados na visualização acima.
-- MAGIC
Execute o comando novamente sem **`DRY RUN`** para excluir permanentemente esses arquivos.
-- MAGIC
**OBSERVAÇÃO**: Não há mais nenhuma versão anterior da tabela acessível.

<hr>--i18n-1630420a-94f5-43eb-b37c-ccbb46c9ba40
-- MAGIC
-- MAGIC
Como **`VACUUM`** pode ser uma ação bastante destrutiva para datasets importantes, é sempre uma boa ideia reativar a verificação da duração da retenção. Execute a célula abaixo para reativar essa configuração.

<hr>--i18n-8d72840f-49f1-4983-92e4-73845aa98086
-- MAGIC
-- MAGIC
Observe que a história da tabela indica o usuário que concluiu a operação **`VACUUM`**, o número de arquivos excluídos e registra em log que a verificação de retenção estava desativada durante a operação.

<hr>--i18n-875b39be-103c-4c70-8a2b-43eaa4a513ee
-- MAGIC
-- MAGIC
Consulte a tabela novamente para confirmar que você ainda tem acesso à versão atual.

<hr>--i18n-fdb81194-2e3e-4a00-bfb6-97e822ae9ec3
-- MAGIC
-- MAGIC
<img src="https://files.training.databricks.com/images/icon_warn_32.png"> Como o Delta Cache armazena cópias de arquivos consultados na sessão atual em volumes de armazenamento implantados no cluster ativo no momento, você ainda pode acessar temporariamente versões anteriores da tabela (embora os sistemas **não** devam ser projetados para esperar esse comportamento). 
-- MAGIC
Reiniciar o cluster garantirá que esses arquivos de dados armazenados em cache sejam eliminados permanentemente.
-- MAGIC
Você pode ver um exemplo disso removendo o comentário e executando a célula a seguir, que pode ou não falhar
(dependendo do estado do cache).

<hr>--i18n-a5cfd876-c53a-4d60-96e2-cdbc2b00c19f
-- MAGIC
-- MAGIC
Após concluir este laboratório, você deverá ser capaz de realizar estas tarefas com confiança:
* Concluir comandos padrão de criação de tabelas e manipulação de dados do Delta Lake
* Revisar metadados da tabela, incluindo a história da tabela
* Utilizar o controle de versão do Delta Lake para queries e reversões de snapshots
* Compactar arquivos pequenos e indexar tabelas
* Usar **`VACUUM`** para revisar os arquivos marcados para exclusão e confirmar essas exclusões

<hr>--i18n-b541b92b-03a9-4f3c-b41c-fdb0ce4f2271
-- MAGIC
 
Execute a célula a seguir para excluir as tabelas e arquivos associados a esta lição.

