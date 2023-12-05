2. Fluxo de desenvolvimento
+++++++++++++++++++++++++++++

2.1. Resumo do Fluxo de desenvolvimento
=========================================

2.1.1. Criação de nova branch sempre a partir da branch master;
-------------------------------------------------------------------

2.1.2. Desenvolvimento na branch pessoal do engenheiro;
--------------------------------------------------------

2.1.3. Testes: 
---------------

Criação de Pull Request da branch pessoal para a branch dev.

* Neste passo, ao realizar o merge das branches, certifique-se de desmarcar a opção de deletar a branch de origem após o merge, pois ela será usada novamente posteriormente para produção. 

2.1.4. Criação de Pull Request da branch pessoal para a branch master (produção).
--------------------------------------------------------------------------------------

* A partir desse passo, o pull request passará pela avaliação do time de governança para seguir com a esteira de CI/CD e publicação em PRD. 


2.2. Ambientes de Functions
==============================

**Produção:** é a etapa final do ciclo de desenvolvimento, onde o código que foi desenvolvido, testado, avaliado e aprovado em ambientes de teste (como DEV ou homologação) é implantado e executado para uso real. Portanto, é fundamental garantir que o ambiente de produção seja altamente confiável, estável, seguro e escalável. 

   * Nome: **func-ipp-[AREA]-prd**

**Desenvolvimento:** O ambiente de desenvolvimento é um ambiente de trabalho criado para que os desenvolvedores possam projetar, codificar e testar o que estão desenvolvendo. É um espaço isolado onde os programadores podem criar e experimentar novas funcionalidades e soluções sem afetar diretamente o ambiente de produção ou os usuários finais. 

   * Nome: **func-ipp-[AREA]-dev**

Atualmente, há um ambiente legado, onde estão hospedadas algumas functions mais antigas, e novos ambientes, segregados por áreas da Ipiranga (cada projeto ou squad desenvolverá no ambiente da área a qual pertence). 

2.3. Clonar repositório localmente
====================================

