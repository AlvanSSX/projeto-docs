3. Desenvolvimento
+++++++++++++++++++++

**IMPORTANTE!**

Os logic Apps em DEV e LAB serão desligados diariamente para que não fique rodando, somente os logic apps de PRD ficaram ligados funcionando com as triggers. 

3.1. Criando um Logic App
===========================

Para criar um novo aplicativo, no Portal Azure, busque por “Logic apps” na barra de pesquisa e abra. 

    .. image:: /imagesla/Imagem3.jpg 

**Figura 3:** Abrindo Logic apps

Aparecerão os recursos para os quais você possui acesso para visualizar. Clique na opção **Add** para criar um novo Logic App. 

    .. image:: /imagesla/Imagem4.jpg

**Figura 4:** Adicionando novo Logic app 

Na tela que se abrirá em seguida, definiremos algumas configurações básicas do novo Logic app:

* “Subscription”: deverá ser selecionada a subscrição de laboratório, **Ipiranga – DataLake – DEV;**
* “Resource group”: selecionar o da área a qual o projeto pertence, cujo nome segue o padrão **rg-ipp-[área]-dev** conforme a tabela 2 abaixo;
* “Logic App name”: o nome deverá seguir o padrão especificado na seção 2.1;
* “Publish”: **Workflow** como padrão;
* “Region”: **East US 2**, que é a mesma localização em que se encontram a maioria dos nossos recursos;
* “Plan type”: **Consumption** é o padrão usado, em que pagamos conforme execução do aplicativo;
* “Zone redundancy”: manter desabilitado.

    .. image:: /imagesla/la1.png

**Tabela 2:** tabela de resource groups a serem utilizados para desenvolvimento

    .. image:: /imagesla/Imagem5.jpg

**Figura 5:** Criando novo Logic app, aba Basics, parte 1 

    .. image:: /imagesla/Imagem6.jpg

**Figura 6:** Criando novo Logic app, aba Basics, parte 2 

Feito isso, clique no botão **Review + create**, para revisar as configurações e finalizar a criação do Logic App. Confira os detalhes informados nessa tela, e, estando de acordo, clique em **Create**

    .. image:: /imagesla/Imagem7.jpg

**Figura 7:** Criando novo Logic app, aba Review + create 

Aparecerá uma notificação indicando que o deploy do aplicativo foi iniciado, e depois seremos redirecionados para a página indicada na Figura 8. Clicando no botão **Go to Resource**, será aberta a página do Logic **Apps Designer** (Figura 9).  

    .. image:: /imagesla/Imagem8.jpg

**Figura 8:** Deploy de Logic App finalizado com sucesso

    .. image:: /imagesla/Imagem9.jpg

**Figura 9:** Logic Apps Designer 

Nessa página, serão exibidas algumas opções para iniciar o desenvolvimento, como triggers para ativação do Logic app e ainda templates para usar como base. 

    .. image:: /imagesla/Imagem10.jpg

**Figura 10:** templates disponíveis para criação de Logic app 

Selecionaremos aqui a opção **“Blank Logic App”** para seguir com exemplos, e a primeira ação será escolher o tipo de trigger que dará início à execução

    .. image:: /imagesla/Imagem11.jpg

**Figura 11:** Logic app em branco 

3.2. Triggers 
================

Na aba **All**, podemos ver os tipos de triggers disponíveis, separadas por grupos. 

    .. image:: /imagesla/Imagem12.jpg

**Figura 12:** Grupos de triggers para Logic app 

As triggers mais comumente usadas são dos grupos Request, Schedule, Outlook, SharePoint e Azure Event Grid, e as descreveremos a seguir

3.2.1. Request 
---------------

Do grupo Request, **“When a HTTP request is received”** é usada para chamar um logic app a partir de outros processos, como um pipeline do Data Factory, por exemplo. 

    .. image:: /imagesla/Imagem13.jpg


