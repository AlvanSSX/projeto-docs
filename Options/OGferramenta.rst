6. Ferramentas do Ambiente Cloud Microsoft Azure
+++++++++++++++++++++++++++++++++++++++++++++++++

6.1 Data Lake Storage
======================

Na Ipiranga, o nosso Data Lake é constituído por 5 camadas, a saber: Transient, Raw, Trusted, Refined e Serving. Cada camada tem o seu motivo e objetivo, assim como padrões de armazenamento definidos pela área. Segue uma breve definição de cada uma delas: 

* Transient: camada usada para ingestão e armazenamento de dados gerais, como: cópias temporárias de tabelas e arquivos, e outros dados de curta duração, antes de serem ingeridos; 
* Raw: camada de ingestão, que armazena os dados brutos, assim como vieram da origem, sem conter filtros ou tratamentos, exceto filtro para o intervalo de tempo dos dados; 
* Trusted: são armazenados os dados já tratados/limpos e validados; transforma os dados brutos vindos da camada bruta em dados confiáveis para consumo; 
* Refined: são armazenados os dados enriquecidos, consolidados, vindos da camada Trusted; 
* Serving: para disponibilizar os arquivos que serão utilizados pelo usuário de negócio. 

Para mais informações sobre a definição das camadas e padronizações, consultar o documento de 
`Definição de Camadas do Data Lake <https://grupoultracloud.sharepoint.com/:b:/r/sites/ipp-portalgestaodados/Documentos Compartilhados/Analytics/Engenharia/Data Lake Storage/Defini%C3%A7%C3%A3o de Camadas Data Lake.pdf?csf=1&web=1&e=X291S0>`_.  

Atualmente, o nosso Data Lake produtivo é a storage account **“stippdatalakedev”**. É este o ambiente no qual os nossos pipelines, functions e notebooks armazenam os dados produtivos que são consumidos pelo negócio ou demais processos de engenharia ou de ciência de dados. Também possuímos uma storage account **“stippdatalakelab”** utilizada pelo time de ciência de dados e projetos para realizar testes de desenvolvimento, como se de fato fosse um laboratório. Aconselhamos que os novos processos de engenharia sejam testados inicialmente neste ambiente evitando assim que testes sejam realizados no Data Lake de produção. 

.. image:: /images/Imagem1.jpg

**Figura 1:** Data Lake de Produção, container **data** 

.. image:: /images/Imagem2.jpg

**Figura 2:** Data Lake de LAB, container **data** 

Para os dados sensíveis, existe uma storage account **“stippdatalakelgpdhml”** na qual armazenamos as tabelas que contém informações sensíveis, como CPF, nome de cliente, dados bancários, endereço, telefone etc. Porém, vale destacar que para ingerir tabelas com informações sensíveis é necessário ter autorização jurídica, e o mesmo vale para o projeto ou usuário que irá consumir estes dados.

.. image:: /images/Imagem3.jpg

**Figura 3:** Data Lake para dados sensíveis, container **dados-sensiveis** 

Para mais informações sobre o Data Lake Storage, consulte a documentação da Microsoft: https://docs.microsoft.com/pt-br/azure/storage/blobs/data-lake-storageintroduction. 

6.1.1 Acesso aos ativos
---------------------------

Vale ressaltar que os engenheiros terão apenas acesso de leitura ao ambiente de produção, portanto, não é possível fazer qualquer alteração nos diretórios ou parquets via Portal. Para os demais usuários, caso seja necessário obter acesso de leitura a algum diretório ou arquivo em específico, o mesmo deve solicitar ao engenheiro de dados alocado no projeto que abra uma solicitação para o time de arquitetura via formulário, como orientado na seção 3.1, informando qual recurso será utilizado para realizar a leitura do ativo, assim como o diretório onde ele se encontra.

6.2 Logic Apps
=================

O uso mais comum que fazemos do Logic App é para captura de arquivos enviados como anexos em e-mails, gravando esse arquivo no Data Lake. Além desse cenário, pode ser usado para trazer arquivos disponibilizados no SharePoint, ou mesmo fazer envio de email de notificação em caso de falha/sucesso em determinado processo. Casos que fujam dos mencionados devem ser levados à equipe de Governança Técnica para avaliação. 

