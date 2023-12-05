5. Desenvolvimento de pipelines
++++++++++++++++++++++++++++++++

5.1. Orientações Gerais
========================

5.1.1. Ingestões com origem em bancos de dados
-----------------------------------------------

Os datasets permitidos para uso em novos desenvolvimentos ou manutenções são os listados no documento `Data Factory - Datasets Padrão <https://grupoultracloud.sharepoint.com/:x:/r/sites/ipp-portalgestaodados/Documentos Compartilhados/Analytics/Engenharia/Data Factory/Data Factory - Datasets Padr%C3%A3o.xlsx?d=w0f545456bf7048dab8c0c5f157cccc34&csf=1&web=1&e=QU0BPe>`_. A criação de novos datasets será permitida apenas em caso de necessidade, quando os já existentes não atenderem ao novo processo sendo construído, conforme especificado na seção 3.1

Para os datasets referentes à bancos de dados, pode ser necessário solicitar acesso para a tabela a ser ingerida. Sendo assim, sempre que for iniciado o desenvolvimento de uma nova ingestão de banco de dados, a orientação é que seja testado, usando os datasets que constam no documento mencionado anteriormente, se é possível realizar um preview na tabela.  

    Caso a tentativa retorne um erro ORA-00942, é um indicativo de que:

    * A tabela não existe na instância do banco informada; ou 
    * O usuário padrão do Data Lake não possui acesso para a tabela.  
  
Exemplo: tentativa de leitura da tabela DMCONTR.AG_CONTRATO_CFP_PESSOA_AM, existente em ORABI1, mas para a qual o usuário do Data Lake não possui acesso.

   .. image:: /imagesdf/Imagem7.jpg

   **Figura 7:** Exemplo de erro ORA-00942 

Nesse caso, é necessário criar uma tarefa para o time de Administração de Dados (via ALM ou Jira), solicitando os acessos necessários para a tabela, para o usuário de banco: 

* Oracle (ORASN, ORABI, ORABI1, ORABIF, JDE): usuário DLETL 
* ABADI: usuário ABADIDL 
* SQL Server (sqldb-datalake-bi): usuários são definidos por projeto e o acesso às tabelas será concedido conforme necessidade de cada um (verificar com o time de AD ao abrir a solicitação). 

5.1.2. Arquivos de parâmetros dos pipelines
--------------------------------------------

Foi adotada a prática de utilização de parâmetros em pipelines e dataflows, para que em casos de manutenções, a alteração seja concentrada num único ponto. Algumas regras a serem seguidas: 

    * Os arquivos de parâmetros devem ser carregados na pasta **“data/_conf/”**, em formato JSON; 
    * O nome de um arquivo deve sempre refletir o nome do pipeline a que pertence, sem o prefixo **“pip_”**;
    * É necessário incluir no JSON uma chave referente às informações do processo. Na descrição do processo, deve-se informar também o nome do engenheiro responsável pelo desenvolvimento, consultoria a que pertence e projeto atendido:

    ::

        "processInformation": { 
                "application": "DataFactory", 
                "resourceName": "adf-ipp-datalake-dev", 
                "applicationName": [nome do pipeline], 
                "processDescription": [breve descrição do processo] 
                        } 

    * Outras chaves do json são referentes às origens e destinos do processo, conforme exemplificado nas seções destinadas a ingestões, mas em suma, seguem o padrão abaixo: 

    ::

        "dataset[Destino ou Origem][Camada]": { 
                "fileSystem": [container em que o dado será salvo ou lido], 
	    	"directory": [camada e diretório em que o dado será salvo ou lido], 
                "fileName": [nome do arquivo com extensão], 
	   	"compression": [tipo de compressão, normalmente snappy] 
                }

    * Os parâmetros devem ser passados pela opção **Add dynamic content**, exibida para cada parâmetro dos datasets. Ali são adicionados valores dinâmicos, que podem ser saídas de steps anteriores do pipeline ou funções que calcularão valores. 

   .. image:: /imagesdf/Imagem8.jpg 

   **Figura 8:** Exemplo de passagem de parâmetros para dataset 

5.1.3. Criação de processos genéricos
--------------------------------------

É proibida a criação de processos genéricos para carga de bases de dados ou outros fluxos. Por processo genérico entende-se pipelines parametrizados de forma que atendam a mais de uma necessidade, passando apenas os parâmetros necessários para executá-lo.  

