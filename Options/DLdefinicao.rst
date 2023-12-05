2. Definição das camadas
++++++++++++++++++++++++++

2.1. Camada Transient
======================

* Camada usada para ingestão e armazenamento de dados gerais, como: cópias temporárias de tabelas e arquivos, e outros dados de curta duração, antes de serem efetivamente ingeridos;
* Não é armazenado o histórico dos dados;
* É recomendável que nesta camada exista uma rotina de limpeza (a implementar);
* Camada utilizada para início de desenvolvimentos, enquanto não há segregação dos ambientes de desenvolvimento e produção
* Exemplos:  
   * o dados temporários de processos; 
   * o dados não produtivos (testes iniciais de desenvolvimentos). 


2.2. Camada Raw
=================

* Camada de ingestão, que armazena os dados brutos, assim como vieram da origem, sem conter filtros ou tratamentos, exceto o filtro necessário para o intervalo de tempo desejado dos dados;
* Nessa camada, os arquivos podem ser armazenados no formato (extensão) de origem;
* As ingestões devem ser individuais, para cada tabela, **sem** uso de querys que contenham joins, filtros, agregações etc.;
* Esta camada é onde devem ser salvos os dados que são inputs de usuários, como planilhas Excel;
* Camada onde são armazenados spools de streaming;
* É armazenado o histórico de ingestão dos dados, como arquivos incrementais trazidos antes de serem consolidados apenas com a versão atual de cada registro na camada trusted;
* Pode haver inconsistências de data types de colunas em relação à origem do dado; ▪ Podem existir registros duplicados ou registros excluídos na origem;
* Esta camada é onde os dados confidenciais devem ser armazenados, já criptografados e registrados no catálogo de dados (a implementar);
* Exemplos:
    * o Histórico de ingestões incrementais; 
    * o Arquivos Excel enviados por e-mail por áreas de negócio e consumidos via Logic App; 
    * Dados de fontes externas, armazenados no formato e estrutura que vem de sua origem.


2.3. Camada Trusted
=====================

* Camada onde são armazenados os dados já tratados/limpos e validados; 
* Transforma os dados brutos vindos da camada bruta em dados confiáveis para consumo;
* Tipos de tratamento: validação de campos, tratamentos de campos nulos, qualidade de dados, ajuste de data types, remoção de duplicidade etc.;
* Nessa camada, os arquivos já devem estar em formato parquet;
* Dados devem ser mantidos em baixa granularidade, permitindo que estes sejam agregados em níveis superiores da hierarquia;
* Camada de consumo para as aplicações/processos;
* Ferramentas de visualização podem consumir dessa camada em casos em que o dado necessário passa apenas por tratamento, sem nenhum nível de refinamento.
   * Casos previstos: dados de fontes externas ou enviados por usuários;
   * Casos não previstos: dados cuja fonte sejam sistemas ou bancos de dados internos à Ipiranga. Para esses casos, deve-se seguir a arquitetura padrão, a partir do Analysis Services.
* Exemplos:  
   * dados externos tratados;
   * dados do negócio validados;
   * dados da camada raw após remoção de duplicidade, podendo haver ainda registros excluídos na origem, uma vez que não é possível verificar o log para identificar tais registros. 

2.4. Camada Refined
====================

* Camada onde são armazenados os dados enriquecidos, consolidados, vindos da camada trusted;
* Novos refinamentos de dados contidos nesta camada, também deverão ser salvos na mesma;
* Camada de consumo de ferramentas de visualização, como Power BI;
* Exemplos:
   * Réplicas de relatórios do Microstrategy;
   * Visões geradas para squads de ciência de dados;
   * Outputs gerados por projetos.

2.5. Camada Serving
====================

* Camada para disponibilizar os arquivos que serão utilizados pelo usuário de negócio;
* Apenas arquivos csv ou xlsx, caso o output seja parquet, deve ficar na camada refined;
* Camada de consumo de ferramentas de visualização, como Power BI. 