Uma ação que é possível de se realizar via Logic App, mas que **não utilizamos** na Ipiranga, é fazer chamada de pipelines do Data Factory. **Toda orquestração de processos deve ser concentrada no ADF**. No caso de Logic Apps, caso um processo precise ser executado após a disponibilização de um arquivo no storage, deve-se criar uma event trigger para capturar esse evento e acionar o pipeline. 

Testes que envolvam envio ou recebimento de e-mails devem ser testados com o email Ipiranga do engenheiro e apontando para o storage de laboratório, **stippdatalakelab**. Quando for produtizado, essas conexões serão passadas para o email produtivo do Data Lake, svc.ipippdatalake-p@ultra.com.br, e storage produtivo, **stippdatalakedev**. 

Exemplos de Logic Apps existentes: 

* logic-ipp-email-site-location-potencial-postos-rede-ativa 
* logic-ipp-datalake-custos-sendemail 

Para mais informações sobre Logic Apps, consultar documentação da Microsoft https://docs.microsoft.com/pt-br/azure/logic-apps/logic-apps-overview.

6.2.1 Fluxo de produtização
-----------------------------

A criação de Logic Apps é feita primeiramente no Resource Group do projeto em que o engenheiro estiver alocado, que normalmente segue o padrão **rg-ipp-lab-[área do projeto]**, na subscription de LAB. 

Quando o desenvolvimento for concluído, deve-se enviar um email para arquiteturadedados@ultra.com.br solicitando que o Logic App criado seja passado para o ambiente produtivo, informando os nomes dos processos criados para movimentação. 

6.3 Data Factory
=================

Em nosso ambiente de engenharia, o Data Factory é usado para criar pipelines que fazem a cópia dos dados das bases internas da Ipiranga, como as bases transacionais e de BI mencionadas anteriormente. 

Essa cópia de dados pode ser feita de forma full, trazendo a base completa, ou incremental, trazendo apenas novos registros a cada execução. A carga full é indicada para bases com menor volumetria, como dimensões do BI, enquanto para tabelas de alta volumetria, a orientação é realizar carga incremental. No início de qualquer desenvolvimento de ingestão de dados, é importante avaliar essas questões, e entender a regra de carga da tabela em questão, para definir o processo da melhor forma, e levar o assunto para a equipe de Governança Técnica se necessário. 

Exemplos de pipelines de carga full e incremental abaixo. 

    .. image:: /images/Imagem4.jpg

**Figura 4:** Exemplo pipeline que faz a cópia full dos dados do banco de dados para Data Lake

    .. image:: /images/Imagem5.jpg

**Figura 5:** Exemplo pipeline que faz a cópia incremental dos dados do banco de dados para Data Lake 

6.3.1 Ingestão de dados com filtros 
---------------------------------------

Como descrito no documento de `Definição de Camadas do Data Lake <https://grupoultracloud.sharepoint.com/:b:/r/sites/ipp-portalgestaodados/Documentos Compartilhados/Analytics/Engenharia/Data Lake Storage/Defini%C3%A7%C3%A3o de Camadas Data Lake.pdf?csf=1&web=1&e=X291S0>`_, **não se deve usar** no processo de ingestão **querys que contenham joins, filtros ou agregações**, mas sempre trazer o dado mais bruto possível, em seu menor grão. Isso porque diferentes projetos podem compartilhar de uma mesma (ou várias) tabelas, portanto, se cada um trouxesse as tabelas que precisa aplicando filtros específicos diretamente na origem, teríamos diversos processos de ingestão para uma mesma tabela. Qualquer exceção a isso deve ser levada ao time de Arquitetura para avaliação da necessidade.para avaliação da necessidade. 

6.3.2 Conversão de tipos
--------------------------

Durante o processo de ingestão, também é possível converter o tipo das colunas. Para ingestões do Oracle, especificamente, as colunas do tipo “int”, “float” ou “double” são entendidas pelo Data Factory como tipo “decimal” e isto pode ser um problema para o time de ciência de dados, por exemplo, uma vez que a coluna do tipo “decimal” passa a ser entendida como do tipo “object” e isso tem um impacto negativo quando utilizando algumas bibliotecas no Python. Por esta razão, para esse tipo de ingestão, é mandatória a correção dos tipos das colunas durante a ingestão dos dados. Para outros tipos de ingestão, é importante considerar a questão.  

