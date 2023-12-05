3. Matriz de Obrigatoriedade
+++++++++++++++++++++++++++++

A obrigatoriedade das camadas foi definida por tipo de dados, conforme representado na tabela a seguir.

* Todo arquivo final de processos de ingestão devem ser disponibilizados na camada trusted, de acordo com as regras e tratamentos descritos na definição dessa camada;
* Dados não obrigatoriamente precisam passar pelas camadas raw e transient.
* Dados com duplicidade na camada raw devem ser obrigatoriamente tratados e passados para a camada trusted.
   * Exemplo: cargas full, como de dimensões ou tabelas fato, podem ser salvos diretamente na trusted. Cargas que trazem dados com duplicidade, salvam na camada raw e após remoção de duplicidade, salvam na trusted.
* Dados que são lidos da origem usando querys específicas (com filtros, por exemplo), devem ser salvos diretamente na camada trusted. Porém, isso será permitido apenas em caráter de exceção, sendo necessário justificar a necessidade e ter aprovação do time de Arquitetura.




   .. image:: /imagesdl/dl1.png

1 Dado na trusted apenas após tratamentos para garantir integridade 
2 Casos de exceção, conforme explicitado acima 

**Tabela 1**: Matriz de obrigatoriedade dos dados através das camadas
