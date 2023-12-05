2. Ambientes
++++++++++++++

O ambiente de desenvolvimento agora encontra-se unificado, contendo apenas um Workspace Databricks denominado `dtb-ipp-dev <https://adb-5221172512454999.19.azuredatabricks.net/?o=5221172512454999>`_. 

Neste ambiente, os desenvolvimentos serão concentrados dentro de “Repos”, onde cada desenvolvedor poderá vincular o repositório da área a qual tem acesso dentro do seu usuário. **(Figura 11)** 

Sendo assim, o desenvolvimento será feito no ambiente de DEV dentro da área correspondente e, após deploy, as alterações realizadas serão passadas para o workspace produtivo (a saber, `dtb-ipp-prd <https://adb-7950802705954835.15.azuredatabricks.net/?o=7950802705954835>`_ via pipeline de CI/CD.) 

No workspace de DEV, é possível fazer leitura e escrita no storage de laboratório, stippdatalakelab, utilizando o ponto de montagem **/mnt/[área]/dev/**, e apenas leitura no storage produtivo, stippdatalakedev, utilizando o ponto de montagem **/mnt/[área]/prd/**. 

No workspace de **produção, só é possível fazer leitura e escrita no storage produtivo**, portanto, todo arquivo necessário para um processo deve estar disponível ali.


2.1 Dados sensíveis
====================

Possuímos ainda um workspace Databricks, **dtb-ipp-sensiveis-prd**, utilizado para trabalhar com dados sensíveis, como dados cadastrais de nossos clientes, sejam dados bancários, CPF, endereço, telefone, email etc. Caso um projeto precise trabalhar com dados sensíveis, contidos no storage **stippdatalakelgpdhml**, através do Databricks, deverá solicitar acesso ao ambiente de dados sensíveis, pois é o único que possui acesso de leitura e escrita para esse storage. O workspace **dtb-ipp-sensiveis-dev** é onde serão desenvolvidos e testados os processos que posteriormente passarão para o workspace produtivo, e possui acesso de leitura para o storage stippdatalakelgpdhml, e de leitura e escrita para o storage **stippdatalakelgpdlab**
