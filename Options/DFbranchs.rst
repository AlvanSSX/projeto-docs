2. Branchs 
+++++++++++

O Data Factory tem a opção de trabalhar com o modo GIT, onde é realizado o versionamento de cada alteração realizada. Sempre que um novo desenvolvimento for iniciado, deve-se criar uma nova branch, a partir da master, seguindo o padrão de nome abaixo:

1. Para projetos que usam Jira ou outras ferramentas que atribuam números/identificadores às atividades:

    * Padrão: [identificador da tarefa]_[breve descrição]
    * Exemplo: tarefa de identificador DLE-123 no Jira para ingestão da dimensão DM_PRODUTO, branch com nome DLE_123_Ingestao_DM_PRODUTO.

2. Para projetos que não caem no caso acima:

    * Padrão: [nome do projeto ou squad]_[breve descrição]
    * Exemplo: tarefa do projeto PE Regional para manutenção da ingestão da base de ORABI, PR_LANC_INDICADOR_GPLANFIN_MES, branch com nome pe_regional_ajuste_tabela_fato