3. Padrão de desenvolvimento
++++++++++++++++++++++++++++++
3.1 Organização dos notebooks
==============================

A organização dos notebooks dentro do workspace deve ser feita segundo o padrão abaixo: 

    .. image:: /imagesdb/db1.png

3.2 Pasta config
==================

No repositório das áreas, cada projeto deverá ter a própria pasta **config**. Dentro dela, deve haver 2 artefatos principais a serem usados em todos os notebooks, que são os arquivos **criaListaCamadas.py** e **acessoCamadas.py**. 

Nesses notebooks, haverá duas variáveis importantes. A primeira, **escopoArea**, é o nome da área a que pertence o workspace, e é usada para “construir” o ponto de montagem a ser usado nas funções de acesso a arquivos. A segunda, **escopoProjeto**, é o nome do projeto, e é usada para construir o caminho no sistema de arquivos do Databricks onde serão gravados os arquivos de apoio

3.2.1 Notebook criaListaCamadas.py
------------------------------------

Nesse notebook devem ser especificados os diretórios e arquivos do storage que serão acessados pelos processos.  

São 3 chaves a serem preenchidas:

* **baseLeitura**, onde devem estar os diretórios acessados para leitura;
* **baseEscrita**, onde devem estar os diretórios acessados para escrita;
* **baseArquivos**, onde devem estar os nomes dos arquivos que serão acessados

    .. image:: /imagesdb/Imagem1.jpg

**Figura 1**: Exemplo de preenchimento do notebook criaListaCamadas.py 

Se um diretório for acessado para leitura e escrita, deve ser especificado em ambas as chaves. 

Dentro de cada uma dessas 3 chaves, conterão as informações sobre cada base que será usada, também agrupadas como chaves. A estrutura dessas sub-chaves deve conter:

* **“nome”**: nome usado para referenciar aquela base, seja diretório ou nome de arquivo;
* **“caminho”**: diretório em que se encontra o arquivo no Data Lake, seja este o diretório de leitura ou de escrita; no caso de baseArquivos, deve ser preenchido com o nome do arquivo;
* **“informacao”**: descrição da base em questão. 

A partir das chaves principais, 3 arquivos json serão criados no sistema de arquivos: baseLeitura.json, baseEscrita.json e baseArquivos.json. Eles serão usados no notebook “acessoCamadas.py”.  

    .. image:: /imagesdb/Imagem2.jpg

**Figura 2**: Exemplo de preenchimento do notebook criaListaCamadas.py

**Importante**: o notebook “criaListaCamadas” deve ser executado no início de cada processo que o utiliza, para garantir que os arquivos de base estejam sempre atualizados. 

Com a utilização do “Repos” e para que em produção não ocorra “quebras” de chamadas dos notebooks criaListaCamadas assim como acessoCamadas (3.2.2), ambos devem ser chamados fazendo uso do caminho relativo, exemplo: 

**%run ../../config/criaListaCamadas**

**%run ../../config/acessoCamadas**  

**%run ../config/criaListaCamadas**

**%run ../config/acessoCamadas**  

O caminho relativo vai mudar dependendo da hierarquia de pasta utilizada.

3.2.2 Notebook acessoCamadas.py
----------------------------------

É um notebook padrão, que será disponibilizado na pasta do projeto na criação do ambiente. Define as variáveis e métodos a serem usados nos notebooks para acesso aos arquivos, para leitura e gravação. No notebook estão disponíveis instruções para uso, bem como alguns exemplos.  

A partir dos arquivos base criados no notebook **criaListaCamadas.py**, serão criados os dataframes usados nos métodos de leitura e escrita.  

    .. image:: /imagesdb/Imagem3.jpg

**Figura 3**: Exemplo do notebook acessoCamadas.py 

Os mencionados métodos para acesso a arquivos irão retornar o diretório de um arquivo com base no nome da base e ambiente que forem passados como parâmetro. 

