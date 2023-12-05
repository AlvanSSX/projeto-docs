5. Orientações gerais
++++++++++++++++++++++++

* Variáveis de ambiente devem ser usadas para fazer referência a storage account;
* Tokens e afins devem ser referenciados através de key vaults, que são criados pela equipe de Infra Cloud;
* Functions devem ter processamento curto;  Functions com processamento longo, devem ser orquestradas com funções duráveis;
* Functions que precisem de acesso a outros recursos devem usar a biblioteca de identidade;  Você pode configurar o seu ambiente com VS Code ou como melhor preferir;
* Todas as variáveis relacionadas ao ambiente devem ser definidas usando o json local.settings.json;
* Todas as funções precisam ter um ou mais casos de teste que devem ser versionados e fazem parte do processo de validação.

