5. Tabelas on-premises
++++++++++++++++++++++++

Em alguns casos é necessário realizar tratamentos em tabelas após a ingestão para que seus dados fiquem confiáveis como por exemplo sem duplicidades em seus registros, podendo o dado ficar armazenado na camada `Trusted <https://grupoultracloud.sharepoint.com/:b:/r/sites/ipp-portalgestaodados/Documentos%20Compartilhados/Analytics/Engenharia%20de%20dados/Data%20Lake%20Storage/Defini%C3%A7%C3%A3o%20de%20Camadas%20Data%20Lake.pdf?csf=1&web=1&e=OtRPiZ>`_, é comum o uso do Databricks para realizar esta operação, porém com o intuito de concentrar tabelas desta natureza em um único local e termos uma melhor organização assim como facilidade em suas manutenções, será demonstrado nos próximos passos onde e como armazenar estes desenvolvimentos. 

Obs.: Estes processos de ingestões são referentes as fontes: ORACLE, ABADI e JDE

5.1 Ambiente de desenvolvimento
==================================

Os notebooks devem ser desenvolvidos dentro de um dos Workspace/repositórios abaixo dependendo da natureza do dado 

* **dtb-ipp-dev** (para dados que não possuem características sensíveis) o prj-pco >> prj-pco-adb
* **dtb-ipp-sensiveis-dev** (para dados que possuem características sensíveis) o prj-pco >> prj-pco-sensiveis-adb   

5.2 Hierarquia de pastas
============================

Será mantido uma hierarquia similar ao que hoje existe no Data Factory 

    .. image:: /imagesdb/Imagem22.jpg

**Figura 22**: Exemplo de hierarquia 

5.3 Particionamento dos dados
================================

Convencionado o uso do Databricks para realizar o particionamento  dos dados (anteriormente feito por DataFlow no `Data Factory <https://grupoultracloud.sharepoint.com/:f:/r/sites/ipp-portalgestaodados/Documentos%20Compartilhados/Analytics/Engenharia%20de%20dados/Data%20Factory?csf=1&web=1&e=QlCgoR>`_ tópico: Ingestão de tabela fato e **agregada – Oracle** ) no Data Lake. Os próximos passos demonstram como seguir quando houver demanda de ingestão de tabela fato e/ou agregada de forma incremental.  

5.3.1 Construção do Notebook
------------------------------

A hierarquia segue como exemplificado no passo 5.2 deste documento, e a construção segue no mesmo formato como descrito em passos anteriores (Referencia: 3.2 a 3.3). 

O pipeline no `Data Factory <https://grupoultracloud.sharepoint.com/:f:/r/sites/ipp-portalgestaodados/Documentos%20Compartilhados/Analytics/Engenharia%20de%20dados/Data%20Factory?csf=1&web=1&e=Z902ul>`_ segue no mesmo formato, mudando apenas o recurso responsável por obter os dados da camada transient e particionar seguindo a hierarquia seja nível ANO/MÊS/DIA ou ANO/MÊS como já é seguido atualmente.   

    .. image:: /imagesdb/Imagem23.jpg

**Figura 23**: Exemplo de pipeline de ingestão de tabelas fato   

A próxima imagem contém um exemplo de notebook, por ser um exemplo de um processo refatorado em ambiente produtivo, algumas variáveis e comandos inclusos foram mantidos por questões de legado não havendo necessidade da permanência para os processos futuros, sendo:

* Passo **3 – Variáveis:** (rf_pr_pesquisa_preco_bomba)
* Passo **6 – Gravando Resultado Final:** (Movimentação 2 - Camada REFINED)  

O controle de carga (Passo 7 da imagem) segue na mesma dinâmica, obtendo o valor máximo da data de referência do arquivo gerado na transient, após a captura, o valor é salvo na pasta _conf/controle_data do Data Lake assim como é feito atualmente.


    .. image:: /imagesdb/Imagem24.jpg

Figura 24: Exemplo de um Notebook 

Exemplo das funções utilizadas no notebook de exemplo:

    .. image:: /imagesdb/db2.png


    .. image:: /imagesdb/db3.png

