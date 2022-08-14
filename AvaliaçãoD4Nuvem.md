## Avaliação curso Computação em Nuvem - CTEDS

## Introdução 

Este documento descreve os recursos de infraestrutura necessários para um hipotético sistema de vendas de ingressos para shows e eventos, utilizando a Microsoft Azure.
A Aplicação será do tipo e-commerce com cadastro de usuário, fila de espera e pagamentos online.

##  Visão Geral

A aplicação no lado do cliente (frontend) será um PWA desenvolvido em React, utilizando localstorage para gerenciar e gravar dados do usuário.
No backend, será implementado uma API REST escrita em .NET com banco de dados Microsoft SQL Server.

## Recursos 

De início, serão provisionadas 2 VM's trabalhando em paralalelo para implentação de Load Balancer e com possibilidade de escalonamento horizontal, a depender do aumento de usuários da aplicação.
Cada VM contará com a seguinte configuração:

* 2 vCPUs Standard D2s_v3 
* 8 GB de Ram
* 150 GB de espaço em disco
* S.O. Ubuntu 20.04 LTS

Na camada de dados, será utilizado o banco de dados SQL Server com as seguintes configurações:

* Standard
* 10 DTUs
* 30 GB tamanho máximo 

Será utilizado o recurso de Load Balancer Standard.

### Diagrama da infraestrutura
![Digrama de infraestrutura](https://github.com/RenatoNr/d4-avaliacao/blob/master/assests/diagram.png)

### Balanceamento de carga 

Será utilizado o recusro de Load Balancer Standad da Azure para dividir o fluxo de trabalho da aplicação. 
A camada de backend será duplicada nas 2 máquinas virtuais, cada uma acessando o mesmo banco de dados. o Load Balancer vai permitir uma divisão do trabalho do sistema nos momentos de pico, e em caso de falha de uma das VMs, será direcionado o fluxo para a que está em funcionamento. A depender da demanda, nesta configuração fica mais fácil o escalonamento horizontal, adicionando mais VMs para suprir a demanda. 

## Arquitetura da aplicação

Será utilizado o padrão de Request-Reply assíncrono, pois será implementado o conceito de fila quando o usuário efetua a compra de ingresso(s). 
O cliente faz o processo de compra, dependo do fluxo de usuários ao mesmo tempo na aplicação, a requisição que é enviada ao backend entra em uma fila por ordem de requisição.
A aplicação retorna o status 202 de requisição aceita e vai pra fila de processamento. O cliente envia nova requisição e se esta ainda estiver na fila, retorna status 200 informando ao frontend qua ainda está pendente.
A aplicação frontend fica enviando requisições em determinado espaço de tempo e quando o trabalho no backend é concluído, é retornado status 302 de redirecionamento para uma URL contendo o comprovante do ingresso em PDF finalizando o processo de aquisição do ingresso.

### Diagrama requisições

![Diagrama API](https://github.com/RenatoNr/d4-avaliacao/blob/master/assests/api.png)

## Conclusão

A escolha por criação de máquinas virtuais pareceu mais viável para o projeto pois é o ambiente que estou mais familiarizado, não significando que opções como Containers não seriam melhores escolhas.
Pelo escopo da aplicação, um sistema de venda de ingressos onde o fluxo de requisições pode ser alto quando é ofertado um show por exemplo, um padrão de Request-Reply assíncrono me pareceu mais correto a ser aplicado.

