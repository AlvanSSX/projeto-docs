4. Bases e ferramentas da Ipiranga
+++++++++++++++++++++++++++++++++++

4.1. Bancos de dados internos
==============================

* **Transacionais**: JDE (JD Edwards) e ORASN (Oracle + a sigla para Sistema de Negócio). 
* **BI**: em produção, os bancos ORABI, ORABI1, ORABIF. Em desenvolvimento, ORABID, ORABI1D, ORABIFD. Em homologação, ORABIH, ORABI1H, ORABIFH. 

Quais são as diferenças entre as bases ORABI, ORABI1 e ORABIF no BI? 

* ORABI contém das tabelas fatos (tabela com prefixo PR) e dimensões (tabela com prefixo DM), por exemplo, PR_ITEM_CUPOM e DM_COMPONENTE; 
* ORABI1 contém tabelas agregadas (tabela com prefixo AG), por exemplo, AG_SITUACAO_COMPONENTE_MES; 
* ORABIF contém tabelas de franquias ou programas de marketing, como dados da AmPm, Jet Oil e KmV. Nesta base podem conter tabelas fatos, ou dimensões. 
  
Quando uma equipe de projeto recebe uma demanda de ingestão de dados, se faz necessário primeiramente mapear se já não existe algum processo que faça ingestão das tabelas para o nosso Data Lake. Caso de fato não exista, deve ser aberta uma solicitação de acesso a tabela para o usuário DLETL, conforme especificado na seção 3.3. 

Caso a demanda envolva uma View, deve-se fazer ingestão das tabelas envolvidas e reproduzir a view no ambiente Azure (via Databricks, por exemplo), e não trazer diretamente a view do banco, de forma a não onerar o ambiente Oracle com essas consultas. 

Os modelos de dados das bases de BI e Sistema de Negócio, e de algumas tabelas do JDE, podem ser encontrados no 
`Portal Gestão de Dados <https://grupoultracloud.sharepoint.com/sites/ipp-portalgestaodados>`_, que dispõe de informações sobre as tabelas, como sua descrição com objetivo e dicionarização das colunas. Para mais informações sobre os conceitos do corporativo, como componente, ponto de venda, entre outros, consultar o material 
`Conceitos do Corporativo <https://grupoultracloud.sharepoint.com/:p:/r/sites/ipp-portalgestaodados/Documentos Compartilhados/Administra%C3%A7%C3%A3o de Dados/Documentos de Normas e Padr%C3%B5es/03. Apresenta%C3%A7%C3%B5es/Ipiranga_final.ppt?d=w89a6a27a88464c79aaa0b9850d18544a&csf=1&web=1&e=br4iMk>`_. 

4.2 Power Center
===================

Plataforma de integração de dados amplamente usada na Ipiranga, para fazer extração dos dados de diversos sistemas, realizar limpeza e transformação conforme as regras e necessidades do negócio, para que então sejam carregados no Data Warehouse (as bases de BI), concluindo a etapa de ETL realizada pela ferramenta. Na Ipiranga, o Power Center armazena mapas e workflows de carga das tabelas, responsáveis pelas ações mencionadas acima. 

4.3 Microstrategy - Business Intelligence
============================================

O Microstrategy foi o primeiro BI da Ipiranga e nele encontram-se vários relatórios e indicadores em que são visualizadas, por exemplo, informações de vendas por franquias, por componentes, região, margem, volume de venda etc. Para criar esses relatórios, as fontes de dados são importadas para o Microstrategy e mapeadas, para permitir as consultas, e a partir dessas bases podem ser criadas colunas calculadas, com base em agregações e junções entre diferentes tabelas.  

Apesar de ser uma ferramenta “antiga”, ela segue sendo utilizada pelo time de negócio para análise relatórios, indicadores e tomada de decisão. Por esta razão, é comum surgirem demandas em projetos nas quais sejam solicitadas a replicação de algum desses relatórios em nosso Data Lake, devido a alguma necessidade de negócio.  

Portanto, é necessário identificar com o solicitante da demanda o “path” do relatório no Microstrategy para que possamos através desse caminho, solicitar ao time de Data Viz a query utilizada pela ferramenta para construir o relatório em questão. Através dessa informação, é possível identificar as tabelas envolvidas, assim como os relacionamentos e tratamentos que são feitos para replicar o relatório em questão. 

4.4 Sistema de Arquivo ABADI
==============================

O ABADI é um sistema de arquivo no qual os dados são armazenados no Cache (MUMPS). Para as tabelas do ABADI, o ideal é realizar um estudo de viabilidade junto a liderança técnica do projeto e identificar a real necessidade de ser feita esta ingestão, uma vez que se trata de um banco transacional.  
