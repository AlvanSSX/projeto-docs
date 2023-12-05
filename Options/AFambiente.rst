4. Testando no ambiente de Desenvolvimento
++++++++++++++++++++++++++++++++++++++++++++

 
Para testar o código no ambiente de functions de desenvolvimento deverá ser feito um Pull Request para a Branch **“dev”**.  

**IMPORTANTE!** Atenção ao criar o Pull Request para dev e desmarcar a opção de deletar a Branch.  

1 Criação do Pull Request apontando para Branch de dev: 

.. image:: /imagesaf/Imagem30.jpg

**Figura 30:** criação do pull request para dev. 

2 Em DEV NÃO é necessário enviar o pull request para aprovação, somente quando for enviado a produção. 

3 No canto superior direito selecionar “complete”: 

.. image:: /imagesaf/Imagem31.jpg

**Figura 31:** botão de complete.

4 **DESMARCAR A OPÇÃO DE DELETAR BRANCH ANTES DE COMPLETAR O MERGE:**

.. image:: /imagesaf/Imagem32.jpg

**Figura 32:** completando o merge para dev. 

5 Abrir o function app de dev: func-ipp-[AREA]-dev 

6 Abrir a function a ser testada e apertar “Test/Run”

.. image:: /imagesaf/Imagem33.jpg

**Figura 33:** function utilizada para o exemplo de test/run. 

Obs: Teste feito apenas para functions com trigger de timer, para trigger de evento será necessário criar o evento e para function de post também será necessário gerar o post para poder ativa-la.   


