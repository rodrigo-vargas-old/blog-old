---
layout: post
title:  "Agendamento de Tarefas no Linux"
date:   2015-03-23 00:00
categories: linux
---

No post de hoje irei explicar o funcionamento do daemon do linux responsável pelo agendamento de tarefas, o cron. O agendamento de tarefas é uma funcionalidade bastante útil para administrar uma rede com computadores linux, visto que boa parte das tarefas de administração pode ser automatizada. Além disso, é possível manter uma agenda de relatórios que pode ajudará a diagnosticar problemas isolados com maior eficácia.

## Criando uma tarefa no cron

O comando para administração do cron é o cronta. Através dele podemos visualizar e editar as tarefas para determinado usuário. Digitando o comando "crontab -l", teremos uma lista das tarefas cadastradas para o usuário.

<pre>#crontab -l </pre>
<pre>no crontab for user</pre>

O exemplo assim retorna que não temos tarefas agendadas para o usuário. Vamos fazer a adição de uma tarefa. Primeiramente digite o comando abaixo:

<pre>#crontab -e</pre>

Ao digitar este comando será aberto o arquivo de tarefas do cron. Ele será aberto usando o editor padrão definido no sistema.

Ao cadastrar uma tarefa no chronos a mesma tem a seguinte sintaxe:

<pre>minuto hora dia-do-mes mes dia-da-semana /caminho/comando</pre>

Como exemplo, vamos cadastrar uma tarefa que executa o comando "uname" a cada minuto, para isso digitaremos:

<pre>* * * * * uname</pre>

Isto indicará que o cron deve executar o comando "uname" a cada minuto. Caso quisessémos que este comando fosse executado apenas uma vez por dia, substituiriamos o valor e do minuto da hora por um valor fixo, como por exemplo:

<pre>15 23 * * * uname</pre>

Assim a tarefa seria executada apenas as 23:15 de cada dia. Ainda poderiámos definir que só enviariamos esta tarefa aos domingos, ficando:

<pre>15 23 * * 0 uname</pre>

Ainda temos outras possibilidades para os dias, usando os caracteres "-", "," e "/". O caractere "-" estabelece um período com início e fim, onde a tarefa irá ser executada. No exemplo:

<pre>15 23 * * 0-2 uname</pre>

O comando seria executado as 15:23 de domingo,segunda e terça. Se quisessemos adicionar quinta feira, substituiriamos o "0-2" por "0-2, 4". Ainda poderiamos usar o caractere "/" que traduzindo seria "a cada". Então para executar a mesma tarefa as 15:23 mas "a cada" 3 domingos, escreveriamos : 

<pre>15 23 0 0 0/3 uname</pre>

Fácil não? Porém como testar se o meu agendamento está funcionando? Isto é o que veremos agora. 

## Manipulando a saída dos comandos do cron
O cron possui como característica não produzir resultados no terminal, e sim através do envio de emails. Por padrão estes emails serão enviados para o usuário root da máquina. Para termos acesso a eles, podemos usar o comando "mail". Acessando um destes emails teremos uma saída semelhante a esta:

<pre>
From usuario@nomedamaquina Sat Mar 21 16:25:01 2015
Date: Sat, 21 mar 2015 16:25:01 -0300
From root@nomedamaquina (Cron Daemon)
To: usuario@nomedamaquina
Subject: Cron <usuario@nomedamaquina> uname
Content-Type: text/plain; charset=UTF-8
X-Cron-Env: <SHELL=/bin/sh>
X-Cron-Env: <HOME=/home/rodrigo>
X-Cron-Env: <PATH=/usr/bin:/bin>
X-Cron-Env: <LOGNAME=rodrigo>
Content-Length: 6

Linux
</pre>

Notamos que temos várias informações utéis, como por exemplo o nome da máquina que está enviando o email assim como o usuário logado. No corpo da mensagem temos a saída do comando "uname", no caso o valor "Linux". Deste modo podemos testar se nosso agendamento está funcionando como deveria. Entretanto, como normalmente não teremos como checar o email do usuário de cada máquina da nossa rede, seria melhor configurar um email único para todas.

##Configurando um email externo para o envio de mensagens do cron

Para configuramos um email externo no cron, temos duas formas de fazer isto. A primeira seria adicionar o seu email na linha "root:" do arquivo /etc/aliases, deste forma:

<pre>root:seuemail@exemplo.com.br</pre>

Após editar o arquivo /etc/aliases, execute o comando:
<pre>newaliases</pre>

Desta maneira, os alertas do cron serão enviados para o seu email. Lembrando que é necessário que esteja configurado um servidor de emails na máquina. A outra forma de redirecionar os alertas e adicionando o comando "MAILTO:" antes de cada tarefa no arquivo de configuração do cron (crontab -e)

Por hoje era isso pessoal, dúvidas, sugestões, comentários e críticas nos comentários, até a próxima. s