Dentro de cada área de projeto no DevOps, haverá um repositório de function onde será feito o versionamento dos códigos. Para o ambiente legado, o repositório é **prjdatalake-webscraping**; para os novos ambientes, o nome segue o **padrão prj-[área]-afa** (por exemplo, para projetos que utilizam o ambiente do PCO, o local será https://dev.azure.com/ipiranga-dev/prj-pco/_git/prj-pco-afa). 

Para cada novo desenvolvimento, é necessário criar uma branch onde serão commitadas as alterações. Essa branch pode ser importada no VS Code, por exemplo, para desenvolver e testar localmente. 

1 Via DevOps, acessar o repositório apropriado;

2 Criar uma branch a partir da branch **master**.

   a Para exemplo, criamos uma de nome Deploy_passo_a_passo, mas padrão de nomes deve ser o mesmo utilizado para branchs dos demais ambientes: **[nome do projeto ou squad]_[breve descrição]**.

   
.. image:: /imagesaf/Imagem1.jpg

**Figura 1:** Exemplo criação da Branch a partir da Branch master


3 Abrir a branch criada, ir até opção **Clone** e copiar a URL;

.. image:: /imagesaf/Imagem2.jpg

**Figura 2:** Obtendo URL para clonar repositório, passo 1 

.. image:: /imagesaf/Imagem3.jpg

**Figura 3:** Obtendo URL para clonar repositório, passo 2 

4 Abrir o prompt do MS-DOS na pasta criada para o repositório e executar o comando: 

git clone <string do repositório> 

.. image:: /imagesaf/Imagem4.jpg

**Figura 4:** Clonando repositório via prompt de comando

5 Clonar pelo VS Code:

.. image:: /imagesaf/Imagem5.jpg

**Figura 5:** Clonar pelo VS Code. 

6 Irá abrir o VS Code. 

.. image:: /imagesaf/Imagem6.jpg

**Figura 6:** pop up para abrir o VS Code.

7 Será aberto o VS Code e necessário clicar em “abrir” novamente:

.. image:: /imagesaf/Imagem7.jpg

**Figura 7:** pop up para abrir no VS Code.

8 Selecionar a pasta que deseja alocar o repositório localmente: 

.. image:: /imagesaf/Imagem8.jpg

**Figura 8:** pasta de exemplo onde alocar o repositório localmente.


9 Abrir o repositório após clonar. 

.. image:: /imagesaf/Imagem9.jpg

**Figura 9:** pasta de exemplo onde alocar o repositório localmente. 

2.4. Instalação do Azure Tools
===============================

Costumamos usar o Visual Studio Code, e nele, é necessário instalar a extensão **Azure Tools**. 

.. image:: /imagesaf/Imagem10.jpg

**Figura 10:** Tela de instalação do Azure Tools no VS Code 

Com o Azure Tools instalado, abriremos a pasta local onde o repositório foi clonado. No menu superior, clicar em “Arquivo” (ou “File”) e depois em “Abrir pasta” (ou “Open folder”), e selecionar a pasta do repositório. 

.. image:: /imagesaf/Imagem11.jpg

**Figura 11:** Abrindo projeto local 

O projeto será aberto, e na barra inferior, no canto esquerdo, veremos a branch master sendo exibida (Figura 6), e ao clicar nela, aparecerá um menu suspenso onde a branch desejada deverá ser selecionada (Figura 7). 

.. image:: /imagesaf/Imagem12.jpg

**Figura 12:** Branch master

.. image:: /imagesaf/Imagem13.jpg

**Figura 13:** Menu para associar projeto à branch desejada 

Feito isso, via extensão da Azure (menu lateral esquerdo), na área **“Workspace”**, selecione o botão com símbolo do Azure Functions e a opção **“Create Function”** (que aparecerá em um menu suspenso).

.. image:: /imagesaf/Imagem14.jpg

Em seguida, será necessário escolher o modelo de função (Figura 9), e será solicitado definir um nome para a nova function (Figura 10). O padrão usado para nomear functions é o seguinte:

* **af_fontesexternas_[breve descrição]**, caso a function seja para ingestão de dados de uma fonte externa à Ipiranga, por exemplo, IBGE ou Denatran; 
* **af_fontesinternas_[breve descrição]**, caso a function seja para ingestão ou tratamento de dados internos à Ipiranga.  

.. image:: /imagesaf/Imagem15.jpg

**Figura 15:** Selecionar modelo para nova function

.. image:: /imagesaf/Imagem16.jpg

**Figura 16:** Definir nome para a nova function

Concluídos esses passos, será criada uma pasta local para a function, com o nome definido para ela, com alguns códigos de template (arquivo function.json e o novo arquivo de código do Python, explicados na próxima seção).

2.5. Padrão de desenvolvimento
===================================

No DevOps de cada área, no repositório de functions, as functions devem ser armazenadas na pasta **“Application”**, e cada function fica armazenada em sua respectiva pasta. Para cada uma delas, deve conter em sua pasta os arquivos:

* __init__.py: sendo este o arquivo onde estará o código principal;
* function.json: onde será definido o tipo de agendamento do código, seja por Timer Trigger, Event Trigger, HTTP Trigger etc. 

.. image:: /imagesaf/Imagem17.jpg

**Figura 17:** Repositório de function – pasta “Application” e folder de Function 

Abaixo de **“Application”**, há uma pasta chamada **“sharedCode”**, e outros dois arquivos de extrema importância,“af_[fonte]_[nome da function]”, **“local.settings.json”** e **“requirements.json”**. Detalharemos esses objetos a seguir. 

2.5.1. Pasta sharedCode
----------------------------

Armazena os jsons de parâmetros das funções, código de métodos comuns e arquivo de acesso a camadas. 

.. image:: /imagesaf/Imagem18.jpg

**Figura 18:** pasta sharedCode

af_[fonte]_[nome da function].json 

Arquivo de parâmetros da function, utiliza-se da seguinte estrutura

.. image:: /imagesaf/af1.jpg

.. image:: /imagesaf/af2.jpg

functions.py Arquivo .py que contém as funções padrão da Azure e a serem utilizadas pela function.

2.5.2. Arquivo local.settings.json
------------------------------------
 

Também na raiz da pasta **“Application”**, temos o arquivo **“local.settings.json”** no qual estão contidas as variáveis de ambiente para testar o código localmente. 

.. image:: /imagesaf/Imagem19.jpg

**Figura 19:** Exemplo de arquivo local.settings.json

2.5.3. Arquivo requirements.txt
---------------------------------

Assim como o arquivo **“requeriments.txt”** que armazena as libs que não são “built in” e precisam de instalação no Function App desejado. Lembrando que pode ser necessário definir o tipo de versão da lib utilizada, uma vez que se não o fizer, o processo de instalação de lib poderá buscar a versão mais recente.  

.. image:: /imagesaf/Imagem20.jpg

**Figura 20:** Exemplo de arquivo requirements.txt 

**Atenção!** Ao alterar as libs deverá ser garantido que não afetará outros processos contidos no ambiente de functions.




