**Figura 13:** Seleção de trigger do tipo Request 


    .. image:: /imagesla/Imagem14.jpg

No box **“Request Body JSON Schema”** (Figura 14) é possível especificar o corpo da requisição, definindo o conteúdo (dados) que deve ser passado para o Logic App. Caso não possua um schema pronto, mas tenha um conteúdo de exemplo no formato JSON, podemos gerar o schema com base nesse conteúdo. Clique em **“Use sample payload to generate schema”**, insira o conteúdo (Figura 15)  e conclua. O resultado pode ser visto na Figura 16. Na requisição, não esquecer de incluir um header “Content-Type” com o valor **“application/json”**

    .. image:: /imagesla/Imagem15.jpg

**Figura 15:** Usando estrutura JSON exemplo para gerar schema

    .. image:: /imagesla/Imagem16.jpg

Por padrão o método da requisição é POST, mas é possível definir um método diferente, clicando em **“Add new parameter”** (Figura 14) e selecionando **“Method”**. Esse parâmetro listará outros métodos possíveis. 

    .. image:: /imagesla/Imagem17.jpg

3.2.2. Schedule
-----------------

No grupo Schedule, temos as triggers **“Recurrence”** e **“Sliding Window”**. Ambas permitirão o agendamento do Logic App para execução com uma determinada frequência. A grande diferença entre essas triggers é que em caso de problemas que impactem alguma das execuções agendadas, a Sliding Window voltará nas execuções perdidas e processá-las, enquanto a Recurrence não, apenas voltará a executar a partir do próximo horário. 

    .. image:: /imagesla/Imagem18.jpg

**Figura 18:** Triggers do tipo Schedule

Dessas, a de uso mais comum em nossos processos é a Recurrence. Caso o desenvolvedor verifique a necessidade de uso da Sliding Window, deve ser alinhado com o time de Governança Técnica. 

A configuração da trigger deve ser definida com base num intervalo e frequência de execução, que podem ser minutos, horas, dias, meses etc. 


    .. image:: /imagesla/Imagem19.jpg

**Figura 19:** Opções de frequência para trigger 

Junto a essa configuração básica, pode-se incluir outros parâmetros (Figura 20):
* **“Time Zone”**: fuso horário usado para definição de data e hora;
* **“Start Time”**: data e hora de início da trigger, no padrão. Atenção para que a data e hora especificadas estejam em conformidade com o fuso horário escolhido. Caso não seja especificado, a primeira execução será realizada imediatamente após salvar ou implantar o Logic App, independente da configuração de recorrência da trigger; 
* **“On these days”**: dias da semana em que a execução deve ocorrer. Disponível apenas para a frequência Week;
* **“On these hours”**: horas em que a execução deve ocorrer. Disponível apenas para as frequências Day e Week;
* **“On these minutes”**: minutos de cada hora em que a execução deve ocorrer. Disponível apenas para as frequências Day e Week 

    .. image:: /imagesla/Imagem20.jpg

**Figura 20:** Parâmetros adicionais para configuração de recorrência da trigger 

3.2.3. Office 365 Outlook 
---------------------------

Desse grupo, a mais comumente usada é **“When a new email arrives”**, que é disparada quando um email que atende a determinadas características é recebido. 

    .. image:: /imagesla/Imagem21.jpg

**Figura 21:** Triggers do tipo Office 365 Outlook 

O primeiro passo para configurar a trigger é fazer o login em sua conta Ipiranga. Para testes, é usado o email do engenheiro, mas quando o processo é produtizado, o email usado é o do Data Lake, svc.ipippdatalake-p@ultra.com.br.

    .. image:: /imagesla/Imagem22.jpg

**Figura 22:** Conectar à conta Outlook Ipiranga 

Feito o login, serão exibidas as opções de configuração da trigger, com parâmetros que podem ser adicionados.