O método **caminhoLeitura** buscará no arquivo baseLeitura.json o diretório correspondente ao nome de base informado. Caso seja passado também um valor para o parâmetro “nomeArquivo” (opcional), o nome informado em baseArquivos.json será concatenado com o diretório, retornando o caminho completo do arquivo. 

Outro parâmetro obrigatório é o que se refere ao ambiente, que definirá se a leitura ou escrita será feita em ambiente produtivo (ambienteDevHmlPrd = “prd”) ou de laboratório (ambienteDevHmlPrd = “dev”). O retorno do método será a concatenação do ponto de montagem do ambiente informado via parâmetro com o restante do diretório. Dessa forma, se o diretório retornado não existir no ambiente desejado, ou esse diretório for usado para escrita em ambiente produtivo (a partir do ambiente de laboratório), a tentativa de leitura ou escrita retornará um erro, portanto, é necessário atenção durante o desenvolvimento.  

    .. image:: /imagesdb/Imagem4.jpg

**Figura 4**: método caminhoLeitura 

O método **caminhoEscrita**, funciona da mesma forma, porém, buscará os diretórios indicados no arquivo baseEscrita.json. 

    .. image:: /imagesdb/Imagem5.jpg

**Figura 5**: método caminhoEscrita 

O método **getPath** tem funcionamento semelhante aos dois anteriores, porém serve para ambas as operações de leitura e escrita, sendo necessário informar um valor para o parâmetro “tipo”, que indicará qual será a operação (“E” para escrita, ou “L” para leitura). Isso determinará se o método recuperará o diretório no arquivo baseEscrita.json ou baseLeitura.json

    .. image:: /imagesdb/Imagem6.jpg

**Figura 6**: método getPath 

3.3 Clusters
==============

No modelo atual (Mencionado em 2. Ambientes) é importante e recomendado que ao desenvolver, selecione o cluster de sua área, esta visão de mais que um cluster é possível apenas para desenvolvedores que fazem parte e/ou tem acesso a outras áreas de desenvolvimentos, por isso é fortemente recomentado que selecione o correto. 

    .. image:: /imagesdb/Imagem7.jpg

**Figura 7**: Listando Clusters disponíveis  

No ambiente de DEV é disponibilizado um cluster para cada área, e seu nome segue o padrão **cluster-[área]**. A versão da runtime usada é a última LTS (Long Time Version) disponível. Atualmente (setembro de 2022), é a 10.4 LTS, com Spark 3.2.1 e Scala 2.12. 

Qualquer necessidade de atualização da versão do cluster deve ser alinhada com o time de Infra Cloud, para que seja verificada a viabilidade. Se a versão do cluster usada durante o desenvolvimento não corresponder a do ambiente produtivo, é possível que ocorra erro na execução após o deploy. A situação inversa também é verdadeira, uma vez que alterar a versão do cluster produtivo pode impactar no funcionamento de processos que já estão ali. 

3.4 Workflows
===============

Não orientamos a criação de Workflows nos ambientes de desenvolvimentos, entende-se que se o processo tem necessidade de execução com uma frequência e está vinculado a uma demanda existente de uso, o mesmo deverá passar pela esteira de produtização para que possa ser agendado via Data Factory. 

Existe uma rotina diária que faz a deleção de todos os Workflows que possivelmente foram criados, esta rotina não impacta em nenhum notebook desenvolvido, apenas deleta todos os Workflows seja ele em execução ou inativo. 

3.5 Boas práticas e orientações gerais
=========================================

