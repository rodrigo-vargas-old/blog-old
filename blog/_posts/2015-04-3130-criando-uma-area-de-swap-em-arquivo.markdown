---
layout: post 
title:  "Criando área de SWAP em arquivo"
date:   2015-04-13 00:00
categories: linux
---

Olá pessoal, no post de hoje irei mostrar os passos para criar uma área de SWAP em um arquivo na raiz do sistema. Deste modo, temos uma flexibilidade muito maior para a manipulação do SWAP da máquina, além de não termos que criar uma partiço exclusiva para isto.

## Criando o arquivo para SWAP

Primeiramente, vamos criar um arquivo contendo zeros, utilizando o comando dd, a sintaxe completa fica.

<pre>
# dd if=/dev/zero of=swapfile bs=1M count=500
</pre>

Explicando o comando acima, estamos criando um arquivo chamado “swapfile”, com 500 unidades de 1 MB, totalizando um arquivo com 500 MB.

Após criado o arquivo, crie uma área de SWAP neste arquivo utilizando o comando:

<pre>
# mkswap -f /swapfile
</pre>

Neste momento, o SWAP ainda não foi criado, veja com o comando “free -m”.

<pre>
# free -m
                  total      used       free     shared    buffers     cached
Mem:                1264     553        711          0          6        522
-/+ buffers/cache:          24         1239
Swap:                 0           0            0
</pre>

Para ativar o SWAP , utilize o comando swapon, passando o nome do arquivo.

<pre>
# swapon /swapfile
# free -m


                  total       used       free     shared    buffers     cached
Mem:                1264        553        710          0          6        522
-/+ buffers/cache:          25       1239
Swap:               499          0        499

</pre>

## Configurando o arquivo SWAP para ser criado durante inicialização da máquina

Para que o SWAP seja criado sem a interferência do administrador, devemos inserir a seguinte linha no arquivo /etc/fstab.

<pre>
/nome_do_arquivo_swap none swap sw 0 0
</pre>

Após inserir a linha, desative o swap utilizando o comando swapoff:

<pre>
#swapoff /swapfile
</pre>

Em seguida, vamos fazer a ativação de todos os arquivos swap definidos na máquina, que no caso do nosso post se refere apenas ao arquivo referenciado em /etc/fstab.

<pre>
# swapon -a
</pre>

Para testar o funcionamento, podemos utilizar o comando free -m.

Por hoje era isso pessoal, dúvidas, críticas e comentários podem ser feitos aqui embaixo. Até a próxima.