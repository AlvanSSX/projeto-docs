5. Ambientes de projetos
+++++++++++++++++++++++++

No início de todo projeto, deve ser elaborado um desenho com a arquitetura proposta para a solução, e este deve ser submetido à aprovação do time de Arquitetura. Nesse desenho deve ser especificado o fluxo do dado, da origem ao destino, indicando a ferramenta/recurso usado em cada etapa. Também deve conter os tipos de origens, como arquivos manuais, bases Oracle, sites externos, APIs etc. (no caso de bases internas da Ipiranga, deve-se listar os nomes das tabelas que serão usadas). Cada etapa deve ser numerada e o desenho deve conter uma box com legenda, onde serão descritas as etapas. 

Aprovada a arquitetura, a equipe de Infra preparará o ambiente do projeto, criando os recursos e concedendo os acessos necessários. Algumas informações sobre os ambientes de cada recurso: 

* DevOps: é criado um projeto para a área a qual pertence o projeto, e dentro dele, haverá repositórios para cada tipo de recurso que faz versionamento de códigos; 
* Databricks: é criado um workspace de LAB para a área a qual pertence o projeto, onde serão desenvolvidos e testados os processos. O ambiente produtivo é único para todos os projetos; 
* Function App: é criado um function app de LAB e DEV para a área a qual pertence o projeto. As functions são desenvolvidas localmente, mas podem e devem ser testadas no ambiente de LAB para garantir que executarão no ambiente Azure antes de serem passadas para DEV (ambiente produtivo); 
* Logic App: devem ser criados pelos engenheiros dentro do Resource Group de LAB do projeto, e após teste e validação do time de Arquitetura, é passado para o Resource Group de DEV; 
* Data Factory: recurso único para todos os projetos, o desenvolvimento é feito em branchs individuais para posterior merge com a branch master. 