* **“Folder”:** pasta onde o email é esperado. Podemos digitar o nome da pasta ou clicar no ícone de pasta à direita para exibir as pastas existentes na caixa de email;
* **“Importance”:** para filtrar por e-mails de acordo com a importância definida em seu envio, podendo ser: Any, Low, Normal, High; 
* **“Only with Attachments”:** “Yes” ou “No”, para filtrar apenas e-mails que contenham anexos;
* **“Include Attachments”:** “Yes” ou “No”, para incluir ou não o conteúdo dos anexos na resposta da trigger;
* **“Only with Attachments”**: “Yes” ou “No”, para filtrar apenas e-mails que contenham anexos;
* **“To”:** filtrar por e-mails endereçados a algum endereço de e-mail específico. No caso de múltiplos endereços, separá-los por ponto e vírgula, e caso haja correspondência com pelo menos um, a trigger executará;
* **“CC”:** semelhante ao anterior, porém verifica os destinatários CC do email;
* **“To or CC”:** semelhante aos dois anteriores, verificará os destinatários em “To” e “CC”;
* **“From”:** filtrar por e-mails enviados por remetentes específicos. No caso de múltiplos endereços, separá-los por ponto e vírgula, e caso haja correspondência com pelo menos um, a trigger executará;
* **“Subject Filter”:** filtrar por e-mails que tenham uma determinada string no assunto. Deve-se evitar o uso de filtros genéricos, aos quais e-mails de propósitos complementes diferentes possam atender. Portanto, o assunto do email deve ser alinhado com o responsável pelo envio, para garantir que seja bem definido

    .. image:: /imagesla/Imagem23.jpg

**Figura 23:** Parâmetros para configuração da trigger

Referência para outras triggers e ações do grupo Office 365 Outlook no link: 

* `triggers <https://docs.microsoft.com/pt-br/connectors/office365/#triggers>`_

* `actions <https://docs.microsoft.com/pt-br/connectors/office365/#actions>`_


3.2.4. SharePoint 
------------------

Desse grupo, as triggers mais usadas são aquelas relacionadas a criação e/ou alteração de arquivos em pastas, como **“When a file is created or modified in a folder”.**

    .. image:: /imagesla/Imagem24.jpg

**Figura 24:** Triggers do tipo SharePoint 

Primeiramente, é necessário criar a conexão, e para isso, deve ser usado o email Ipiranga do engenheiro.  

    .. image:: /imagesla/Imagem25.jpg

**Figura 25:** Criação de conexão SharePoint 

Os parâmetros para configuração das triggers desse grupo são parecidos, aqui exemplificaremos com os da trigger “When a file is created in a folder”. 

* **“Site Address”:** URL do SharePoint. É possível digitar ou selecionar, clicando na seta à direita, que exibirá os sites aos quais o engenheiro possui acesso;
* **“Folder Id”:** pasta em que o evento deve ocorrer. Clicando no ícone de pasta à direita é possível navegar pelas pastas do Sharepoint e selecionar a desejada;
* **“Infer Content Type”:** “Yes” ou “No”, para inferior o tipo de conteúdo do arquivo baseado na extensão do mesmo;
* **“How often do you want to check for items?”**: frequência usada para verificar atualização de arquivo na pasta especificada. É possível ainda adicionar parâmetros para determinar a data e hora de início de execução do aplicativo e fuso horário usado. 

    .. image:: /imagesla/Imagem26.jpg

**Figura 26:** Configuração da trigger SharePoint 

Atenção aos tipos de triggers e definição de pastas em sua configuração. Para algumas, é possível verificar eventos na biblioteca inteira, para outras, apenas dentro da pasta informada, sem considerar subpastas.  

Referência para outras triggers e ações do grupo SharePoint no link:  

* `triggers; <https://learn.microsoft.com/pt-br/connectors/sharepointonline/#triggers>`_

* `ações <https://learn.microsoft.com/pt-br/connectors/sharepointonline/#actions>`_