* Usar o **PySpark** no lugar do Python puro, uma vez que este inviabiliza o uso do processamento paralelo pelo Spark;
* Caso necessário utilizar a biblioteca pandas, importar a do Spark, **pyspark.pandas**, uma vez que o Spark é multithread e seu código pode ser executado de forma distribuída;
* Sempre dar preferência à utilização de bibliotecas que são built in do Spark. Necessidades que fujam dessa orientação devem ser levadas ao time de Governança Técnica, que avaliará a possibilidade de instalação; 
* Não fazer leitura de arquivos XLSX ou XLS no Databricks, uma vez que esta extensão reduz consideravelmente o desempenho do cluster. Para trabalhar com dados que originalmente possuem esse tipo de extensão, recomendados a utilização do Data Factory para conversão para parquet; 
* Não fazer leitura de um diretório completo, a menos que seja indispensável. Caso a necessidade do processo seja ler um range de tempo específico, montar o diretório de forma a ler apenas os anos, meses ou dias desejados. Por exemplo, se uma tabela tem histórico de 2013 a 2022 no Data Lake, mas um processo que a usará precisa apenas dos dados de 2021 em diante, deve-se ler apenas esse período de tempo, evitando, assim, uma leitura desnecessária. 
* Não é permitido produtizar processos apontando para o workspace de DEV, portanto, toda alteração deve ser produtizada, ou os outputs corretos serão escritos apenas no storage de LAB, cuja utilização não será liberada em outros processos;
* Arquivos temporários de processos devem ser escritos na camada transient, conforme padrão de camadas definido, e excluídos ao final do processo;
* Ao realizar escrita no storage, o Databricks gera arquivos temporários e com nomes fora do padrão usado pela Ipiranga. Para solucionar isso, usamos um método simples que faz a escrita inicial na camada transient, e depois faz cópia apenas do arquivo parquet para a camada final (trusted ou refined), renomeando conforme padrão. 

    .. image:: /imagesdb/Imagem8.jpg

**Figura 8**: método copia_arquivo

3.6 Uso de Delta Table
=========================

O uso de Delta Tables é indicado quando há necessidade de controle de versão de registros para o output de um processo, ou seja, quando é realizado operações de insert e/ou update de registros. 

Se o processo contempla, por exemplo, reprocessamento full dos dados ou de um período de tempo, ou processa sempre dados de uma data de referência, a orientação é gravá-los como parquets padrão, segundo o padrão de camadas em `Definição de Camadas do Data Lake <https://grupoultracloud.sharepoint.com/:b:/r/sites/ipp-portalgestaodados/Documentos%20Compartilhados/Analytics/Engenharia%20de%20dados/Data%20Lake%20Storage/Defini%C3%A7%C3%A3o%20de%20Camadas%20Data%20Lake.pdf?csf=1&web=1&e=d2hBjb>`_.  

Ao fazer uso de Delta Table é importante aplicar  alguns comandos para garantir uma boa performance da tabela, assim como eliminar versões não mais utilizadas as quais impactam em armazenamento. 

A seguir será listado dois comandos que ajudam bastante neste quesito, são eles: 

3.6.1 Vacuum
---------------

Remove todos os arquivos do diretório da tabela que não são gerenciados pela Delta, bem como os arquivos de dados que não estão mais no estado mais recente do log de transações da tabela e são mais antigos do que um limite de retenção (padrão 7 dias (168 horas)) reduzindo assim custos de armazenamento em nuvem. 

Por não acionar automaticamente é importante manter uma rotina de execução regularmente (mantenha uma frequência que busque equilibrar as compensações de custo e desempenho), para reduzir os custos excessivos de armazenamento, exemplo da sintaxe: 

**spark.sql(f"VACUUM DELTA.`{path}` RETAIN 168 hours")**
 
Referências: 

https://docs.databricks.com/sql/language-manual/delta-vacuum.html 

https://docs.delta.io/latest/delta-utility.html#language-sql 

3.6.2 Optimize
----------------

O Delta Lake no Azure Databricks pode aumentar a velocidade das consultas de leitura de uma tabela. Uma forma de aumentar essa velocidade é unir arquivos pequenos com arquivos maiores, esta ação de compactação é feita através do comando OPTIMIZE. 

Assim como o VACUUM o OPTIMIZE precisa também manter uma rotina de execução (mantenha uma frequência que busque equilibrar as compensações de custo e desempenho), exemplo da sintaxe: 

**spark.sql(f"OPTIMIZE DELTA.`{path}`")**

Referências: 

https://docs.databricks.com/delta/optimize.html 

https://docs.delta.io/latest/optimizations-oss.html#language-sql 