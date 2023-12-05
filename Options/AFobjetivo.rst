1. Objetivo
+++++++++++++

Em nossa arquitetura, usamos as Functions para construção de códigos que trazem dados de APIs, fazem web scraping em sites externos, ou mesmo tratamento de dados internos, e elas são executadas no ambiente Azure. 

Este documento contém orientações para o desenvolvimento de Azure Functions de acordo com o padrão definido pela equipe de Arquitetura, deploy desses objetos em nossos ambientes (produtivos ou de laboratório, quando houver), além de citar alguns requisitos para preparação do ambiente local para desenvolver e testar os códigos. 

Antes de iniciar o desenvolvimento, algumas instalações são necessárias para criar e executar functions localmente:

* Azure CLI
* .NET
* Node.js 