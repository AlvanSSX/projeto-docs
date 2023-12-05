6. Fluxo de dados
++++++++++++++++++

Com base na definição de cada camada, e na matriz de obrigatoriedade apresentada na seção 3 desse documento, podemos definir alguns fluxos padrão de dados que possuímos hoje. O documento `Arquitetura - Fluxo de Dados <https://grupoultracloud.sharepoint.com/:b:/r/sites/ipp-portalgestaodados/Documentos%20Compartilhados/Analytics/Engenharia/Data%20Lake%20Storage/Arquitetura%20-%20Fluxos%20de%20Dados.pdf?csf=1&web=1&e=fzM9rQ>`_ ilustra esses casos. Em todos os casos descritos abaixo, deve-se excluir o arquivo da camada **transient** ao fim do processo.

6.1. Arquivos manuais
=======================
Arquivos manuais são normalmente enviados por email, capturados via Logic App e armazenados no storage. 

Em alguns casos, pode ser necessário validar o layout do arquivo antes de trazê-lo oficialmente para o Data Lake. Para isso, alguns processos o salvarão inicialmente na camada **transient** e, via Data Factory, usarão uma atividade Get Metadata para checar o layout do arquivo, por exemplo, para garantir que as colunas presentes são as esperadas. Após essa verificação os dados serão salvos na camada raw para seguir com o fluxo. Caso não haja necessidade desse tipo de validação, os dados poderão ir diretamente para a camada **raw.** 

Para realizar o tratamento dos dados, de forma a garantir a integridade e qualidade deles, são utilizadas functions, que farão a manipulação dos dados via Python e disponibilizarão o arquivo na camada **trusted**. 

Os fluxos 1 e 2 do desenho indicado anteriormente representam os casos descritos aqui. 

6.2. Bases internas
=====================

No caso de ingestão de bases de dados internas da Ipiranga, o processo pode ser feito de duas formas, e a ferramenta padrão para ambos os casos é o Data Factory. 

A primeira, incremental, traz apenas novos registros, inseridos ou alterados no banco, para o storage, através de uma atividade Copy Data. Esse arquivo é gravado na camada **transient**, e um Data flow cuidará do ajuste de datatypes das colunas e particionamento dos dados na camada **raw** por uma data de referência. Em seguida, é necessário garantir a integridade dos dados, removendo duplicidade de registros e gravando o resultado na camada **trusted**. Para definir a ferramenta a ser usada para esse fim, é necessário avaliar caso a caso com a equipe de Governança Técnica. Este caso é representado no fluxo 3 do desenho. 

A segunda forma, que chamamos de full, traz sempre a base completa para o storage e salva seus dados como um arquivo único. Uma atividade de Copy data gravará o arquivo na camada **transient**, e para realizar o ajustes de datatypes, os dados passarão pelo Synapse, que fará o cast das colunas por uma query. O resultado, então, é disponibilizado na camada **trusted**. Este caso é representado no fluxo 4 do desenho. 

6.3. Dados externos
======================

Processos de ingestão de dados externos são feitos, em sua maioria, via Azure Functions. A function desenvolvida será responsável por obter o dado de sua fonte, seja ela um site ou API, gravando-o no storage na camada **raw**, no formato e estado em que foi obtido. A mesma function deverá tratar esse dado, tornando-o confiável, e salvar o arquivo na camada **trusted**, preferencialmente particionado por uma data de referência. Este caso é representado no fluxo 5 do desenho. 

Caso se perceba necessário utilizar outra ferramenta que não functions, para realizar esse tipo de extração, deve-se levar a questão para a equipe de Governança Técnica, que avaliará e orientará a melhor solução. 
 

6.4. Refinamentos
===================

Refinamentos de dados disponíveis no Data Lake, para gerar novas visões para projetos ou squads, indicadores, KPIs etc. são normalmente feitos via notebooks Databricks. O processo poderá consumir dados tanto da camada **trusted** quanto da **refined**, e após realizado o processamento deles pelo notebook, o output deve ser disponibilizado na camada **refined**. O fluxo 6 do desenho representa um caso em que esse output é usado para carregar uma tabela no Azure SQL, e isso deve ser feito via Data Factory, através de uma atividade Copy Data, que copiará o dado da camada **refined** para o banco. 