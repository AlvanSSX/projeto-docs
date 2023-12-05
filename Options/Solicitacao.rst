3. Solicitação de acessos
++++++++++++++++++++++++++

3.1 Ambiente Azure
===================
Para solicitar acesso a recursos do nosso ambiente Azure, deve ser preenchido o `Formulário de Concessão de Acessos <https://tinyurl.com/form-acesso>`_. Ali, devem ser preenchidas as seguintes informações:

* Para quem está sendo solicitado o acesso; 
* Gestor que aprovará a solicitação; em geral, é o líder do projeto do lado Ipiranga; 
* Área a qual o desenvolvedor pertence. Sendo de consultoria, indicar qual; 
* Projeto em que o desenvolvedor está alocado; 
* Perfil do desenvolvedor, para a concessão dos acessos; 
* Recursos para os quais se deseja acesso. Por exemplo: sendo um arquivo, indicar o nome do storage e o diretório; sendo um ambiente do Databricks, indicar o nome do workspace; e o análogo serve para demais recursos; 
* Motivo do acesso. Toda solicitação deve ser justificada no formulário, e posteriormente aprovada pelo gestor indicado, que receberá um email notificando sobre esse pedido. 

Enviada a solicitação e aprovada pelo gestor por email, esta entrará na fila do time de Infra Cloud, sendo atendida dentro do SLA de 48 horas úteis.

Quanto à criação de recursos, esta é feita mediante desenho de arquitetura da solução apresentado e aprovado pelo time de Arquitetura. Tendo essa aprovação, o time de Infra criará os recursos necessários e os grupos de acesso para o projeto (para isso é necessário que o líder do projeto informe a relação de desenvolvedores do projeto e seus perfis). O SLA para criação de recursos é de 72 horas úteis.

Em caso de dúvidas ou outros tipos de solicitações, entrar em contato com o time de arquitetura através do email arquiteturadedados@ultra.com.br


3.2 Azure SQL 
===================

Solicitações referentes ao ambiente Azure SQL devem ser feitas via ALM ou Jira, e são direcionadas para o time de Administração de Dados. Para o ALM, é necessário ter acesso à rede Ipiranga, portanto, apenas colaboradores Ipiranga ou terceiros com acesso à VPN conseguirão abrir tarefas. 

Para obter acesso a tabelas, deve-se incluir na tarefa: 

* Usuário de rede do desenvolvedor (login Ipiranga) 
* Nome do banco e ambiente (desenvolvimento ou produção)
* Tipo de acesso, se leitura ou escrita (sendo no ambiente produtivo concedido apenas leitura) 
* Tabela e seu owner 
* Motivo do acesso 
* Aprovação do líder ou owner do projeto.

Para criação de objetos, como tabelas ou views, é necessário incluir na tarefa:

* Projeto para o qual está sendo criada a tabela 
* Motivo/objetivo da criação 
* Nome do banco onde será feita a criação 
* Planilha de dicionarização, preenchida com informações sobre a estrutura da tabela. O modelo dessa planilha e detalhes sobre preenchimento podem ser encontrados no `Portal Gestão de Dados <https://grupoultracloud.sharepoint.com/sites/ipp-portalgestaodados/SitePages/Dicionariza%C3%A7%C3%A3o-de-Estruturas-de-Dados.aspx>`_. 
* Observação: tabelas no Azure SQL devem ser normalizadas. O time de Arquitetura aprova a arquitetura geral do projeto, mas a modelagem das tabelas a serem criadas deve passar por avaliação e aprovação do time de Administração de Dados

O SLA de atendimento para concessão de acessos e criação de objetos é de 5 dias. 
  
3.3 Oracle
===========

O processo para solicitação de acesso de usuários pessoais (para analistas e desenvolvedores) à tabelas no ambiente Oracle é descrito no seguinte documento desenvolvido pelo time de Administração de Dados, `Processo de solicitação de acesso <https://grupoultracloud.sharepoint.com/:b:/r/sites/ipp-portalgestaodados/Documentos Compartilhados/Administra%C3%A7%C3%A3o de Dados/Processo de Solicita%C3%A7%C3%A3o de Acesso em Produ%C3%A7%C3%A3o/AD - Processo solicita%C3%A7%C3%A3o de acesso - BI.pdf?csf=1&web=1&e=wUlLrn>`_, disponível no Portal Gestão de Dados.

Para desenvolvimento de processos de ingestão do Oracle para o Data Lake, é necessário solicitar acesso às tabelas em questão para o usuário DLETL, o usuário de banco do Data Lake. Antes de qualquer coisa, orientamos que seja verificado se já não existe algum processo para esse mesmo fim, o que dispensaria um novo desenvolvimento. Para abrir essa solicitação de acesso, deve-se criar uma tarefa via ALM ou Jira, incluindo as informações:

* Usuário DLETL; 
* Projeto que está fazendo a solicitação; 
* Nome das tabelas e seus owners (se possuir essa última informação); 
* Nome do banco e ambiente (desenvolvimento, homologação ou produção); 
* Tipo de acesso, se leitura e/ou escrita; 
* Motivo do acesso; 

Os acessos serão concedidos pelo time de AD nos ambientes de desenvolvimento e homologação, e para o ambiente produtivo, é necessário abrir uma implantação de banco de dados, também via ALM ou Jira. A partir dessa implantação, a AD responsável abrirá uma Mudança, e esta passará pelo comitê de mudanças para aprovação, que ocorre às terças e quintas. Após esse processo, os DBAs do Ultra executarão a tarefa no ambiente produtivo. 

O SLA do time de AD é de 5 dias. 


3.4 JDE
========

A solicitação de acesso para o ambiente do JDE segue o mesmo padrão descrito anteriormente para Oracle. 

3.5 ABADI
===========

Para solicitação de acesso a tabelas do ABADI, deve ser aberto uma RDM via `Portal de Serviços <https://portaldeservicos.ultra.com.br/ipiranga>`_, solicitando acesso ao usuário **ABADIDL** para a base necessária. Para que seja concedido, é preciso aprovação dupla dos times de Gestão De Demandas Ti - Op e Coord. Serviços A Sistemas. 

Existe uma instância chamada **Shadow**, que é um espelho do ABADI, tendo sido projetada para evitar onerar o banco produtivo com querys de projetos e múltiplas requisições que possam ser necessárias. Novos processos devem preferencialmente consumir dados a partir dessa instância. 