3.2.5. Azure Event Grid 
------------------------
Desse grupo, há apenas uma trigger disponível, **“When a resource event occurs”**, acionada quando ocorre um evento em algum recurso da subscription

    .. image:: /imagesla/Imagem27.jpg

**Figura 27:** Triggers do tipo Azure Event Grid 

Ao selecionar a trigger, passaremos para a tela de criação da conexão, que deve ser feita utilizando a autenticação de usuário do engenheiro. Para isso, basta clicar em **Sign** In e fazer o login na conta Ipiranga. 

    .. image:: /imagesla/Imagem28.jpg

Figura 28: Criação da conexão com o Azure Event Grid 

Na próxima tela é onde configuraremos o evento que deverá acionar o processo.

* **“Subscription”**: subscrição na qual o evento deve ocorrer;
* **“Resource Type”**: tipo de recurso para o qual o evento será criado. O mais comum é evento em uma storage account, portanto, selecionar “Microsoft.Storage.StorageAccounts”;
* **“Resource Name”**: nome do recurso em que o evento deve ocorrer, que em nosso caso, deverá ser o nome da storage account de LAB durante o desenvolvimento;
* **“Event Type Item”**: tipo de evento que a trigger deverá observar, por exemplo: blob created, blob deleted, directory created etc. Para adicionar mais de um tipo de evento, basta clica em **Add new item**; 
* **“Prefix Filter”**: no caso de eventos no storage, prefixo do diretório em que deve ocorrer o evento. Por exemplo, o prefixo “data/raw/dados_internos/input_user/” fará com que a trigger filtre apenas eventos dentro dessa pasta. Aqui é importante especificar o melhor possível o local em que o evento ocorrerá, no caso do diretório, até a subpasta mais interna.
* **“Sufix Filter”**: análogo ao anterior, porém se refere ao sufixo do evento. Por exemplo, usar o sufixo “.parquet” combinado ao prefixo anterior fará com que a trigger filtre apenas por arquivos com a extensão parquet dentro daquele diretório. Também pode ser usado o nome do arquivo.  

    .. image:: /imagesla/Imagem29.jpg


**Figura 29:** Configuração do evento que acionará a trigger 

Referência para a trigger do grupo Azure Event Grid no link triggers. 

3.3. Actions  
==============

Listadas abaixo estão as referências de algumas ações disponíveis para uso, para consulta dos engenheiros. 


* Control
    
    * `Condition <https://learn.microsoft.com/pt-br/azure/logic-apps/logic-apps-control-flow-conditional-statement>`_
    * `Loops <https://learn.microsoft.com/pt-br/azure/logic-apps/logic-apps-control-flow-loops>`_ : For each e Until
    * `Scope <https://learn.microsoft.com/pt-br/azure/logic-apps/logic-apps-control-flow-run-steps-group-scopes>`_
    * `Switch <https://learn.microsoft.com/pt-br/azure/logic-apps/logic-apps-control-flow-switch-statement>`_
    * `Branches <https://learn.microsoft.com/pt-br/azure/logic-apps/logic-apps-control-flow-branches>`_
    * `Criar variaveis <https://learn.microsoft.com/pt-br/azure/logic-apps/logic-apps-create-variables-store-values>`_

Para consultar a lista de conectores disponíveis, acesse o link `Logic App Connectors <https://learn.microsoft.com/pt-br/connectors/connector-reference/connector-reference-logicapps-connectors>`_, e para mais informações sobre Logic Apps, consultar a documentação da Microsoft, em `Logic App <https://learn.microsoft.com/pt-br/azure/logic-apps/>`_.

3.4. Exemplos
===============

Nesta seção serão especificados dois dos casos mais comuns de processos no Logic app, a saber, captura de email com anexo e envio de email de notificação, com um passo a passo para criação deles. Ainda, serão fornecidos nomes de outros processos já existentes em nosso ambiente, para que os engenheiros possam usar como referência.

3.4.1. Captura de email com anexo
------------------------------------

