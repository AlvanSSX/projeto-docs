6. Publicação de Alterações 
++++++++++++++++++++++++++++

Terminado o desenvolvimento, as alterações devem ser publicadas, seguindo alguns passos e requisitos descritos nesta seção. 

6.1. Criação de Pull Request
=============================

Para publicar as alterações feitas é necessário criar um Pull Request. Antes de qualquer coisa, é recomendado validar a branch pelo próprio Data Factory, usando a opção **Validate all**, que retornará se a branch está ok, ou se possui qualquer inconsistência, indicando, nesse caso, onde estão os erros. 

   .. image:: /imagesdf/Imagem19.jpg

   **Figura 19:** Exemplo de onde acionar a opção “Validate all” 

Validada a branch, pode-se criar o pull request para seguir com o deploy das alterações. Com a branch em questão aberta, basta clicar na seta ao lado do nome da branch e ir até a opção **Create pull request**, que o direcionará para uma página do DevOps para inserir as informações a respeito do pull request. 

   .. image:: /imagesdf/Imagem20.jpg

   **Figura 20:** Exemplo de como criar Pull Request a partir do ADF 

Nessa página, é mandatório preencher os comentários para o versionamento, identificando o objetivo da alteração sendo feita, o número da tarefa que gerou a manutenção/desenvolvimento e o projeto a que está atendendo. 

Projeto: nome do projeto ou squad 

Tarefa: identificador da tarefa, se houver 

Descrição: descrição da demanda, explicando o objetivo de novos desenvolvimentos ou motivo das alterações, listando os objetos e/ou processos afetados. 

Evidências de execução: incluir evidências de execução e validação da branch, com prints dos itens: execução dos pipelines (aba output); arquivos gerados no storage; Factory Validation Output (com data e hora). 

**Exemplo:** 

Squad: Pricing ME

Tarefa: DLE-1026

Descrição: Foi construído o pipeline "pip_ap_parecer_acao_preco" , uma vez que para replicar o relatório de Margem Canceladas se faz necessário o uso das informações contidas nesta tabela. O pipeline está vinculado na trigger "trg_schedule_7_30_am". 

Diretório no Data Factory: fontes_internas/oracle/orasn/apco/ 

Diretório no Data Lake: raw/dados_internos/sistema_de_negocio/apco/ap_parecer_acao_preco/yyyy/ mm/dd 

E sobre o pipeline "pip_margem_em_fluxo", foi adicionado o passo "dl_parquet_refined" para deletar os parquets que já estavam no diretório antes da inserção de novos dados. 

6.1.1.	Resolução de conflitos
--------------------------------

Se a criação do pull request indicar existência de conflitos entre a branch de origem e a master, tais conflitos devem ser analisados. Em geral, as alterações já existentes na master devem ser sempre mantidas. Quando um conflito for relacionado a triggers, é provável que duas branchs tenham sido criadas aproximadamente na mesma época, ambas incluíram pipelines numa mesma trigger, mas uma foi produtizada antes. Nesse caso, mantém-se a versão da master e, depois de finalizado o pull request, cria-se uma nova branch e pull request apenas para inclusão da trigger. Casos mais complexos devem ser analisados junto a equipe de Governança Técnica. 

6.2. Finalização do Pull Request
==================================

Algum membro do grupo de aprovação poderá questionar algum ponto ou solicitar ajustes no pull request, para adequar aos padrões definidos. Após aprovado o PR, deverá ser concluído pelo engenheiro que o criou. 

   .. image:: /imagesdf/Imagem21.jpg

   **Figura 21:** Exemplo de como completar um Pull Request 

Ao completar o merge entre as branchs, é orientado que a branch de origem seja excluída. 

   .. image:: /imagesdf/Imagem22.jpg

   **Figura 22:** Exemplo de onde marcar a opção de deletar a branch após finalização do Pull request 

Completada a etapa realizada no Azure DevOps, deve-se retornar ao Data Factory para publicar o que foi feito. Recomenda-se novamente realizar a validação da branchs, pelo **Validate all**, e então publicar, pela opção **Publish** (exibida na Figura 19). As alterações serão listadas e o desenvolvedor deverá dar o Ok para a publicação iniciar. 

   .. image:: /imagesdf/Imagem23.jpg

   **Figura 23:** Exemplo de tela de publicação de alterações no ADF 

Após o **Ok**, no ícone de notificação poderá ser conferido se a publicação e a geração de ARM foram concluídas com sucesso. 

   .. image:: /imagesdf/Imagem24.jpg

   **Figura 24**: Exemplo de notificação de publicação com sucesso 

Ocorrendo algum erro na publicação das alterações, deve-se verificar os detalhes para entender que ação deve ser tomada. Caso essas alterações contenham deleção de um pipeline, por exemplo, um erro associado a **Lock** do recurso será retornado (Figura abaixo). Nesses casos, a orientação é procurar a equipe de Governança Técnica para que o lock seja removido e a publicação concluída. 

   .. image:: /imagesdf/Imagem25.jpg

   **Figura 25:** Exemplo de erro por Lock 



