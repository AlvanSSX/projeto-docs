3. Testando Localmente o código desenvolvido
++++++++++++++++++++++++++++++++++++++++++++++

3.1  Variáveis de Ambiente
===============================

Para testar localmente serão necessárias algumas configurações:  Adicionar as variáveis de ambientes contidas no local.settings.json às variáveis de ambientes locais da sua máquina da seguinte forma: 

1 Pesquisar por “variáveis de ambientes” no menu do Windows:  

.. image:: /imagesaf/Imagem21.jpg

**Figura 21:** Imagem do Windows de onde editar as varáveis de ambiente. 

2 Clicar no menu “Variáveis de Ambiente...” contido na aba “avançado”:  

.. image:: /imagesaf/Imagem22.jpg

**Figura 22:** varáveis de ambiente.

3 Clicar no menu “novo” em “variáveis de sistema”: 

.. image:: /imagesaf/Imagem23.jpg

**Figura 23:** adicionando nova varável de ambiente localmente. 

4 Adicione as variáveis de ambientes contidas em local.settings.json, **NÃO** é necessário adicionar às variáveis as variáveis referentes às functions: 

.. image:: /imagesaf/Imagem24.jpg

**Figura 24:** variáveis a serem adicionadas estão destacadas de vermelho. 

3.2  VS Code 

1 No VS Code, abrir o terminal e executar o comando “az login”: 

.. image:: /imagesaf/Imagem25.jpg

**Figura 25:** abrir novo terminal no vs code. 

.. image:: /imagesaf/Imagem26.jpg

Irá abrir uma tela para efetuar login com sua conta da Ipiranga.

2 Na extensão da Azure do VS Code em “Local Project” desativar as functions que não irá testar. Neste caso irei testar a “af_fontesinternas_devops_pr_comments” então desabilitarei todas as outras.  

.. image:: /imagesaf/Imagem27.jpg

**Figura 27:** desabilitando a trigger das functions.

3 No repositório local abra o json referente à trigger da function a ser testada adicionar a seguinte linha:  "runOnStartup": true

   a Para TIMER TRIGGER:   

.. image:: /imagesaf/Imagem28.jpg

**Figura 28:** linha de comando para executar functions de TIMER TRIGGER. 

4 Para executar o teste da function vá até a opção de “debug”

.. image:: /imagesaf/Imagem29.jpg

**Figura 29:** debug da function para teste.

Verificar se está em “Attach to Python Functions” e para executar clicar.

