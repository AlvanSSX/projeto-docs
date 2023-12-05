5. Dados sensíveis
++++++++++++++++++++

No início de todo novo desenvolvimento deve-se verificar o schema da base sendo ingerida, de forma a garantir que informações sensíveis sejam direcionadas para o local correto. Quando uma ingestão se enquadrar nesse caso, deve ser direcionada para um storage apartado, ao qual o acesso é concedido mediante alinhamento com justificativa da necessidade. 

A storage account em questão é **stippdatalakelgpdhml**, e nela deve-se seguir o mesmo padrão de pastas do storage produtivo comum, sendo a única diferença o contêiner onde os dados estarão, **dados-sensiveis** (no lugar de **data**).  