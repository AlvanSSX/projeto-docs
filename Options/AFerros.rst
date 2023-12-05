7. Possíveis Erros
++++++++++++++++++++++   

7.1 Erro de Credenciais expirados (token)
============================================

 Caso ao tentar executar retorne o erro de token expirado no Azure AD seguir os seguintes passos:
 
 2 Apertar Win + R e digitar: %LOCALAPPDATA%\.IdentityService\ 

.. image:: /imagesaf/Imagem41.jpg

**Figura 41:** tela do Windows + R.

3 Apagar o arquivo “msal.cache” ou “msalV2.cache”
4 Apertar Win + R novamente e digitar: C:\Users\\[NOME DO USUARIO]\.azure\ 


.. image:: /imagesaf/Imagem42.jpg

5 Apagar os seguintes arquivos:

* msal_token_cache.bin
* msal_http_cache.bin 

6 Executar o comando “az login” novamente no terminal.

7 Executar debug novamente. 

7.2 Erro de autorização local (máquina)
=========================================

* executar no powershell: 

Get-ExecutionPolicy -List Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser 


7.3 Erro de Directory Corrupt 
================================

.. image:: /imagesaf/Imagem43.jpg

**Figura 43:** imagem do erro de “directory corrupt” ao realizar deploy. 


Este erro geralmente é devido a extensão do VS Code, link do erro:

https://learn.microsoft.com/en-us/answers/questions/1328281/central-directory-corrupterror-on-vscode-deploy 

Para resolver basta reverter a extensão da Azure Functions no VS Code para a versão anterior.  

.. image:: /imagesaf/Imagem44.jpg

**Figura 44:** imagem da resposta corrigindo o erro informado. 



