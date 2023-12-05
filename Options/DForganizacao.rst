4. Organização
+++++++++++++++

4.1. Datasets
==============

É obrigatório que os datasets utilizados sejam organizados em pastas por tipos de datasets.

São considerados tipos de dataset:

* Arquivos: por tipo de arquivo, dentro da pasta “files” (parquet, json, csv, avro, binary, orc etc.)


    .. image:: /imagesdf/Imagem1.jpg
     :align: center

    **Figura 1:** Exemplo de organização de datasets do tipo arquivo
      

* Bancos: separados por instância. Para bancos externos, deve-se seguir a mesmaregra.
  
    .. image:: /imagesdf/Imagem2.jpg
     :align: center

    **Figura 2:** Exemplo de organização de datasets do tipo Oracle

* Azure SQL

    .. image:: /imagesdf/Imagem3.jpg
     :align: center

    **Figura 3:** Exemplo de organização de datasets do tipo Oracle

* Cache

    .. image:: /imagesdf/Imagem4.jpg
     :align: center

    **Figura 4:** Exemplo de organização de datasets do tipo Cache 

4.2. Pipelines e Dataflows
===========================

É obrigatório que pipelines e dataflows sejam organizados em pastas por assunto. 

**Regras:**

1. Nomes de pastas e pipelines sempre em letras minúsculas e sem acentuação.
2. Estrutura a ser utilizada para organização dos pipelines:

   2.1. **Fontes internas:** pipelines/dataflows desenvolvidos para ingestão de dados internos à Ipiranga ou refinamentos específicos de projetos ou squads.
   
    2.1.1.	**Oracle (e demais bancos):** ingestões de/para bancos de dados. A hierarquia de pastas deve ser seguida conforme instância do banco e owner da tabela. Pipelines que replicam views do Oracle também deverão seguir esse padrão, mesmo que não seja feita a cópia direta da view a partir do banco. 

      ::

            fontes_internas 
                   oracle 
                          [instância do banco]
                                 [owner da tabela] 
                                       pip_[nome da tabela a] 
                                       pip_[nome da tabela b]  
                    azure_sql 
                           [nome do banco] 
                                 pip_azsql_[nome da tabela]

    2.1.2.	**Projetos:** a pasta projetos compreende processos desenvolvidos por/para squads de Ciência de Dados e projetos. Deve ser organizado conforme o projeto/squad solicitante e o assunto.

      ::

            fontes_internas 
                    projetos 
                            [nome do projeto ou squad]  
                                  [assunto]  
                                         pip_[descrição]

    2.1.3.	**Views:** pipelines ou dataflows desenvolvidos para gerar visões a partir de dados existentes, mas não especificamente para uma squad ou projeto, como processos mais genéricos de uso geral(avaliar com o time de Governança Técnica).
    
      ::

            fontes_internas 
                    views 
                          pip_[descrição] 

    2.1.4.	**SFTP:** pipelines ou dataflows desenvolvidos para ingestão via SFTP 

      ::

            fontes_internas 
                    sftp 
                            [assunto]  
                                    pip_[descrição]  


    .. image:: /imagesdf/Imagem5.jpg
     :align: center
        

    **Figura 5:** Exemplo de organização de processos de “fontes_internas”

 
   2.2.	**Fontes externas:** pipelines e dataflows desenvolvidos para ingestão ou tratamento de dados externos à Ipiranga. Deve ser organizado conforme a área solicitante e fonte da informação.

    ::

        fontes_externas
                [nome do projeto ou squad]
                        [fonte de dado]
                                [assunto]
                                        pip_[descrição] 

   .. image:: /imagesdf/Imagem6.jpg
     :align: center

   **Figura 6:** Exemplo de organização de processos de “fontes_externas” 