Por exemplo, pipeline para carga de tabelas da instância ORABI, em que o owner e nome da tabela são parametrizados, e outros processos fazem chamada a esse pipeline passando os parâmetros para carga da tabela que desejam.

A orientação oficial é de que sejam criados pipelines individuais para cada novo processo, de forma que o monitoramento dessas cargas seja feito de forma mais fluida e facilmente identificável. 

5.1.4.	Definição de agendamento de execução 
---------------------------------------------

Para agendamento de processos específicos para squads ou projetos, o horário de execução deve ser alinhado com o demandante do desenvolvimento (analista de negócio, líder de projeto ou cientista de dados). 

Para processos cuja origem são bancos de dados, deve-se verificar o horário de execução do processo de carga na origem para definir o melhor horário de carga do dado no Data Lake. Algumas formas de verificar:  

* Pelas colunas de data de inclusão e/ou alteração (DT_INCL e DT_ALTER) das tabelas, buscando pela faixa de horário mais comum; 
* Verificação do horário e frequência de agendamento da carga. Esta opção demanda uma consulta à equipe de sustentação, ao Power Center e/ou ao documento de procedimentos de carga para produção disponível no `Portal ASG <https://grupoultracloud.sharepoint.com/:f:/r/sites/ipp-portalgestaodados/Documentos%20Compartilhados/Geral/ETL/Especifica%C3%A7%C3%A3o%20de%20Procedimento%20de%20Carga%20-%20Produ%C3%A7%C3%A3o/PowerCenter?csf=1&web=1&e=OIZMlA>`_.
  
5.1.5.	Dados sensíveis
------------------------

Quando for verificado que um dado sendo ingerido contém informação sensível, a ingestão do mesmo deve ser direcionada para um storage apartado, ao qual o acesso é concedido mediante alinhamento com justificativa da necessidade. Esse direcionamento é válido para qualquer tipo de base de dados, seja ela interna ou externa, vindo de sistemas ou sendo input de usuários. 

O engenheiro responsável pelo desenvolvimento do processo deve sempre verificar o schema da base sendo ingerida, de forma a garantir que informações sensíveis sejam direcionadas para o local correto. 

    Algumas informações importantes sobre o storage de dados sensíveis:

    * Storage account: stippdatalakelgpdhml 
    * Container: dados-sensiveis
    * Estrutura de pastas: deve seguir o mesmo padrão do storage produtivo, mas o container usado é “dados-sensiveis” 
    * Caso haja necessidade de tratamento do dado via Databricks, deve ser solicitado acesso ao workspace destinado a este fim, **dtb-ipp-datalake-dados-sensives.** 

5.1.6.	Utilização de Integration Runtime 
------------------------------------------

Existe uma integration runtime otimizada que deve ser usada no desenvolvimento dos pipelines, **AutoResolveIntegrationRuntimeTTLMem**. Ela deve ser selecionada na chamada de execução de dataflows, na aba **Settings**, como na Figura abaixo. 

   .. image:: /imagesdf/Imagem9.jpg

   **Figura 9:** Exemplo de seleção de Integration Runtime para Data flow 

5.1.7.	Timeout de atividades de pipeline
------------------------------------------

Ao ser adicionada uma nova atividade a um pipeline, esta vem com um timeout padrão de 7 dias, porém, todas as atividades deverão ser parametrizadas para o timeout de até 1 dia (formato D.HH:MM:SS), na aba **General** da atividade em questão. 

5.1.8.	Concorrência de pipelines
-----------------------------------

Por padrão, não há um número máximo de execuções concorrentes que um pipeline pode ter. Porém, é interessante definir a concorrência para 1, de forma a evitar que, por qualquer motivo, múltiplas execuções de um mesmo pipeline se sobreponham, podendo causar erros. Dessa forma, outras execuções que tentem iniciar, serão colocadas em uma fila para aguardar o término da atual. 

Nas configurações gerais do pipeline, na aba **Settings**, é possível definir esse número

   .. image:: /imagesdf/Imagem10.jpg

   **Figura 10:** Exemplo  de configuração de concorrência de pipeline 

5.2. Ingestão de tabela dimensão – Oracle
==========================================