Para iniciar o processo, a trigger para acionamento será a explicada na seção 3.2.3. Identificado o email através da trigger, é necessário obtê-lo para trabalhar com suas informações, e para isso, a atividade usada é a **Get Emails**. 

    .. image:: /imagesla/Imagem30.jpg

**Figura 30:** Action Get emails 

Nos parâmetros da atividade, pode-se especificar alguns filtros que limitarão os emails recuperados, como verificar apenas e-mails não lidos, com anexos, enviados por um remetente específico etc. Aqui é importante selecionar “Yes” no parâmetro **“Include Attachments”**, para que o anexo do email seja incluído no retorno da atividade. O parâmetro **“Top”** determinará o número de e-mails a ser recuperado (recomenda-se manter o padrão de 10, para recuperar os últimos e-mails recebidos e depois filtrar o de interesse em outra atividade). 

O próximo passo é percorrer os e-mails recuperados pelo **Get emails**, e para isso usamos a atividade **For Each** do grupo **Control**, que fará uma iteração em cima do resultado da atividade anterior. Clique na caixa de texto da atividade para adicionar conteúdo dinâmico (Dynamic content), e selecione a opção de output **“value”** da atividade **Get emails**.

    .. image:: /imagesla/Imagem31.jpg

**Figura 31:** Atividade For Each 

Agora, adicionaremos uma nova atividade dentro do **For Each**, para realizar filtros que garantam que estamos recuperando o e-mail correto. Do grupo **Control**, usaremos a atividade **Condition**. Cada condição é representada por uma tríade, com: dado a ser comparado, operação de comparação e valor esperado.  

    .. image:: /imagesla/Imagem32.jpg

**Figura 32:** Atividade Condition 

Os filtros normalmente utilizados são referentes ao assunto do email e remetentes. No filtro de assunto, recomenda-se passá-lo para caixa alta, de forma a evitar disparidade na comparação, e para isso, combinaremos o uso de uma expressão com conteúdo dinâmico. Em **Expression** selecionaremos “toUpper”, e em **Dynamic content**, **“Subject”**

    .. image:: /imagesla/Imagem33.jpg

**Figura 33:** Incluindo condição de assunto, parte 1  

Em seguida, escolha o operador que melhor se adequa ao desejado (Figura 34) e o valor que é esperado (Figura 35). Pode-se comparar a equivalência com uma frase ou expressão inteira, ou verificar partes do assunto (Figura 35)  

    .. image:: /imagesla/Imagem34.jpg

**Figura 34:** Incluindo condição de assunto, parte 2  

    .. image:: /imagesla/Imagem35.jpg


**Figura 35:** Incluindo condição de assunto, parte 3 

Para adicionar uma nova condição, clique em **Add**, e escolha **Add Row** caso queira adicionar uma nova linha ao operador “And”, ou **Add Group**, caso queira adicionar um novo grupo com outro operador. Para verificar os remetentes, usaremos um novo grupo com o operador “Or”, de forma que basta que um dos remetentes atenda o esperado. Adicionaremos conteúdo dinâmico, selecionando a opção “From” de **Get emails**, o operador “is equal to” e os e-mails desejados. 

    .. image:: /imagesla/Imagem36.jpg

**Figura 36:** Incluindo condição de remetente 

Concluída a condição, é necessário definir as ações que serão tomadas caso ela seja atendida (True) ou não (False). 

Em caso negativo, é comum enviar uma de notificação para o email IpirangaRiodeJaneiroDataLake@ultra.com.br, usando a atividade **Send na email**, onde definiremos os destinatários, assunto e texto do email (além de outros parâmetros que podem ser incluídos).

    .. image:: /imagesla/Imagem37.jpg

**Figura 37:** Envio de email em caso de erro 

Em caso afirmativo, precisaremos verificar os anexos do email (considerando que é possível que no email contenha mais de um anexo), avaliando se o nome do arquivo é o esperado. Caso esteja correto, esse arquivo será salvo no Data Lake. Caso esteja errado, será enviado um email de notificação, da mesma forma já exemplificada anteriormente (Figura 37 acima) 