Atualmente, para cargas full, utilizamos o Synapse para realizar a conversão de tipos, enquanto para cargas incrementais, utilizamos o Dataflow. Exemplo desses pipelines são indicados na seção 6.3.7. 

6.3.3 Orquestração de processos
---------------------------------

Utilizamos o Data Factory como nosso orquestrador oficial de processos. É possível fazer agendamento de notebooks Databricks, encadeamento de pipelines, agendamento de pipelines através de time triggers ou event triggers etc. Apenas functions utilizam agendamento próprio, através do arquivo **function.json** que define o gatilho, as associações e outras definições de configuração da função, sendo esse arquivo único para cada function. 

6.3.4 Uso do Dataflow
-----------------------

Utilizamos o Dataflow principalmente para cargas incrementais, particionando os dados na estrutura de YYYY/MM/DD ou YYYY/MM, seja através das colunas de data, como DT_INCL (data de inclusão) e DT_REF  (data de referência), ou através da coluna de ano e mês, NO_AM (ano/mês de referência do dado).  

    .. image:: /images/Imagem6.jpg

**Figura 6:** Exemplo de estrutura de Dataflow 

Apesar de existir alguns pipelines que podemos utilizar como referência para novos desenvolvimentos, é necessário entender primeiramente a lógica de carga da tabela a ser ingerida, para construir o processo da melhor forma, inclusive, definir o melhor particionamento. 

6.3.5 Criação de datasets
---------------------------

Novos datasets só devem ser criados caso os que já existem não atendam a necessidade do processo, e nesse caso, deve-se consultar o documento de 
`Padrão de Desenvolvimento do Data Factory <https://grupoultracloud.sharepoint.com/:b:/r/sites/ipp-portalgestaodados/Documentos Compartilhados/Analytics/Engenharia/Data Factory/Data Factory - Padr%C3%A3o de Desenvolvimento.pdf?csf=1&web=1&e=7BG4HR>`_ para definir nomes e configurações. Para consultar os datasets permitidos, acessar documento de Padrão de Datasets. 

6.3.6 Parametrização de pipelines e datasets
---------------------------------------------

Adotamos a prática de utilização de parâmetros em pipelines e dataflows, para que em casos de manutenções, a alteração seja concentrada num único ponto. Esses parâmetros ficam num arquivo json, que deve ser carregado na pasta **“data/_conf/”**, e a leitura dele é feita a partir de uma atividade de Lookup. 

    .. image:: /images/Imagem7.jpg

**Figura 7:** Exemplo de parâmetros passados para um dataflow 

Além de pipelines, também utilizamos datasets parametrizados. Isso permite que um mesmo dataset seja utilizado por diversos processos. Por exemplo, no caso de datasets para bancos de dados, passamos o owner e nome da tabela a ser acessada; para arquivos do storage, passamos contêiner, diretório, nome do arquivo e tipo de compressão. 

    .. image:: /images/Imagem8.jpg

**Figura 8:** Exemplo de dataset parquet que aponta para o storage produtivo 

    .. image:: /images/Imagem9.jpg

**Figura 9:** Exemplo de uso de dataset parametrizado e passagem dos parâmetros 

    .. image:: /images/Imagem10.jpg

**Figura 10:** Exemplo de dataset que permite a conexão com o Oracle 

    .. image:: /images/Imagem11.jpg

**Figura 11:** Exemplo de dataset destinado a arquivos no formato parquet, para leitura ou escrita 

Também é possível importar dados do próprio Data Lake, fazer algum tipo de conversão, como alterar o tipo do dado de CSV para parquet ou XLSX para parquet. Para isso basta usar uma atividade de cópia em que o Source seja um dataset do tipo do arquivo de origem que se deseja converter, e o Sink seja um dataset do tipo do arquivo para o qual se deseja converter. Abaixo um exemplo de conversão de XLSX para parquet, utilizando datasets parametrizados já existentes. 

    .. image:: /images/Imagem12.jpg

**Figura 12:** Exemplo de conversão de XLSX para parquet 

    .. image:: /images/Imagem13.jpg

**Figura 13:** Exemplo de conversão de XLSX para parquet 

6.3.7 Exemplos de pipelines
-----------------------------

Segue alguns exemplos de pipelines quem podem ser utilizados como base para a construção de novos pipelines. 