Para tabelas dimensão, a carga deve ser feita de forma full, devido ao fato de o volume de dados dessas bases ser, em geral, baixo. Podem existir casos em que o volume de uma dimensão seja extremamente acima do comum, e para esses casos, é recomendado que a carga seja incremental, como será descrito na próxima seção. 

Pipelines de ingestão de dimensões possuem tipicamente 3 atividades: 

* Lookup para o arquivo de parâmetros; 
* Atividade de cópia, com origem no Oracle e destino no storage, na camada transient; 
* Atividade de cópia, com origem no Azure Synapse e destino no storage, na camada trusted. 

   .. image:: /imagesdf/Imagem11.jpg

   **Figura 11:** Exemplo de pipeline de ingestão de dimensões 

O objetivo da passagem do dado pelo Synapse é converter os tipos de dados de algumas colunas, que são trocados automaticamente pelo Data Factory para origens Oracle, por exemplo, inteiros sendo interpretados como double. Para isso, é passada uma query com um **cast** das colunas necessárias para o tipo correto. O pipeline **pip_dm_tipo_projeto_synapse** pode ser usado como referência. 

O step referente ao Synapse é necessário apenas quando a dimensão em questão se enquadra no caso mencionado acima. Se as colunas da tabela são todas do tipo varchar, por exemplo, não ocorre o problema de troca de tipos, e, portanto, basta a atividade de cópia do Oracle diretamente para a camada trusted, sem passar o dado pelo Synapse. 

5.3. Ingestão de tabela fato e agregada – Oracle
=================================================

Tabelas fato e agregadas são naturalmente maiores em volume de dados, de forma que realizar carga full delas se torna inviável. Portanto, para esses casos desenvolvemos processos de carga incremental, que verificará, a cada execução, os registros que foram incluídos no banco de dados desde a última carga para o Data Lake. 

Normalmente é necessário realizar uma carga histórica inicial, trazendo os dados dos últimos x meses ou anos para as tabelas que serão usadas num novo desenvolvimento. Esse período de dados deve ser alinhado entre o engenheiro e o demandante (analista de negócio, líder de projeto ou cientista de dados). Não é necessário buscar todo o histórico de uma tabela, a menos que seja solicitado (e nesse caso, o histórico deve ser trazido aos poucos para o storage, por mês ou ano a depender da volumetria, e particionado no mesmo padrão usado para a carga incremental da tabela). 

Diferente do que ocorre para dimensões, os pipelines de tabelas fato e agregadas possuem mais atividades: 

* Lookup para o arquivo de parâmetros;
* Lookup para o arquivo de controle de carga, que contém a data a ser utilizada para trazer os dados; 
* Atividade de cópia, com origem no Oracle e destino no storage, na camada transient; 
* Condição, que verificará se a atividade de cópia trouxe algum dado;
* Dataflow, para particionamento dos dados*;
* Atividade de deleção para o arquivo inicial.
  
   Foi convencionado o uso do Databricks para realizar esta operação de particionamento, desta forma o Dataflow passa a ser substituído pelo `Databricks <https://grupoultracloud.sharepoint.com/:f:/r/sites/ipp-portalgestaodados/Documentos%20Compartilhados/Analytics/Engenharia%20de%20dados/Databricks?csf=1&web=1&e=zve1Q8>`_. Consulte a documentação do Databricks no tópico: **Tabelas on-premises** para referência de como seguir com o uso 

   .. image:: /imagesdf/Imagem12.jpg

   **Figura 12:** Exemplo de pipeline de ingestão de tabelas fato 

Iniciando pelo arquivo de parâmetros, além da primeira chave com os dados sobre o processo, mencionada na seção 5.1.2, ele deve conter algumas outras: 

* Origem do dado 
  
  ::

            "datasetOrigem": { 
                    "srcScheme": "[owner da tabela no banco]", 
	            "srcTable": "[nome da tabela]",   
                    "srcQuery": "[SQL para consulta dos dados na origem, considerando incremental]" 
            }


* Destino temporário, camada transient 

    ::

        "datasetDestinoTransient": { 
                "fileSystem": [container em que o dado será salvo], 
    	        "directory": [camada e diretório em que o dado será salvo], 
                "fileName": [nome do arquivo com extensão], 
	        "compression": [tipo de compressão, normalmente snappy] 
        } 

