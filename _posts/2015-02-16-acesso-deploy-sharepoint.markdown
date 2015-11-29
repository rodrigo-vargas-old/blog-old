---
layout: post
title:  "Acesso deploy Sharepoint"
date:   2015-02-16 00:00:00 -0200
categories: sharepoint
---
Neste post, falarei sobre os passos necessários para conceder acesso de deploy, algo bastante simples e necessário para o desenvolvimento de webparts em ambiente Sharepoint. Para conceder acesso de deploy a um usuário precisamos executar duas etapas:

* Conceder permissão de administrador no server da Farm
* Conceder autorização nas tabelas de conteúdo do Sharepoint
* Conceder permissão de administrador no server da Farm

Acesse o painel de controle. Clique em Contas de Usuário, e conceder acesso a outros usuários neste computador. Irá aparecer uma janela com uma lista de usuários. Caso o usuário desejado não esteja nesta lista, clique em adicionar, preencha o nome do mesmo, e conceda permissões de administrador Caso o usuário já esteja na lista, clique nas propriedades, e o adicione como Administrador.

## Conceder autorização nas tabelas de conteúdo do Sharepoint

Acesse o SQL Server Management Studio. Na pasta Security, clique em Logins, clique no usuário selecionado e abra as propriedades do mesmo Em User Mapping, selecione Sharepoint_Config, e selecione as opções public, Sharepoint_Shell_Access e SPDataAccess. Em WSS_Content, selecione as mesmas opções. Após finalizar, clique em OK.