* Ingestão para carga full dos dados vindos do Oracle: **pip_dm_tipo_projeto_synapse;**
* Ingestão para carga incremental dos dados vindos do Oracle: **pip_pr_situacao_componente**, trazendo os dados por DT_INCL;
* Ingestão para carga incremental dos dados vindos do Oracle: **pip_pr_situacao_movimento_comp**, trazendo os dados por DT_INCL ou DT_ALTER;
* Pipeline responsável por copiar os dados do Data Lake X para o Data Lake Y: **pip_move_output_anp;**
* Pipeline responsável por copiar os dados do Data Lake X para Data Lake Y caso na base final não tenha o arquivo com a versão mais atual: **pip_move_demanda_vendas_sales_rslt;**
* Pipeline que executa um notebook Databricks: **pip_base_ofertas_notebook_dtb;**
* Pipeline que transforma os dados a partir de um output gerado por outro processo e insere esses dados transformados em uma base transacional: **pip_interface_sitelocation_salesforce** (avaliar com muito cuidado as demandas deste tipo e identificar a real necessidade);
* Pipeline que transforma um arquivo xlsx para parquet: **pip_cst_xls_dm_derivados_biocombustiveis;**
* Pipeline que lê um arquivo xlsx e extrai as abas existentes na planilha, gravando cada uma como um arquivo parquet único: **pip_converte_xlsx_parquet_exp_dados_manuais.**

6.3.8 Fluxo de produtização
----------------------------

No Data Factory trabalhamos com o modo GIT, portanto, novos desenvolvimentos devem ser feitos em branchs, criadas a partir da branch master. Isso pode ser feito diretamente pelo ADF: 

    .. image:: /images/Imagem14.jpg

**Figura 14:** Exemplo de criação de branch no ADF 

Com o desenvolvimento pronto e testado, e a branch validada, deve ser criado um pull request (opção também exibida na imagem acima). Essa opção levará para uma página do DevOps, onde deverão ser preenchidas as informações sobre as alterações sendo realizadas na branch. 

Criado o pull request, ele entrará em fila para avaliação da equipe de Governança Técnica, que avaliará a adequação do desenvolvimento aos padrões definidos pela Arquitetura, e poderá solicitar ajustes se necessário, devendo o engenheiro verificar o que foi apontado e corrigir. São necessárias 2 aprovações do grupo **“Aprovação Pull Request”** para finalização, e após esse processo, o pull request deverá ser completado pelo engenheiro. 

    .. image:: /images/Imagem15.jpg

**Figura 15:** Exemplo de finalização de pull request 

Após a conclusão do merge entre as branchs, ainda será necessário publicar as alterações no ADF, e isso é feito selecionando a branch master e em seguida a opção **Publish**. As alterações serão listadas e o desenvolvedor deverá dar o Ok para a publicação iniciar. 

    .. image:: /images/Imagem16.jpg

**Figura 16:** Exemplo de tela do Data Factory para publicação de alterações 

Para mais detalhes sobre o fluxo de produtização, consultar a documentação de padrões do Data Factory. 

6.3.9 Documentação
---------------------
O nosso Data Factory de produção chama-se **adf-ipp-datalake-dev**. Para mais informações sobre o nosso padrão de desenvolvimento e padrão de datasets, consultar os documentos abaixo: 

* `Padrão de Desenvolvimento no Data Factory <https://grupoultracloud.sharepoint.com/:b:/r/sites/ipp-portalgestaodados/Documentos%20Compartilhados/Analytics/Engenharia/Data%20Factory/Data%20Factory%20-%20Padr%C3%A3o%20de%20Desenvolvimento.pdf?csf=1&web=1&e=jRASHO>`_ 
* `Datasets padrão para utilização no Data Factory <https://grupoultracloud.sharepoint.com/:x:/r/sites/ipp-portalgestaodados/Documentos Compartilhados/Analytics/Engenharia/Data Factory/Data Factory - Datasets Padr%C3%A3o.xlsx?d=w0f545456bf7048dab8c0c5f157cccc34&csf=1&web=1&e=z64Wd6>`_ 

Para mais informações sobre o Data Factory, consultar documentação da Microsoft: https://docs.microsoft.com/pt-br/azure/data-factory/.

6.4. Azure Function
=====================