Para iniciar esse fluxo, será adicionada uma nova atividade **For each** dentro de **True**, utilizando como conteúdo dinâmico a opção “Attachments” de **Get emails**.

    .. image:: /imagesla/Imagem38.jpg

**Figura 38:** Atividade For each para verificar os anexos de um e-mail

Dentro do **For each**, será usada uma nova atividade **Condition**, para verificar o nome do arquivo anexo, utilizando como conteúdo dinâmico a opção “Attachments Name” de **Get emails**. Novamente, é possível incluir o nome esperado do arquivo como uma única condição (operador “is equal to”), ou verificar o nome por partes  (com o operador “contains” e múltiplas linhas de condição). 

    .. image:: /imagesla/Imagem39.jpg

**Figura 39:** Atividade Condition para verificar o nome de um anexo 

Caso o nome esteja fora do esperado, será enviado um email de notificação (incluindo uma atividade **Send an email** dentro de **False**), e caso esteja correto, passaremos para a gravação do dado no storage. Dentro de True, será incluída uma atividade Create **blob**. Para criar uma conexão, siga as orientações da seção 2.3.2. 

    .. image:: /imagesla/Imagem40.jpg

**Figura 40:** Configuração da atividade Create blob 

Em “Storage account name”, entre com o nome do storage, stippdatalakelab; em “Folder path”, com o diretório onde o arquivo deve ser salvo e em “Blob name”, com o nome do arquivo. O diretório e nome do arquivo devem seguir o padrão de camadas do Data Lake, e é possível defini-los dinamicamente utilizando expressões Se preciso incluir na construção do diretório a hierarquia de data, posso combinar as expressões **concat**, **formatDatetime** e **utcNow**

**concat('/data/raw/dados_internos/input_user/financeiro/ajustes_financeiros/', formatDateTime(utcNow(), 'yyyy'), '/', formatDateTime(utcNow(), 'MM'), '/', formatDateTime(utcNow(), 'dd'),'/')**

Assim como para o nome do arquivo: 

**concat('rw_exemplo_dev_logicapp_',formatDatetime(utcNow(),'yyyyMMdd_HH_mm'),' .xlsx')**

Por fim, o parâmetro “Blob content” define o conteúdo do arquivo a ser criado, e para isso adicionamos como conteúdo dinâmico a opção “Attachments Content” de **Get emails**. 

Para concluir esse fluxo, pode-se adicionar mais uma atividade **Send an email** para notificar os usuários sobre a conclusão com sucesso do processo. 

Caso seja esperado mais de um anexo em um email, podemos usar múltiplas atividades **Condition** aninhadas para identificar cada anexo e salvá-lo com diretório e nome adequados. A estrutura ficaria parecida com o exemplificado na Figura 41.   

    .. image:: /imagesla/Imagem41.jpg

**Figura 41:** Exemplo de atividades Condition aninhadas 

Suponhamos que o Logic app espere por 2 anexos, “Anexo A.xlsx” e “Anexo B.xlsx”. A atividade For each percorrerá os anexos contidos no email. A primeira condição verifica o primeiro anexo do loop corresponde ao arquivo “Anexo A.xlsx”, salvando-o em caso afirmativo; em caso negativo, passará para a segunda condição, para verificar se corresponde ao “Anexo B”, enviando um email de erro em último caso. Esse fluxo será repetido para cada anexo do email até a conclusão.

3.4.2. Envio de email de falha ou sucesso 
-------------------------------------------

O processo possui basicamente duas atividades, começando pela trigger para acionamento, que é a explicada na seção 3.2.1. O objetivo de enviar um email notificando sobre a finalização de um processo com sucesso ou falha. 

Para configurar o corpo da requisição, podemos usar o sample exibido na Figura 42, por exemplo. Os parâmetros importantes, nesse caso, serão o nome do processo em questão, o texto para o corpo do email, assunto e destinatário. 

    .. image:: /imagesla/Imagem42.jpg