* Destino final, camada raw ou trusted 

    ::

        "datasetDestino": { 
                "fileSystem": [container em que o dado será salvo], 
	        "directory": [camada e diretório em que o dado será salvo], 
                "fileName": [nome do arquivo com extensão], 
	        "compression": [tipo de compressão, normalmente snappy] 
        } 


* Informações para controle de carga 

    ::

      "datasetControleCarga":{ 
            "dtControleCarga": [coluna que será usada para controle da carga incremental, normalmente DT_INCL], 
            "dtControle": [coluna de data que será usada para particionamento do dado], 
            "jsonFileSystem": [container em que JSON de controle será salvo], 
            "jsonDirectory": [diretório onde o JSON será salvo, padrão: "_conf/controle_data"],
            "jsonFileName": [nome do arquivo JSON, padrão: dt_controle_[nome da tabela].json] 
            } 
  
A verificação de novos registros é tipicamente feita pela data de inclusão e/ou alteração, como no exemplo a seguir. A query busca todos os registros da tabela que foram incluídos ou alterados desde a data % (caractere substituído pelo valor recuperado na segunda lookup, do arquivo de controle de carga), até a data $ (caractere substituído pela data do início de execução do processo). 

    ::

        select * 
          from DMFRANQ.PR_VENDA_ROYALTIES 
         where (trunc(DT_INCL) > to_date('%', 'YYYY-MM-DD') and
                trunc(DT_INCL) < to_date('$', 'YYYY-MM-DD'))        
            or (trunc(DT_ALTER) > to_date('%', 'YYYY-MM-DD') and 
                trunc(DT_ALTER) < to_date('$', 'YYYY-MM-DD')) 

A query usada pode ser adaptada ou reformulada conforme necessidade do processo, de acordo com a lógica da carga dos dados na origem. Para isso, é importante fazer uma análise do processo de carga sempre que possível, de forma a entender a periodicidade com que os dados são inseridos ou atualizados, como são atualizados, se existe reprocessamento e outras questões semelhantes. 

Dito isso, o arquivo inicial será gravado na camada transient, e a atividade de condição verificará se o número de linhas que a atividade de cópia retornou é maior que zero. Isso é necessário pois a tabela pode não ter sido atualizada na origem até o momento da carga. Em caso positivo (True) o fluxo passa por um dataflow interno à condição. Em caso negativo, segue adiante e deleta o arquivo da camada transient. 

A seguir, os dados trazidos serão lidos por um dataflow que realizará o particionamento desses dados na camada seguinte, raw (ou trusted, caso a lógica de carga usada contemple reprocessamento e não gere duplicidade de registros). O particionamento é feito por data e, para isso, é necessário definir a coluna que será usada.  

Sempre que possível, recomenda-se particionar o dado pela data de referência da tabela em questão (por exemplo, DT_REF, DT_TRANS, NO_AM), o que permite maior transparência na organização dos dados e consequente otimização da leitura dos mesmos por processos. Quando não, o particionamento é feito pela data de inclusão/alteração dos registros. Nesse caso, se o registro possuir data de inclusão e alteração preenchidas, deve ser considerado para o controle a data mais recente. 

   .. image:: /imagesdf/Imagem13.jpg

   **Figura 13:** Exemplo de steps de um Data flow 

Após a leitura da transient, o segundo e terceiro steps criarão as colunas que apoiarão o particionamento do dado. Serão criadas as colunas de ano, mês e dia, com base na data de referência escolhida para a carga (caso essa data seja no nível mês, a coluna de dia não se faz necessária). Além disso, essa atividade de Derived Column pode ser usada para converter os tipos de dados de outras colunas da base. 

O quarto step é onde se define a “partição” onde cada registro será salvo, com base numa coluna string, de nome “path” por exemplo, que é montada a partir da concatenação de alguns parâmetros passados para o dataflow, com as colunas de data definidas nos steps anteriores: 

   .. image:: /imagesdf/Imagem13-1.jpg

Os três parâmetros se referem a, respectivamente, camada, diretório e nome do arquivo do Data Lake onde o dado será gravado, seguindo os padrões de camadas definidos. No exemplo usado aqui, para um registro cuja coluna NO_AM possui o valor “202111”, a string ficaria da seguinte forma: 

