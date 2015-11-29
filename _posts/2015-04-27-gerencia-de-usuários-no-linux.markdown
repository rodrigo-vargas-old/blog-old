---
layout: post
title:  "Gerência de Usuários no Linux"
date:   2015-04-27 00:00
categories: linux
---

Olá pessoal, no post de hoje irei falar sobre alguns comandos úteis na gerência de usuários. Basicamente, para gerenciar os usuários de uma máquina, precisamos de comandos para adicionar usuários, remover usuários e alterar suas senhas, onde no Linux, temos:

* adduser
* userdel
* passwd
* teste

## adduser

O comando 'adduser' é o comando mais completo para a criação de usuários. Sua sintaxe é:

<pre>
# adduser rodrigo
</pre>

Executando o comando acima iremos criar o usuário 'rodrigo' no sistema. Após rodar este comando uma série de informações serão pedidas, todas são opcionais. Caso não queira responder alguma, apenas tecle enter. Após as perguntas, o usuário rodrigo estará criado no sistema, juntamente com o seu diretório home e suas variáveis de ambiente.

## userdel

Exclui um determinado usuário do sistema. Sua sintaxe é:

<pre>
# userdel rodrigo
</pre>

Juntamente com o comando userdel, podemos utilizar a chave:
-r : remove inclusive o diretório home do usuário

## passwd
O comando passwd é usado para cadastrar ou alterar a senha de um usuário. Também pode bloquear um usuário. Sua sintaxe é:
<pre>
# passwd rodrigo
</pre>
Juntamente com o comando passwd , podemos utilizar as chaves:
-l : bloqueia o usuário
-u : desbloqueia o usuário

Por hoje era isso pessoal, críticas e comentários abaixo e até a próxima.
