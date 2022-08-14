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

## Diagrama da infraestrutura


## Arquitetura da aplicação

Será utilizado o padrão de Request-Reply assíncrono, pois será implementado o conceito de fila quando o usuário compra um ingresso. 
O cliente faz o processo de compra de um ticket, dependo do fluxo de usuários ao mesmo tempo na aplicação, a requisição é enviada ao backend e entra em uma fila por ordem de requisição.
A aplicação retorna o status 202 de requisição aceita e vai pra fila de processamento. Após 
