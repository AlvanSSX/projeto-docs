2. Padrões 
+++++++++++++

2.1. Nomenclatura
===================

Os nomes de logic apps devem seguir o padrão abaixo: 

**logic-ipp-[projeto]-[prefixo da trigger]-[descrição]-[ambiente]**

* Área é aquela à qual o projeto pertence;
* Para identificar o tipo de trigger, deve-se usar os prefixos especificados na Tabela 1 abaixo;
* A descrição deve ser algo que identifique o aplicativo, como o assunto, e não deve ser genérica;
* Durante o desenvolvimento, deve-se usar o sufixo para o ambiente, “-dev”, para melhor identificação. Quando o logic app for produtizado, o sufixo será retirado do nome.
  
    .. image:: /imagesla/la.png

**Tabela 1:** Prefixos para definição de nomes de triggers 

Exemplos (fictícios, para ilustração):

* logic-ipp-custos-http-notificacao
* logic-ipp-painel-vendas-schedule-orcamento-me
* logic-ipp-book-varejo-email-unitarios-correcao
* logic-ipp-custos-sftp-estoque
* logic-ipp-gestao-riscos-sharepoint-import-arc
* logic-ipp-market-share-event-divulgacao-das 

2.2. Resource Group
====================

Novos desenvolvimentos devem ser criados na subscrição de laboratório, **Ipiranga – DataLake – DEV**, dentro do Resource Group da área a qual o projeto pertence, cujo nome segue o padrão **rg-ipp-[área]-dev**

2.3. Conexões
==============

2.3.1. Conexão com Office 365
-------------------------------

Steps que precisem de conexão com o Outlook, devem usar o email Ipiranga do engenheiro. Quando o processo for passado para produção, este email será trocado para o do Data Lake, `svc.ipippdatalake-p@ultra.com.br <mailto:svc.ipippdatalake-p@ultra.com.br>`_

2.3.2. Conexão com storage account
-------------------------------------

Steps que conectam em uma storage account devem usar o storage de laboratório, **stippdatalakelab**, utilizando a conexão existente de managed identity. Ao ser produtizado, a conexão será reapontada para o storage produtivo, **stippdatalakedev**. 

Bom, anteriormente a recomendação era utilizar o “Acess Key” para conexão com os nossos Storages, porém atualmente sugerimos uma nova metodologia de conexão usando o “Managed Identity”. Para selecionar essa conexão você precisa abrir o logic apps envolvido e buscar por “identity”. Nesta opção, você deve seguir os passos abaixo, como no exemplo: 

    .. image:: /imagesla/Imagem1.jpg

**Figura 1**: Exemplo de conexão com Managed Identity

Segue abaixo quais “Managed Identity” devemos utilizar:

**Conexões de produção, datalake de dev (stippdatalakedev) na subscription e rg PRD:**

logic_apps_managed_identity_east_us_prd

logic_apps_managed_identity_east_us_2_prd  

Identidade Gerenciada: 

user-logic-apps-prd  

**Conexões de desenvolvimento, datalake de lab (stippdatalakelab) na subscription e rg DEV:**

logic_apps_managed_identity_east_us_dev

logic_apps_managed_identity_east_us_2_dev

logic_apps_managed_identity_northcentralus_dev

logic_apps_managed_identity_centralus_dev  

Identidade Gerenciada: 

user-logic-apps-dev  

**Conexões de laboratório, datalake de lab (stippdatalakelab) na subscription e rg LAB:**

logic_apps_managed_identity_east_us_lab 

logic_apps_managed_identity_east_us_2_lab 

logic_apps_managed_identity_northcentralus_lab 

logic_apps_managed_identity_centralus_lab 

Identidade Gerenciada: 

user-logic-apps-lab

**Conexões Datalake LGPD:**

**No momento, temos as seguintes conexões para o datalake de prd de lgpd**

logic_apps_managed_identity_east_us_2_prd  

Identidade Gerenciada: 

user-logic-apps-lgpd-prd

**No momento, temos as seguintes conexões para o datalake de lab de lgpd:**

logic_apps_managed_identity_east_us_2_dev 

logic_apps_managed_identity_east_us_2_lab  

Identidade Gerenciada: 

user-logic-apps-lgpd-dev 

user-logic-apps-lgpd-lab 


Segue exemplo de como fica a conexão ao utilizar o “Managed Identity”:


    .. image:: /imagesla/Imagem2.jpg

**Figura 2:** Exemplo de conexão com Managed Identity

**Importante:** visto estas orientações, **NÃO DEVEMOS CRIAR NOVAS CONEXÕES**. Os engenheiros devem utilizar somente as novas conexões criadas. 

É necessário o nome do storage correto


2.4. Outras orientações

Uma ação que é possível de se realizar via Logic App, mas que **não utilizamos** na Ipiranga, é fazer chamada de pipelines do Data Factory. **Toda orquestração de processos deve ser concentrada no ADF**. No caso de Logic Apps, caso um processo precise ser executado após a disponibilização de um arquivo no storage, deve-se criar uma event trigger para capturar esse evento e acionar o pipeline. 

Atenção às versões de triggers e actions disponíveis no Logic app, pois algumas estão descontinuadas (deprecated). 

Todo processo que grava arquivos no storage deve seguir a `Definição de Camadas do Data Lake <https://grupoultracloud.sharepoint.com/:b:/r/sites/ipp-portalgestaodados/Documentos%20Compartilhados/Analytics/Engenharia%20de%20dados/Data%20Lake%20Storage/Defini%C3%A7%C3%A3o%20de%20Camadas%20Data%20Lake.pdf?csf=1&web=1&e=uKYiCh>`_ para adequar os diretórios e nomes de arquivos aos padrões especificados. 

Processos que fazem captura de email são desenvolvidos utilizando o email Ipiranga do engenheiro de dados, mas são produtizados com o email do Data Lake, svc.ipippdatalake-p@ultra.com.br. Portanto, o projeto deve se atentar a isso, e alinhar com o usuário responsável pelo envio que encaminhe tal email corretamente. 