Em nossa arquitetura, usamos as Functions para construção de códigos que trazem dados de APIs ou códigos que fazem web scraping, e elas são executadas no ambiente Azure. Atualmente, temos dois padrões de desenvolvimento convivendo. 

Antes de iniciar o desenvolvimento, o documento `Guia para Configuração dos Pré– Requisitos do Azure Function <https://grupoultracloud.sharepoint.com/:w:/s/ipp-equipeanalytics/EeuRlUmgIpFBndtD7Y5iX2EBfKcZE0-srvQmxhd7Zz1IIw?e=HCHEXc>`_ orienta sobre a preparação do ambiente. 

6.4.1	Padrão de desenvolvimento
----------------------------------

Padrão de desenvolvimento anterior 

O primeiro, mais antigo, faz o versionamento no repositório **prj-datalakewebscraping** e function app **func-ipp-datalake-dev**. É usado para functions antigas e squads que não possuem ambiente próprio. O documento `Guia para Function <https://grupoultracloud.sharepoint.com/:w:/r/sites/ipp-portalgestaodados/Documentos%20Compartilhados/Analytics/Engenharia/Function/Guia%20para%20Function%20-%20PR%20Repo%20e%20Deploy%20Function%20App.docx?d=w79e31de374f74365afb9b5a59fef62fd&csf=1&web=1&e=ZaZ6U2>`_ orienta sobre o uso. 

Padrão de desenvolvimento atual 

O segundo, é usado por projetos mais recentes, que possuem ambiente separado no DevOps por área. Abaixo algumas orientações que devem ser seguidas. 

Fluxo para desenvolvimento e versionamento 

1.	Fazer instalações conforme a documentação inicial; 
2.	No DevOps, no ambiente requerido, criar uma branch a partir da master; 
3.	Na branch desejada, deve-se cloná-la e baixá-la para a máquina local; 
4.	Abrir o projeto no VS Code e instalar a extensão da Azure; 
5.	No VS Code, via extensão da Azure, entrar no function app de LAB para criar uma function a partir dele. Isso criará uma pasta local com alguns códigos de template; 
6.	Quando o desenvolvimento estiver concluído, para testar o que foi feito, podese voltar na extensão da Azure indo no recurso desejado e fazendo o deploy da function para o ambiente de LAB; 
7.	Após testar e validar em LAB, criar um pull request via DevOps para aprovação dos revisores e subida para o ambiente de produção. 

**Observação:** se a branch ficar aberta por muito tempo, o ideal é utilizar o comando "git fetch" e "git pull". 

Pasta sharedCode 

1.	**functions.py** contém alguns métodos de uso comum, como para acesso à storage account, e ler e gravar arquivos no Data Lake. Métodos de uso específico devem ser definidos dentro da function em que serão usados. 
2.	**arquivosAcessoCamada.py** deve ser editado com as referências aos arquivos de parâmetros de cada function, compondo uma chave no padrão abaixo:

::

    {
       "function": "af_[fontes_internas ou fontes_externas]_[descrição]", 
       "json": "sharedCode/[nome da function definido acima].json" 
    }

3. Arquivos de parâmetros
   
     3.1.	Cada function deve ter seu próprio arquivo de parâmetros, e o nome do json deve ser igual ao de sua function;
     
     3.2.	Os arquivos devem ser armazenados no próprio repositório, na pasta **sharedCode;**
     
     3.3.	Os diretórios acessados pelo código, para leitura e escrita, devem estar especificados no json;
     
     3.4.	O arquivo é recuperado usando o método **get_directories;**
     
     3.5.	Descrição das chaves do json: 

        * **processInformation** deve conter informações básicas do processo 
  
            * resourceName: nome do function app onde será armazenada a function 
            * applicationName: nome da function, respeitando o padrão **af_[fontes_internas ou fontes_externas]_[descrição]**
            * processDescription: breve descrição sobre objetivo da function
        * **DatasetSource** deve conter as informações de cada fonte de leitura da function
          
            * Name: nome que será usado para se referenciar àquela base no código
            * FileSystem: container onde está contido o dado 
            * Directory: caminho onde o dado está contido no storage
            * FileName: nome do arquivo 
        * **DatasetTarget**, analogamente ao anterior, deve conter as informações de cada destino de escrita da function.
  
     3.6.	Exemplo de json:
            
            ::

                { 
                    "processInformation": { 
                        "application":"FunctionApp", 
                         "resourceName":"func-ipp-jetoil-lab", 
                         "applicationName":"af_fontesexternas_customers_enrichment_daily", 
                         "processDescription":"Processo de execução diário do enriquecimento da base de clientes do Jetoil" 
                 },
                 "DatasetSource": { 
                     { 
                         "Name":"dm_componente", 
                         "FileSystem":"data", 
                         "Directory":"raw/dados_internos/bi/dw/dm_componente/", 
                         "FileName":"rw_dm_componente.parquet" 
                     } 
                 },
                 "DatasetTarget": { 
                     { 
                         "Name":"consumidor_final_jetoil", 
                         "FileSystem":"dados-sensiveis", 
                         "Directory":"raw/dados_internos/bi/dbfranq/consumidor_final_jeto il", 
                         "FileName":"rw_consumidor_final_jetoil_$DATA.parquet"
                     } 
                 } 
                } 


