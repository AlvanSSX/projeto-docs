3. Padrão para criação de objetos
++++++++++++++++++++++++++++++++++

Para toda e qualquer criação de objetos no Data Factory, deve-se incluir em sua descrição comentários a respeito do objetivo e/ou funcionamento dele, conforme especificações feitas nesta seção. Além disso, os nomes de objetos devem seguir o padrão: 

**[prefixo]_[descrição abreviada].**

As abreviações devem seguir o Dicionário de Abreviaturas disponível no `Portal Gestão de Dados <https://grupoultracloud.sharepoint.com/sites/ipp-portalgestaodados>`_. Com exceção dos steps internos do Dataflow e da branch, todos dos nomes de objetos devem ser escritos em letras **minúsculas**. Além disso, todas as atividades deverão ser parametrizadas para o **timeout** de até 1 dia.  Além disso, todas as atividades deverão ser parametrizadas para o tempo de **timeout** médio das últimas execuções + 1H acrescida por margem de segurança. Serão necessárias anexar evidências no pull requests.

3.1. Datasets
==============

A orientação em relação a datasets é de que sempre sejam utilizados os que já existem, conforme documento `Padrão de Datasets <https://grupoultracloud.sharepoint.com/:x:/r/sites/ipp-portalgestaodados/Documentos Compartilhados/Analytics/Engenharia/Data Factory/Data Factory - Datasets Padr%C3%A3o.xlsx?d=w0f545456bf7048dab8c0c5f157cccc34&csf=1&web=1&e=uN3feH>`_. Novos datasets só devem ser criados se nenhum dos existentes atender a necessidade do processo, e, caso o dado seja de uma origem ainda não mapeada por nenhum linked service existente, a criação deve ser solicitada ao time de Arquitetura, conforme especificado na seção destinada a linked services. Os nomes devem seguir o padrão abaixo: 

**[prefixo tipo dataset]_[usuário de banco]_[instância do banco]** ou

**[prefixo tipo dataset]_[descrição da parametrização]** ou

**[prefixo tipo dataset]_[nome da storage account]_[descrição da parametrização]**, quando o dataset apontar para outro storage que não o produtivo, **stippdatalakedev**; ou

**[prefixo tipo dataset]_[usuário]_[ambiente]**, se o dataset apontar para o Salesforce


===============     ========================
Prefixo 	            Tipo 
===============     ========================
azdtb_deltalake 	Delta Lake 
azsql 	            Banco SQL Server 
binario 	        Arquivo Binário 
csv 	            Arquivo CSV 
json 	            Arquivo JSON 
mysql 	            Banco MySQL 
odbc 	            Cache (Abadi, Alphalinc) 
ora 	            Banco Oracle 
parquet 	        Arquivo Parquet 
sf 	                Salesforce 
syn 	            Azure Synapse 
txt 	            Arquivo TXT 
xlsx 	            Arquivo Excel 
===============     ========================

**Tabela 1**: Prefixos de datasets

**Exemplos:**

* azsql_datalake_bi
* csv_param_file_name
* json_param_file_system
* odbc_abadidl_cache_prd
* ora_dletl_ipjdep
* ora_dletl_orabi
* parquet_ippstgdevatena_param_file_name (parquet que aponta para outro storage)
* parquet_param_file_name
* sf_integrsalesfc_ipirangarede (ambiente produtivo do Salesforce)
* xlsx_param_file_range

3.2. Pipelines
===============

Na criação de pipelines, deve-se seguir o seguinte padrão para nomenclatura: 

**pip_[nome da tabela]**, no caso de ingestões com **origem** em bancos de dados, ou 

**pip_azsql_[nome da tabela]**, no caso de processos de **carga em** tabelas no Azure SQL; ou 

**pip_[descrição sucinta]**, para outros processos 

Todos os pipelines deverão conter, **obrigatoriamente**, em sua descrição:

* Nome do engenheiro responsável pelo desenvolvimento e consultoria a qual pertence;
* Área solicitante (squad, projeto ou BU);
* Explicação da lógica que está sendo utilizada e da regra de negócio. Se possível, deve-se incluir exemplos

