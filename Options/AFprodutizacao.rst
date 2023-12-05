6. Fluxo de produtização
+++++++++++++++++++++++++++

O primeiro passo para produtizar um desenvolvimento ou alteração é sincronizar a branch local e commitar todas as mudanças feitas

1.1 Commitando pelo VS Code 

1.1.1 Vá até a aba de Git do VS Code e digite a mensagem do commit e confirme: 


.. image:: /imagesaf/Imagem34.jpg

**Figura 34:** commit utilizando vs code. 

1.1.2 Sincronizar as Alterações: 

.. image:: /imagesaf/Imagem35.jpg

 
**Figura 35:** sincronizando as alterações.

.. image:: /imagesaf/Imagem36.jpg

**Figura 36:** confirmar e efetuar o pull. 

1.2 Utilizando Git e CMD 

   1 Acessar o diretório montado

   2 Verificar se existem atualizações na branch e sincronizar com a branch local, utilizando os comandos git pull e git fetch.  

.. image:: /imagesaf/Imagem37.jpg

**Figura 37:** Exemplo de execução do comando git pull

2 Executar o comando **‘git add .’** para incluir as alterações do diretório aberto no próximo commit. 

3 Commitar as alterações através do comando **‘git commit -m** “descrição da alteração” ‘ 

.. image:: /imagesaf/Imagem38.jpg

**Figura 38:** Exemplo de execução dos comandos git add e git commit   

4 Subir o conteúdo do repositório local para a branch remota, através do comando:  git push -f <string do repositório> HEAD:<nome da branch de desenvolvimento>.  

.. image:: /imagesaf/af3.jpg

**Figura 25:** Exemplo de execução do comando git push  

1.3 Pull Request 

Feitos os commits na branch: 

   1 Antes de criar um Pull Request, é essencial ter realizado todos os commits necessários na branch que contém as alterações a serem incorporadas ao projeto. 

   2 Acessar o repositório do projeto via DevOps;

.. image:: /imagesaf/Imagem39.jpg

**Figura 39:** criação de pull requests via devops.  

3 Verificar as alterações listadas: No repositório, verifique se todas as alterações que foram realizadas e que você deseja incorporar ao projeto estão corretamente listadas. Isso é importante para garantir que todas as mudanças relevantes estejam incluídas no Pull Request.

4 Criar o Pull Request: Ao verificar que todas as alterações estão corretamente listadas, crie o Pull Request. Isso será feito através da interface do DevOps, selecionando a branch que contém suas alterações e criando o Pull Request a partir dela.  

.. image:: /imagesaf/Imagem40.jpg

Descrição do Pull Request: 

1 Ao criar o Pull Request, forneça uma descrição clara e concisa do objetivo da alteração que está sendo feita. Explique o que a alteração realiza, qual problema ela resolve ou qual funcionalidade ela adiciona. 

2 Identificar o projeto atendido: No corpo da descrição do Pull Request, identifique claramente a que projeto a alteração está atendendo. Isso é especialmente importante em projetos que possuem múltiplos repositórios ou dependências. 

3 Incluir evidências de testes bem-sucedidos: 

4 Certifique-se de incluir evidências que comprovem que a function (ou código afetado) foi testada e executou conforme o esperado. Isso pode incluir: 

   a Outputs no storage seguindo o padrão Ipiranga, caso se aplique.

   b Capturas de tela ou registros do Monitor da function, mostrando a execução bem-sucedida e sem erros

   c Se a alteração for uma correção de erro em produção, inclua evidências que demonstrem que o erro foi solucionado, como logs antes e depois da correção.

Revisar antes de enviar: 

Antes de enviar o Pull Request, revise todas as informações fornecidas na descrição. Garanta que as evidências de testes sejam claras e suficientes para validar as alterações realizadas. 

Enviar o Pull Request: 

Após revisar e se certificar de que tudo está correto, envie o Pull Request. Ele será avaliado pelos revisores do projeto, que irão analisar as alterações, os testes realizados e poderão aprovar ou solicitar ajustes adicionais. 

Lembre-se de manter um padrão consistente na descrição dos Pull Requests, facilitando a compreensão e revisão por parte dos colaboradores do projeto. Além disso, é fundamental seguir as diretrizes e políticas de contribuição do projeto para garantir que as alterações sejam integradas com sucesso.   

