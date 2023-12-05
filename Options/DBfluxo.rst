4. Fluxo de produtização
++++++++++++++++++++++++++

4.1 Versionamento
====================

Será demonstrado a forma mais atual de versionamento dos notebooks no DevOps para posterior produtização, que é através do uso da funcionalidade “Repos” do Databricks. 

4.1.1 Utilizando a funcionalidade Repos
-----------------------------------------

No DevOps do projeto, o engenheiro deverá criar uma branch no repositório do Databricks, cujo nome segue o padrão **prj-[área]-adb**, a partir da branch principal (main/master), conforme imagem abaixo: 

    .. image:: /imagesdb/Imagem9.jpg

**Figura 9**: Criando Branch no repositório  

Após branch criada, é hora de obter o link para realizar o clone do repositório diretamente no Databricks. 

    .. image:: /imagesdb/Imagem10.jpg

**Figura 10**: Obtendo link para clone  

No ambiente de desenvolvimento, dentro de **Repos > pasta usuário**, adicionar o repositório o qual foi feito o clone anteriormente.

    .. image:: /imagesdb/Imagem11.jpg

**Figura 11**: Adicionando repositório clonado

Após colar o link copiado, as demais informações como Git provider e Repository name são preenchidos de forma automática, basta seguir para a criação do repositório. 

    .. image:: /imagesdb/Imagem12.jpg

**Figura 12**: Finalizando criação de repositório   

Seu ambiente terá algo como: 

    .. image:: /imagesdb/Imagem13.jpg

**Figura 13**: Resultado pós criação do repositório 

Para selecionar a branch criada anteriormente, basta seguir os passos destacados, uma nova janela será mostrada a qual possibilitará escolher a branch desejada. 

    .. image:: /imagesdb/Imagem14.jpg 

**Figura 14**: Selecionando branch  

**Obs.:** Este processo de clonar o repositório é necessário apenas uma vez. 

Para criar novas branchs de desenvolvimento, basta seguir os passos:

* Dentro do seu usuário e no repositório desejado, ao clicar em branch uma nova aba será exibida, certifique-se de que selecionou a **branch principal (main/master)**, aplique um pull para sincronizar os ativos, após isso é só criar sua nova branch.   

    .. image:: /imagesdb/Imagem15.jpg  

**Figura 15**: Criando nova branch a partir da principal (main/master)

É possível verificar os commits realizados na branch através do DevOps. 

    .. image:: /imagesdb/Imagem16.jpg  

**Figura 16**: Exemplo 2 de tela de histórico de commits de um notebook no repositório 

Caso necessário, é possível recuperar uma versão anterior de um notebook através da aba de **Last edit**. Nesse exemplo, apagamos uma célula que já havia sido commitada (Figura 17). Navegando pela aba lateral direita, podemos visualizar as versões anteriores do notebook, e para restaurar a versão desejada, basta clicar sobre ela e então na opção **Restore this revision** (Figura 18). Aparecerá uma mensagem para confirmação e, feito isso, o notebook estará de volta ao seu estado anterior (essa ação conta como uma alteração, portanto, é necessário commitar novamente). 

    .. image:: /imagesdb/Imagem17.jpg  

**Figura 17**: Exemplo de restauração de notebook a uma versão anterior, parte 1

    .. image:: /imagesdb/Imagem18.jpg 

**Figura 18**: Exemplo de restauração de notebook a uma versão anterior, parte 2 

4.2 Criação de Pull Request
=============================

Após a conclusão dos desenvolvimentos e realizado o processo de versionamento para cada notebook alterado/criado, é hora de criar o Pull Request, então neste caso, voltar para sua branch (itens 1, 2 e 3 **Figura 19**) ou simplesmente clicar na branch a qual é mostrada no notebook do desenvolvimento (**Figura 20**) e seguir os demais passos: 
   
    **4** Incluir mensagem do commit (requerido) assim como uma breve descrição (opcional) 
   
    **5** Realizar o **commit & Push** das mudanças, caso deseje realizar o commit para apenas alguns processos, basta desmarcar as caixas a esquerda onde mostra como **“changed file”** no exemplo temos apenas 1 arquivo em evidencia. A alteração escolhida será commitada, e as demais permanecerão pendentes. Uma mensagem de sucesso deve aparecer nessa mesma tela. 
   
    **6** Após o commit, é possível visualizar um link para criar um pull request, este link é apenas um facilitador para direcionar ao repositório diretamente a aba Pull Requests. 
   
    **7** Nesta etapa de criação de Pull Request basta seguir o preenchimento do **Checklist** o qual será exibido na tela, importante preencher/conferir cada tópico do **checklist**, pois são parâmetros considerados durante as avaliações, por fim clicar em **“Create”** para concluir o processo.  

    .. image:: /imagesdb/Imagem19.jpg 

**igura 19**: Criando Pull Requets

    .. image:: /imagesdb/Imagem20.jpg 

**Figura 20**: Selecionando branch para criar pull request. 


    .. image:: /imagesdb/Imagem21.jpg 

**Figura 21**: Preenchendo checklist antes do **“Create”**

Preenchidas as informações do Checklist contendo as evidencias necessárias/requisitadas e criado o PR, este cairá para avaliação do time de Governança Técnica, que fará análise e aprovação das alterações. 

Após a aprovação do PR, o pipeline de CI/CD fará a passagem das alterações para o ambiente produtivo, workspace **dtb-ipp-prd**.   