**raw/dados_internos/bi/dmfranq/pr_venda_royalties/2021/11/rw_pr_venda_royaltie s_202111.parquet**

A seguir, o fluxo se divide em dois. O primeiro passará por um select, que irá manter apenas as colunas originais da tabela sendo ingerida, mais a coluna “path” criada. Nesse step, é importante selecionar, na aba **Optimize**, a opção de particionamento por chave, usando a coluna path. 

   .. image:: /imagesdf/Imagem14.jpg

   **Figura 14:** Exemplo de configuração de particionamento por chave

Passa-se então para o step final, o **Sink**, onde o dado é finalmente gravado. É aqui onde o particionamento é realmente definido. Na aba Settings, a opção **File name option** permite que que o diretório dos dados seja definido com base em uma coluna. Dessa forma, cada registro será direcionado para um arquivo, de acordo com a string path que foi montada para ele. 

   .. image:: /imagesdf/Imagem15.jpg

   **Figura 15:** Exemplo de configuração do particionamento dos dados do Data flow 

O segundo fluxo é onde será definida a data de controle para a próxima carga. Uma atividade de agregação obterá a maior data de inclusão (DT_INCL) existente nos registros, e em seguida, gravará esse valor em um arquivo JSON que será lido pela segunda lookup do início do processo. 

Pipelines que podem ser usados como referência: 

* Informação nível ano mês: pip_pr_venda_royalties; 
* Informação nível dia, tabela possui colunas de data de inclusão e alteração: pip_pr_pesquisa_prc_bomba. 
  
5.4. Ingestão de view – Oracle
===============================

Para o caso de views existentes nas bases de dados da Ipiranga, a orientação é que não seja feita ingestão desses objetos, para evitar a oneração do ambiente. Quando houver necessidade de uso de uma view para desenvolvimento de um processo, devese fazer ingestão das bases envolvidas no DDL e reproduzir a query no processo que a utilizará. 

A depender da view, pode ser construído um processo via Databricks, que consumirá do storage os dados das tabelas envolvidas na view, reproduzirá o DDL e salvará o arquivo resultante no storage (seguindo o mesmo padrão de diretórios usado para tabelas do Oracle). 

5.5. Debugando pipelines e dataflows
=====================================

Para testar se as saídas de cada atividade estão conforme o esperado, usamos o recurso de **Debug** do Data Factory. 

Começando pelo dataflow, para visualizar os outputs intermediários dos steps internos, usamos o **Data Preview** de cada um deles. Mas para isso, precisamos iniciar o **Data flow debug:** 

   .. image:: /imagesdf/Imagem16.jpg

   **Figura 16:** Exemplo de ativação do Data flow debug 

Com ele ligado, é necessário informar o conteúdo de cada parâmetro usado pelo dataflow, em **Debug Settings > Parameters**. Ali, os parâmetros devem ser preenchidos exatamente como estão no arquivo JSON do processo. Feito isso, basta dar um **Refresh** em **Data Preview.** 

Importante mencionar que o debug de um dataflow apenas gera previews dos dados, não realiza qualquer alteração no storage. Já o debug de um pipeline faz uma execução real, e, portanto, deve ser usado com cautela. 

Ainda sobre pipelines, é possível também debugar até certo ponto do fluxo, com a opção **Debug until**. Ao clicar em uma atividade do pipeline, aparecerá no canto superior direito dela um círculo vermelho não preenchido, que indica que a opção de debug until está desabilitada. 

Por exemplo, caso o desenvolvedor queira testar apenas as saídas dos arquivos de parâmetros, basta selecionar a última lookup e habilitar a opção. O círculo vermelho ficará preenchido, e as atividades seguintes ficarão “apagadas”: 

   .. image:: /imagesdf/Imagem17.jpg

   **Figura 17:** Exemplo de uso do “Debug until” 

Feito isso, basta clicar em Debug para iniciar a execução do trecho do pipeline habilitado. O resultado pode der visualizado na aba Output, onde o ícone   exibirá a entrada recebida pela atividade de lookup, e o ícone   exibirá a saída, que, no nosso exemplo, será o conteúdo do arquivo JSON. 

   .. image:: /imagesdf/Imagem18.jpg

   **Figura 18:** Exemplo de output do pipeline 