Figura 42: Sample para gerar JSON Schema 

A segunda e última atividade é um **Send an email**, em que usaremos conteúdo dinâmico para preencher os parâmetros, utilizando a resposta da requisição.

    .. image:: /imagesla/Imagem43.jpg

**Figura 43:** Preenchimento de parâmetros com base da resposta da requisição 

É possível definir o corpo do email de forma mais livre no próprio Logic app. Se, por exemplo, o conteúdo de “emailBody” for apenas o tipo da notificação (“falha” ou “sucesso”), podemos concatenar apenas essa informação com um texto já definido.

    .. image:: /imagesla/Imagem44.jpg

**Figura 44:** Exemplo de construção do corpo do email utilizando conteúdo dinâmico 

3.4.3. Outros Exemplos
------------------------

* Email com anexo
   * logic-ipp-bipne-to-lake
* HTTP Request para envio de email
   * logic-ipp-datalake-custos-SendEmail
* Arquivo em STFP:
   * logic-ipp-custos-estoque-sftp
* Arquivo SharePoint:
   * logic-ipp-datalake-gst-riscos-operacoes-sharepoint
   * logic-ipp-email-inadimplencia
* Evento Storage:
   * logic-ipp-email-site-location-potencial-postos-rede-ativa
   * logic-ipp-email-market-share  


3.5. Testando o Logic app 
============================

Para realização de testes do seu Logic App, é possível executá-lo manualmente, selecionando **Run Trigger** e depois Run na barra de ferramentas do **Logic App designer** (Figura 45) ou no painel **Overview** (Figura 46). 

    .. image:: /imagesla/Imagem45.jpg

**Figura 45**: Executar aplicativo, Logic app designer 

    .. image:: /imagesla/Imagem46.jpg

No caso de triggers por agendamento, o Logic app será iniciado imediatamente. Caso a trigger do Logic app seja o recebimento de um email, por exemplo, será necessário provocar esse evento, enviando um email de fato, para que o aplicativo identifique esse email e inicie o fluxo. Se a trigger for do tipo HTTP Request, será exibido em **Run**, uma segunda opção **Run with payload** (Figura 47), em que forneceremos um json com os dados definidos para o corpo da requisição (Figura 48).  

    .. image:: /imagesla/Imagem47.jpg

**Figura 47:** Executar aplicativo com trigger HTTP Request

    .. image:: /imagesla/Imagem48.jpg

**Figura 48:** Executar aplicativo com trigger HTTP Request, Run with payload 

O fluxo será iniciado, e para visualizar a execução, clique em **View monitoring view** na tela que será exibida (Figura 49). 

    .. image:: /imagesla/Imagem49.jpg

**Figura 49:** Executar aplicativo com trigger HTTP Request, Output de Run with payload 

No painel de execução do Logic app, será exibida cada etapa da execução em questão, com o status de cada atividade e o tempo de execução de cada uma. 

    .. image:: /imagesla/Imagem50.jpg

**Figura 50**: Painel de execução de Logic app 

Para exibir mais informações sobre uma etapa, selecione-a, e esta será expandida, exibindo suas entradas e saídas, assim como possíveis erros

    .. image:: /imagesla/Imagem51.jpg

**Figura 51:** Detalhes de execução da atividade, parte 1

    .. image:: /imagesla/Imagem52.jpg

**Figura 52:** Detalhes de execução da atividade, parte 2 

Também é possível verificar o histórico de execuções através do painel **Overview**, em **Runs history**. Clicando na execução desejada, serão exibidos os detalhes das etapas, assim como mencionado anteriormente. 

    .. image:: /imagesla/Imagem53.jpg

**Figura 53:** Painel Overview, Runs history

Para mais informações sobre histórico de execuções de Logic apps, consultar documentação em Monitar aplicativos lógicos.  




