**Exemplo 1:**

Nome: pip_pr_pneg_componente_propneg

Descrição: Pipeline responsável pela cópia de dados da tabela PR_PNEG_COMPONENTE_PROPNEG em ORABI para o lake na camada raw.

Engenheiro: Robson Cesar Gomes Quintino

Squad solicitante: Site Location

**Exemplo 2:**

Nome: pip_interface_sitelocation_salesforce

Descrição: Pipeline para preparação dos dados gerados pela squad de Site Location, a cerca de volume potencial mensal para postos, para carga na base INTEGRSALESFCMV.CG_VOLUME_POTENCIAL_POSTO_MES, para uso em integração do Salesforce.

Engenheiro: Ingrid de Oliveira Canaane

Squad: Site Location

3.2.1. Atividades do pipeline
------------------------------

Para atividades internas a um pipeline, seguir o padrão:

**[prefixo atividade]_[descrição sucinta]**

=========  ================
Prefixo    Atividade
=========  ================
af         Azure Function
appvar     Append variable
cp         Copy Data
dl         Delete
dtb        Databricks
fail       Fail
fe         ForEach
ft         Filter
getmet     Get Metadata
if         If Condition
lk         Lookup
pip        Execute Pipeline
scpt       Script
setvar     Set variable
sp         Stored Procedure
st         Switch
until      Until
vl         Validation
web        Web
wh         WebHook
wt         Wait
=========  ================

**Tabela 2:** Prefixos de atividades do pipeline

Cada atividade deve ter um nome explicativo com uma descrição sucinta sobre seu funcionamento, que faça sentido.

**Exemplo 1:**

Nome da atividade: dtb_ntb_armazenagem_terceiros

Descrição: Faz chamada do notebook dtb_etl_armazenagem_terceiros, que faz carga dos dados de armazenagem em e para terceiros.

**Exemplo 2:**

Nome da atividade: cp_carga_orabi_transient

Descrição: atividade que realiza a cópia dos dados incrementais da tabela em ORABI para o storage, na camada transient

3.3. Dataflows
===============

O padrão de nomenclatura para dataflows é o seguinte:

df_[nome da tabela] ou

df_[descrição do objetivo]

É necessário incluir a descrição do objetivo do fluxo criado, e especialmente para dataflows que possuam regras complexas, deve-se incluir uma explicação da lógica aplicada. Dessa forma, facilita o entendimento do processo em futuras necessidades de manutenção. 

**Exemplo:**

Nome: df_ag_lancamento_conta

Descrição: Dataflow responsável pelo particionamento dos dados da carga incremental da tabela AG_LANCAMENTO_CONTA na camada raw, utilizando a coluna NO_AM, e gravação da última data de inclusão dos registros ingeridos no arquivo json para controle de carga.

3.3.1. Steps internos do dataflow
----------------------------------

Para atividades internas de dataflows, deve-se incluir o prefixo do tipo de step, mas diferentemente de outros objetos, pode-se utilizar letras maiúsculas para definição do nome.

**[prefixo step][descrição sucinta]**

======= ====================
Prefixo Step
======= ====================
agg     Aggregate
alt     Alter Row
dc      Derived Column
ex      Exists
fl      Flatten
ft      Filter
jn      Join
lk      Lookup
ps      Parse
pv      Pivot
rk      Rank
sk      Surrogate Key
sk      Sink
sl      Select
spl     Conditional Split
sr      Source
st      Sort
str     Stringify
un      Union
upv     Unpivot
wd      Window
======= ====================

Obs.: “New Branch” adiciona novo fluxo, o nome ficará o mesmo do step selecionado.

**Tabela 3: Prefixos de steps do dataflow**

**Exemplos:**

* aggCountReg
* altUpdateReg
* dcFormataCols
* exData
* flRegistros
* ftDataNull
* jnDmVenda
* lkDmVenda
* pvValoresVenda
* skIdVenda
* skJson
* slRenomeiaCols
* splMesAnterior
* srTransient
* stRegistros
* unDmPontoVenda
* unpValoresVenda
* wdRankRegistros