Orientações gerais

1.	Variáveis de ambiente devem ser usadas para fazer referência a storage account; 
2.	Tokens e afins devem ser referenciados através de **key vaults**, que são criados pela equipe de Infra. 

6.4.2 Instruções para novas functions
--------------------------------------

* Functions devem ter processamento curto. 
* Functions com processamento longo, devem ser orquestradas com `funções duráveis <https://docs.microsoft.com/pt-br/azure/azure-functions/durable/durable-functions-overview?tabs=csharp>`_; 
* Functions que precisem de acesso a outros recursos devem usar a `biblioteca de identidade <https://docs.microsoft.com/en-us/python/api/overview/azure/identity-readme?view=azure-python>`_; 
* Você pode configurar o seu ambiente com `VS Code <https://docs.microsoft.com/pt-br/azure/azure-functions/functions-develop-vs-code?tabs=csharp>`_ ou como melhor preferir; 
* Todas as variáveis relacionadas ao ambiente devem ser definidas usando o `local.settings.json <https://docs.microsoft.com/pt-br/azure/azure-functions/functions-develop-vs-code?tabs=csharp#local-settings>`_; 
* Todas as funções precisam ter um ou mais casos de teste que devem ser versionados e fazem parte do processo de validação. 

6.4.3 Fluxo de produtização
-----------------------------

Dentro de cada área de projeto no DevOps, haverá um repositório de function, com nome no padrão **prj-[área]-afa**, onde será feito o versionamento dos códigos. Por exemplo, para projetos que utilizam o ambiente do PCO, o local será https://dev.azure.com/ipiranga-dev/prj-pco/_git/prj-pco-afa. 

Para cada novo desenvolvimento, é necessário criar uma branch para commitar as alterações. Essa branch pode ser importada no VS Code para testar localmente (para isso, o documento mencionado no início dessa seção pode ser usado como referência para preparar o ambiente local).  

É recomendado que o deploy seja feito primeiro no function app de lab, para testes de execução. Após conclusão, deve-se criar um pull request, que será submetido à análise e aprovação da equipe de Governança Técnica, assim como feito para deploy no Data Factory, e após isso, o engenheiro fará o merge das branchs e o processo de CI/CD cuidará da publicação no ambiente produtivo. 

6.5 Databricks
===============

Na Ipiranga, utilizamos o Databricks com as linguagens **PySpark** e **Spark SQL** para manipular grandes conjuntos de dados, transformá-los, realizar limpeza nos dados, remoção de duplicidades, criação de novas tabelas, replicação de relatórios, seja na extensão CSV ou parquet, entre outras ações.

Os workspaces Databricks para desenvolvimento são criados por áreas, e para cada projeto, será concedido acesso ao workspace da área a qual ele pertence. Sendo assim, o desenvolvimento será feito no ambiente de LAB da área e, após o fluxo de produtização, passará para o workspace produtivo.

