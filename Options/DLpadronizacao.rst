4. Padronização 
++++++++++++++++

Em todas as camadas, os nomes dos arquivos deverão conter o prefixo correspondente à abreviatura da camada, conforme Tabela 2. 

   .. image:: /imagesdl/dl2.png

**Tabela 2:** Abreviatura de nomes das camadas 

4.1. Camadas Transient, Raw e Trusted
=======================================

Nestas camadas, os dados serão organizados por sua origem, entre dados internos, oriundos de sistemas internos ou áreas de negócio da Ipiranga, e dados externos, que são oriundos de sistemas externos à Ipiranga. 

Dessa forma, os três níveis iniciais da estrutura de diretórios deverão ser: 

**data/[camada]/[origem do dado]/**

4.1.1. Bancos de dados
------------------------

No caso de informações oriundas de bancos de dados, o quarto nível na estrutura de diretórios deverá conter o nome correspondente a cada instância, conforme Tabela 3 abaixo.

Para as instâncias ORABI, ORABIF e ORABI1, usamos o agrupador “bi”. O agrupamento foi adotado devido a possibilidade de uma mesma tabela existir em instâncias diferentes (sinônimo público), o que pode acarretar ingestões duplicadas de uma mesma tabela em estruturas de diretórios diferentes

   .. image:: /imagesdl/dl3.png

¹ Para dados do BI, a instância precisa ser incluída no Catálogo de Dados 

² Dimensões compartilhadas entre instâncias devem ser extraídas a partir da instância ORABI. 

**Tabela 3**: Identificadores das instâncias de bancos de dados 

4.1.2. Regras gerais
----------------------

* Para dados internos, com origem em bancos de dados, deve-se usar na hierarquia de pastas o nome correspondente à instância (conforme Tabela 3);
* Para dados externos e internos, cuja fonte não seja banco de dados, deve-se adotar o nome do sistema de origem, seguido por assunto, e, por fim, a hierarquia de tempo (ano, mês e dia, se necessário); ▪ Para tabelas com carga full, como dimensões, não há obrigatoriedade de criação de subpastas de data. Para tabelas fato e agregadas, mesmo para carga full, pode ser interessante particionar por data. Nesse caso, avaliar com o time de Arquitetura;
* Ingestões incrementais devem possuir subdiretórios de data até o nível dia (dd) ou mês (mm), a depender da data de referência usada para a carga;
* Uso de subdiretórios no nível de hora, minuto e segundo (hh/mi/ss) é opcional, como para ingestões intraday;
* Um dado disponível na camada raw, ao passar pelos tratamentos necessários para torná-lo confiável, será levado para a camada trusted seguindo a mesma estrutura de pastas da camada anterior (mudando-se o prefixo da camada no nome do arquivo).  


4.1.3. Estrutura de Pastas
----------------------------

Dados de bancos de dados

   data/[camada]/dados_internos/[banco]/[owner]/[tabela]/[yyyy]/[mm]/[dd]/[hh]/[ mi]/[ss]/rw_tabela_[yyyymmdd]_[hhmiss].parquet

   * Exemplo 1: carga full, dimensões trusted/dados_internos/bi/dmpne/dm_indicador_franquia_projeto/rw_dm_in dicador_franquia_projeto.parquet  
   * Exemplo 2: carga incremental nível dia raw/dados_internos/bi/dmpne/pr_pneg_aluguel_volume_var/2021/11/14/rw_pr_pneg_aluguel_volume_var_20211114.parquet
   * Exemplo 3: carga incremental nível mês raw/dados_internos/bi/dmcontrato/ag_cntr_forn_prod_am/2021/11/rw_ag_c ntr_forn_prod_am_202111.parquet 

Carga de bancos de dados com aplicação de filtros (exceção) 

   data/trusted/dados_internos/views/[projeto]/v_[descrição do assunto da view]/[yyyy]/[mm]/[dd]/[hh]/[mi]/[ss]/rw_v_[descrição]_[yyyymmdd]_[hhmiss].parquet

   * Exemplo: carga incremental diária.  Tabela PRODDTA.F4111 do JDE, com aplicação de filtro de empresa igual a Ipiranga (ILKCO = '00007') trusted/dados_internos/views/operacoes/v_operacao_estoque_ipiranga/2021 /11/17/td_v_operacao_estoque_ipiranga_20211117.parquet  
   * Exemplo 2: carga full Tabela PRODDTA.F4311 do JDE, com aplicação de filtro de empresa igual a Ipiranga (PDKCOO = '00007') trusted/dados_internos/views/operacoes/v_item_pedido_compra_ipiranga/td _v_item_pedido_compra_ipiranga.parquet 


Arquivos fornecidos por usuários 

   data/[camada]/dados_internos/input_user/[área]/[assunto]/[subassunto]/[yyyy]/[ mm]/[dd]/[hh]/[mi]/[ss]/rw_tabela_[yyyymmdd]_[hhmiss].[extensão]

   Obs.: **[área]** diz respeito à área da empresa responsável por gerar o dado, e não a área ou projeto que o utilizará. Aqui tratamos os arquivos pelas origens dos dados.
   
   * Exemplo 1: carga diária raw/dados_internos/input_user/relacionamento_marketing/treinamento_pv/2 021/06/14/rw_treinamento_pv_ana_20210614.parquet  
   * Exemplo 2: carga diária em que pode acontecer mais de um envio no dia (a carga real está em uma estrutura antiga, aqui foi adaptado apenas para exemplo) raw/dados_internos/input_user/mercado_empresarial/custeio_importacao/20 21/11/22/20/32/rw_custeio_importacao_20211122_2032.xlsx   


Dados de fontes externas

   data/[camada]/dados_externos/[fonte]/[assunto]/[subassunto]/[yyyy]/[mm]/[dd]/ [hh]/[mi]/[ss]/rw_tabela_[yyyymmdd]_[hhmiss].parquet 

   * Exemplo 1: carga mensal raw/dados_externos/anp/base_postos_anp/2021/11/rw_base_postos_anp_20 2111.xlsx  
   * Exemplo 2: carga diária em que pode acontecer mais de um processamento no dia, mantendo-se cada um desses arquivos (a carga real está em uma estrutura diferente, aqui foi adaptado apenas para exemplo) raw/dados_externos/fieldcontrol/customers/2022/05/19/07/00/58/rw_custo mers_20220519_070058.parquet 
  
4.2. Camadas Refined e Serving
================================

Nestas camadas, os dados serão organizados por projeto, assunto e subassunto (caso necessário este último), o que irá compor os 3 primeiros níveis de pastas. O primeiro diz respeito ao demandante do dado, ou seja, projeto para/pelo qual o output foi gerado. Os outros dois, visam segregar os outputs conforme as informações contidas em cada um, sendo o nível subassunto opcional. 

O nível seguinte na estrutura diz respeito à visão gerada. Assim como para ingestões do Oracle o nome da tabela entra na hierarquia de pastas, aqui o nome da visão/output gerado deverá compor o diretório também. 

Sendo assim, a estrutura de diretórios deverá seguir o padrão: 

**data/[camada]/[projeto]/[assunto]/[subassunto]/[tabela ou visão gerada]**

4.2.1. Regras gerais
---------------------

* Ingestões incrementais devem possuir subdiretórios de data até o nível dia (dd) ou mês (mm), a depender da data de referência usada para a carga;
* Uso de subdiretórios no nível de hora, minuto e segundo (hh/mi/ss) é opcional, como para ingestões intraday. 
* Os nomes dos arquivos devem conter o prefixo da camada na qual estão contidos, como feito para as demais. 


4.2.2. Estrutura de Pastas
----------------------------

Dados gerados por projetos

   data/refined/[projeto ou squad]/[assunto]/[subassunto]/[nome da tabela ou visão]/[yyyy]/[mm]/[dd]/[hh]/[mi]/[ss]/rf_[nome da tabela ou visão]_[yyyymmdd]_[hhmiss].parquet 
 

   * Exemplo 1: dado gerado de forma full, diariamente - reprocessamento. refined/book_varejo/expansao/volume_comercial/rf_volume_comercial.parqu et  
   * Exemplo 2: dado incremental, particionado pela data da informação refined/book_varejo/expansao/pr_exprede_etapa_negcc_prov_dia/2022/05/0 3/rf_pr_exprede_etapa_negcc_prov_dia_20220503.parquet  
   * Exemplo 3: carga full diária, mantendo histórico de versões anteriores. O dado é particionado pela data de inclusão **no storage**, seguindo o mesmo padrão do exemplo anterior, de carga incremental.  
   * Exemplo 4: réplica de relatório do Microstrategy , “Venda diária por Tipo de Atendimento Automotivo (a carga real está em uma estrutura diferente, aqui foi adaptado apenas para exemplo) refined/jetoil/venda_diaria/tipo_atendimento_automotivo/2022/05/15/rf_tipo _atendimento_automotivo_20220515.parquet 
  

