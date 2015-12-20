---
layout: post
title:  "Gerência de arquivos no Linux"
date:   2015-04-20 00:00
categories: linux
---

Olá pessoal, no post de hoje irei falar um pouco sobre os principais comandos para gerência de arquivos no Linux. Segue a lista de comandos:

* ls
* rm
* cp
* mv
* locate
* updatedb

## lsd
O comando ls é usado para listar todo o conteúdo de determinado diretório. É com certeza um dos comandos mais utilizados quando se está navegando em diretórios via terminal. Suas principais chaves são:

-a : Mostra todos os arquivos e diretórios, inclusive os ocultos.

<pre>
rodrigo@Falcon:/var$ ls -a
.   backups  crash  local  log   metrics  run    tmp
..  cache    lib    lock   mail  opt      spool
</pre>

-l: Mostra detalhes com permissões de acesso, tamanho e data da gravaço.

<pre>
rodrigo@Falcon:/var$ ls -l
total 44
drwxr-xr-x  2 root root     4096 Abr 20 19:03 backups
drwxr-xr-x 17 root root     4096 Jul 22  2014 cache
drwxrwsrwt  2 root whoopsie 4096 Jul 22  2014 crash
drwxr-xr-x 67 root root     4096 Abr 19 13:37 lib
drwxrwsr-x  2 root staff    4096 Abr 10  2014 local
lrwxrwxrwx  1 root root        9 Abr 18 22:48 lock -> /run/lock
drwxrwxr-x 13 root syslog   4096 Abr 20 18:58 log
drwxrwsr-x  2 root mail     4096 Jul 22  2014 mail
drwxrwsrwt  2 root whoopsie 4096 Jul 22  2014 metrics
drwxr-xr-x  2 root root     4096 Jul 22  2014 opt
lrwxrwxrwx  1 root root        4 Abr 18 22:48 run -> /run
drwxr-xr-x  9 root root     4096 Jul 22  2014 spool
drwxrwxrwt  2 root root     4096 Abr 20 19:12 tmp
</pre>

## rmdRemove arquivos e diretórios. Suas principais chaves são:

-r : Remove recursivamente. Ou seja, no caso de diretórios irá remover os arquivos e subdiretórios dentro do mesmo.

## cpd
Copia arquivos e diretórios. Sua sintaxe é:

<pre>cp nome_do_arquivo_ou_diretorio nome_destino_do_arquivo_ou_diretorio</pre>

## mvd
Move arquivos de diretório. Tambm  utilizado para renomear arquivos, movendo os mara o mesmo lugar. Sua sintaxe é:

<pre>mv nome_do_arquivo destino/nome_do_arquivo</pre>

## located
O locate é um comando bastante veloz para a localização de arquivos no disco. O locate utiliza-se de um indíce de arquivos que é atualizado através do comando updatedb.

<pre>
# locate phpmyadmin
locate: warning : database ‘/var/cache/locate/locatedb’ is more than 8 dias old (actual age is 37,9 days)
/etc/apache2/sites-available
/etc/apache2/sites-available/default
/etc/apache2/sites-available/default-ssl
</pre>

## updatedbd
Gera a base de dados a ser consumida pelo comando locate. Este comando indexa todas as unidades montadas na máquina, inclusive mídias removíveis, como disquetes, unidades flash, DVDs, etc.