3.4. Parâmetros
================

Os parâmetros de pipelines e dataflows devem ser criados com base no padrão: **p_[nome do campo]**

Deve-se evitar o uso de nomes genéricos, procurando nomear com termos que descrevam bem o significado do parâmetro. Por exemplo, **p_data_ult_carga**, no lugar de **p_data1**.

3.5. Triggers
==============

A criação de triggers, assim como de datasets, só deve ser feita se nenhuma das existentes atender a necessidade do processo, e, nesse caso, o nome deve seguir o padrão abaixo:

**trg_[tipo]_[horário de execução]** ou **trg_[tipo]_[descrição do agendamento]**

=========   =========
Tipo 	    Descrição
=========   =========
schedule 	Agendamento feito por definição do horário e frequência de execução. Utilizar preferencialmente o horário de Brasília (UTC-3). 
event 	    Triggers por evento 
=========   =========

**Tabela 4:** Identificadores de tipos de triggers 

**Exemplos:**

* trg_schedule_8_am – execução diária às 8h
* trg_schedule_7_am_weekdays – execução às 7h, apenas em dias úteis
* trg_schedule_6_am_once_a_week_monday – execução às 6h, às segundasfeiras
* trg_event_stone – disparada sempre que um arquivo é criado no diretório especificado

A descrição de uma trigger deve conter a especificação de seu horário e frequência de execução, e no caso de triggers por evento, descrição deste evento. 

**Exemplo:**

* Nome: trg_event_fieldcontrol
* Descrição: Trigger responsável por identificar a criação de um arquivo na pastaraw/dados_externos/field_control/ e iniciar o pipeline pip_interface_fieldcontrol. Estes arquivos serão disponibilizados pela function 3 vezes por dia.

3.6. Linked Services
=====================

Caso necessário, a criação de linked services deve ser solicitada ao time de Arquitetura, que seguirá o padrão de nomes abaixo: 

**ls_[prefixo linked service]_[referência ou descrição]** ou

**ls_[prefixo linked service]_[usuário]_[instância do banco]**, para linked services que fazem conexão com bancos de dados; ou

**ls_[prefixo linked service]_[instância do banco]**, para linked services que fazem conexão com bancos externos; ou

**ls_[prefixo linked service]_[nome do workspace]**, para linked services que fazem conexão com workspaces do Databricks;

**ls_[prefixo linked service]_[usuário]_[ambiente]**, se o dataset apontar para o Salesforce.

==========  ==========
Prefixo 	Linked Service 
==========  ==========
af 	        Azure Function 
azsql 	    SQL Server 
deltalake 	Delta Lake 
dl 	        Storage Gen2 
dtb 	    Databricks 
fs 	        File System 
http 	    HTTP 
mysql 	    MySQL 
odbc 	    Cache 
ora 	    Banco Oracle 
sf 	        SalesForce 
sftp 	    SFTP 
syn 	    Azure Synapse 
==========  ==========

**Tabela 5:** Prefixos de linked services

**Exemplos:**

* ls_af_fontesexternas_abegas
* ls_dl_stippdatalakedev
* ls_dtb_ipp_dev
* ls_mysql_ipiranga_piloto_dt_clean
* ls_ora_dletl_orabi
* ls_sf_integrsalesfc_ipirangarede

A descrição de um linked service deve conter seu objetivo, usuário usado e instância de banco acessada, quando for aplicável.

3.7. Integration Runtimes
==========================

Não se deve fazer criação de integration runtimes, uma vez que já existem IRs para as necessidades comuns do Data Factory. Qualquer necessidade nesse sentido deve ser levada para o time de Arquitetura, que avaliará o pedido. 

Integration runtimes são nomeadas conforme o nome da máquina virtual à qual faz referência, e informando em sua descrição o objetivo. 

**ir_[nome da vm]**

**Exemplo**: ir-vmdatalakeir-dev 