No workspace de LAB, é possível fazer leitura e escrita no storage stippdatalakelab, utilizando o ponto de montagem **/mnt/[área]/dev/**, e apenas leitura no storage produtivo, stippdatalakedev, utilizando o ponto de montagem **/mnt/[área]/prd/**. 

6.5.1 Organização
-------------------

A organização das pastas dentro do workspace (e repositório) deverá ser feita conforme padrão:

  ::

      [área] 
             [projeto A] 
                      config 
	 	 	     acessoCamadas.py 
	 	 	     criaListaCamadas.py

	 	      [assunto 1] 
	 	 	     [notebook 1] 
	 	 	     [notebook 2] 
	      [projeto B] 
 	 	      config 
                 	     acessoCamadas.py
              	 	     criaListaCamadas.py  	 	
                      [assunto 1] 
	 	 	     [notebook 1] 

	 	      [assunto 2] 
	 	 	     [notebook 1] 

	 	      [...] 

6.5.2 Pasta config
-------------------

No repositório das áreas, cada projeto deverá ter a própria pasta **config**. Dentro dela, deve haver 2 artefatos principais a serem usados em todos os notebooks, que são os arquivos **criaListaCamadas.py** e **acessoCamadas.py**. 

Nesses notebooks, haverá duas variáveis importantes. A primeira, **escopoArea** é o nome da área a que pertence o workspace, e é usada para “construir” o ponto de montagem a ser usado nas funções de acesso a arquivos. A segunda, **escopoProjeto** é o nome do projeto, e é usada para construir o caminho no sistema de arquivos do Databricks onde serão gravados os arquivos de apoio. 

Notebook criaListaCamadas.py 

Nesse notebook devem ser especificados os diretórios e arquivos do storage que serão acessados pelos processos.  

São 3 chaves a serem preenchidas:

* **baseLeitura**, onde devem estar os diretórios acessados para leitura; 
* **baseEscrita**, onde devem estar os diretórios acessados para escrita; 
* **baseArquivos**, onde devem estar os nomes dos arquivos que serão acessados. 

 

    .. image:: /images/Imagem17.jpg
        

**Figura 17:** Exemplo de preenchimento do notebook criaListaCamadas.py

Se um diretório for acessado para leitura e escrita, deve ser especificado em ambas as chaves. 

A partir dessas chaves, 3 arquivos json serão criados no sistema de arquivos: **baseLeitura.json**, **baseEscrita.json** e **baseArquivos.json**. Eles serão usados no próximo notebook. 

    .. image:: /images/Imagem18.jpg

**Figura 18:** Exemplo de preenchimento do notebook criaListaCamadas.py 

Deve ser executado no início de cada processo que o utiliza, para garantir que os arquivos de base estejam sempre atualizados. 

Notebook acessoCamadas.py 

É um notebook padrão, que será disponibilizado na pasta do projeto na criação do ambiente. Define as variáveis e métodos a serem usados nos notebooks para acesso aos arquivos, para leitura e gravação. No notebook estão disponíveis instruções para uso, bem como alguns exemplos.  

A partir dos arquivos base criados no notebook **criaListaCamadas.py**, serão criados os dataframes usados nos métodos de leitura e escrita. 

    .. image:: /images/Imagem19.jpg

**Figura 19:** Exemplo do notebook acessoCamadas.py 

6.5.3 Orquestração de processos
----------------------------------

Assim como mencionado na seção de Logic Apps, toda orquestração de processos deve ser concentrada no ADF, de forma a centralizar o monitoramento em um único local. No caso do Databricks, deve ser criado um pipeline onde o notebook será chamado através de uma atividade **“Notebook”**. O uso de Jobs do ADB não é livre, qualquer necessidade nesse sentido deve ser levada para avaliação pela equipe de Arquitetura.  

6.5.4 Dados sensíveis
------------------------

Possuímos ainda um workspace Databricks, **dtb-ipp-sensiveis-prd**, utilizado para trabalhar com dados sensíveis, como dados cadastrais de nossos clientes, sejam dados bancários, CPF, endereço, telefone, email etc, tal como **dtb-ipp-sensiveis-dev**. Caso um projeto precise trabalhar com dados sensíveis, contidos no storage stippdatalakelgpdhml, através do Databricks, deve ser solicitado acesso ao workspace mencionado, pois é o único que possui acesso de leitura e escrita para esse storage. 

6.5.5 Boas práticas e orientações gerais
------------------------------------------

* Usar o **PySpark** no lugar do Python puro, uma vez que este inviabiliza o uso do processamento paralelo pelo Spark; 
* Caso necessário utilizar a biblioteca pandas, importar a do Spark, **pyspark.pandas**, uma vez que o Spark é multithread e seu código pode ser executado de forma distribuída; 
* Não fazer leitura de arquivos XLSX ou XLS no Databricks, uma vez que esta extensão reduz consideravelmente o desempenho do cluster. Para trabalhar com dados que originalmente possuem esse tipo de extensão, recomendados a utilização do Data Factory para conversão para parquet; 
* Não é permitido produtizar processos apontando para o workspace de LAB, portanto, toda alteração deve ser produtizada, ou os outputs corretos serão escritos apenas no storage de LAB; 
* Arquivos temporários de processos devem ser escritos na camada transient, conforme padrão de camadas definido; 
* Ao realizar escrita no storage, o Databricks gera arquivos temporários e com nomes fora do padrão usado pela Ipiranga. Para solucionar isso, usamos um método simples que faz a escrita inicial na camada transient, e depois faz cópia apenas do arquivo parquet para a camada final (trusted ou refined), renomeando conforme padrão. 

 
  ::

    # Grava arquivo no Lake, Cria Cópia [Origem >> Destino] renomeando o arquivo, ao final remove da camada transient 
    def copia_arquivo(path_escrita_rf,path_escrita_tt,df,nome_arquivo): 
        # Grava Arquivo na Camada TT 
        (df.coalesce(1) 
            .write 
            .format("parquet") 
            .mode("overwrite") 
            .save(path_escrita_tt,header=True) 
        ) 
        # Lista Arquivo 
        arquivo = dbutils.fs.ls(path_escrita_tt)[-1][0] 
 
        # Realiza Cópia da camada Transient para a camada Refined atribuindo novo nome     
        dbutils.fs.cp(arquivo,path_escrita_rf+nome_arquivo)  
        # Remove os dados gravados na Transient     
        dbutils.fs.rm(path_escrita_tt,True) 

6.5.6 Fluxo de produtização
-----------------------------

No DevOps do projeto, o engenheiro deverá criar uma branch no repositório do Databricks, **prj-[área]-adb**, a partir da branch master. 

    .. image:: /images/Imagem20.jpg

**Figura 20:** Exemplo de criação de branch para repositório Databricks 

No workspace Databricks de LAB, os notebooks que serão produtizados, sejam alterações ou novos desenvolvimentos, devem ser associados a esta nova branch, indo na opção **Revision History** e clicando na opção **Git** (onde deve aparecer, inicialmente, “Git: Not Linked”) 

    .. image:: /images/Imagem21.jpg

**Figura 21:** Exemplo de associação de notebook Databricks a uma branch 

Ali, mudar status para **Link**, inserir o link de clone do repositório e selecionar a branch criada. O path do repositório deve respeitar o padrão indicado na seção 6.5.1, incluindo a estrutura de diretórios abaixo do nível “notebooks/”. 

    .. image:: /images/Imagem22.jpg

**Figura 22:** Exemplo de configuração para versionamento de notebook 

Após salvar essa configuração, deve-se adicionar o comentário de versionamento para fazer o commit na branch: 

    .. image:: /images/Imagem23.jpg

**Figura 23:** Exemplo de versionamento de notebook 

    .. image:: /images/Imagem24.jpg


**Figura 24:** Exemplo de versionamento de notebook 
 
Então, a estrutura criada, e alterações feitas deverão constar na branch 

    .. image:: /images/Imagem25.jpg


**Figura 25:** Exemplo de branch atualizada após commit 

Cada nova alteração no notebook deve ser commitada por esse mesmo menu de **Revision History**, através da opção **Save now**. Caso não seja feito, não será sincronizado com a branch a ser produtizada. Para checar se a alteração foi salva, pode-se consultar o histórico de commits na aba **History** de cada notebook da branch: 

    .. image:: /images/Imagem26.jpg

**Figura 26:** Exemplo de tela de histórico de commits de um notebook no repositório 

Realizado esse processo para cada notebook alterado, deve ser criado um Pull Request para a branch, adicionando um título que identifique a alteração, e uma descrição de cada alteração/desenvolvimento feito e o objetivo de cada um. Caso seja correção de erro em produção, incluir evidências de que ele foi solucionado. Preenchidas as informações e criado o PR, este cairá para avaliação do time de Governança Técnica, que fará análise e aprovação das alterações. 

Após a aprovação do PR, o pipeline de CI/CD fará a passagem das alterações para o ambiente produtivo, workspace **dtb-ipp-prd**. 
