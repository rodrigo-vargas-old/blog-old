---
layout: post
title:  "Logical Volume Manager"
date:   2015-03-30 00:00
categories: linux
---

Olá pessoal, no post de hoje vamos falar de um sistema alternativo para a criação de partições virtuais, o LVM (Logical Volume Manager). Com este sistema é possível criar áreas de dados, similares a partições, dentro de uma partição física ou lógica. Desta maneira, podemos superar a limitação de 15 partições lógicas que encontramos no particionamento a nível de disco. 

## Instalação

<pre>
apt-get install lvm2
</pre>

## Criação da estrutura


Após a instação do pacote, podemos começar a criar os PVs (Physical Volumes) ou volumes físicos. Para isto utilize o comando pvcreate seguido do caminho da partição. Primeiramente, vou listar todas as partições da minha máquina, usando o comando lsblk.


<pre>
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0    30G  0 disk 
??sda1   8:1    0     1K  0 part 
??sda5   8:5    0    14G  0 part /
??sda6   8:6    0     7G  0 part /home
??sda7   8:7    0     9G  0 part 
</pre>


Vou utilizar a partição sda7 para a criação do meu PV. Neste caso, a partiço sda7 não esta montada no linux, como podemos ver na coluna “MOUNTPOINT”. Caso a mesma estivesse montada deveriámos executar o comando umount para desmonta-la. Continuando, crie o PV utilizando comando:


<pre>
pvcreate /dev/sda7
</pre>

<p class="alerta">Todos os comandos devem ser executados como usuário root.


Abaixo da estrutura do PV, temos o agrupamento de volumes (VG). O VG é o responsável por permitir a grande flexibilidade do LVM. Com o VG podemos, criar uma partição que utilize múltiplos discos rígidos, veremos isso um pouco mais a frente. Por agora, vamos criar um VG dentro do nosso PV, executando o comando.

<pre>
vgcreate tutorial /dev/sda7
</pre>


Após criado, podemos utilizar o comando vgs, para visualizar os VGS instalados na máquina.


<pre>
root@rodrigo-VirtualBox:~# vgs
  VG       #PV #LV #SN Attr   VSize VFree
  tutorial   1   0   0 wz--n- 9,04g 9,04g
</pre>


Como último passo, temos a criação do volume lógico (LV), que é feito através do comando lvcreate. Como parametros passamos o tamanho do volume (-L), o nome do volume (-n) e por fim o nome do VG onde ser armazenado o nosso LV. Por fim, criaremos o nosso LV usando o comando:


<pre>
lvcreate -L100M -n logicalVolume tutorial
</pre>

Para obter, uma lista de logical volumes, digite o comando lvs.

<pre>
root@rodrigo-VirtualBox:~# lvs
  LV            VG       Attr      LSize   Pool Origin Data%  Move Log Copy%  Convert
  logicalVolume tutorial -wi-a---- 100,00m  
</pre>


Com o LV criado, já temos uma unidade de armazenamento pronta para ser formatada. Assim como todos os outros discos, ela se encontra no /dev, dentro do nosso VG. Para formata-la use o comando:


<pre>
mkfs.ext4 /dev/tutorial/logicalVolume
</pre>


Após formata-la, precisamos fazer a montagem da mesma, use os comandos abaixo para criar o diretório de montagem, e fazer o ponto de montagem em seguida.


<pre>
mkdir /tutorial
mount /dev/tutorial/logicalVolume /tutorial
</pre>

Após, vamos ver como ficou as partições do nosso sistema:

<pre>
df-h

Filesystem                          Size  Used Avail Use% Mounted on
/dev/sda5                            14G  4,5G  8,6G  35% /
none                                4,0K     0  4,0K   0% /sys/fs/cgroup
udev                                991M  4,0K  991M   1% /dev
tmpfs                               201M  896K  200M   1% /run
none                                5,0M     0  5,0M   0% /run/lock
none                               1001M   25M  977M   3% /run/shm
none                                100M   76K  100M   1% /run/user
/dev/sda6                           6,8G  379M  6,1G   6% /home
/dev/mapper/tutorial-logicalVolume   93M  1,6M   85M   2% /tutorial
</pre>

## Adição de novos PVs ao VG
Uma grande funcionalidade do LVM é a possibilidade de criar uma unidade lógica que esteja presente em várias partições lógicas “reais”. Para isto, vamos criar um novo PV em uma outra unidade, no meu caso irei montar na unidade sda6.


<pre>
#lsblk

NAME                              MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                                 8:0    0    30G  0 disk 
??sda1                              8:1    0     1K  0 part 
??sda5                              8:5    0    14G  0 part /
??sda6                              8:6    0     7G  0 part 
??sda7                              8:7    0     9G  0 part 
  ??tutorial-logicalVolume (dm-0) 252:0    0   100M  0 lvm 

#pvcreate /dev/sda6
</pre>
Após criamos um novo VG dentro do novo PV:

<pre>
#vgcreate tutorial2 /dev/sda6
</pre>

Após criado, vamos fazer a fusão dos dois VGS, o tutorial, nosso primeiro VG com o VG criado agora

<pre>
#vgmerge tutorial tutorial2
</pre>

Em seguida, vamos usar o comando vgs, para ver como ficou nossos VGs.

<pre>
# vgs
  VG       #PV #LV #SN Attr   VSize  VFree 
  tutorial   2   1   0 wz--n- 16,02g 15,92g
</pre>


Com a fusão dos VGs, temos muito mais capacidade na nossa unidade. Utilizando duas partições diferentes no mesmo volume lógico. No exemplo, utilizamos duas partições do mesmo disco rígido, mas poderíamos fazer com partições de discos rígidos distintos.


## Remoção


Para fazermos a remoção da estrutura, iniciaremos com a desmontagem das unidades, e após iremos remover as estruturas numa abordagem down-top, ou seja, iremos remover primeiro as estruturas filho para depois irmos removendo as estruturas pai.

Listando as partições:

<pre>
# df -h 

Filesystem                          Size  Used Avail Use% Mounted on
/dev/sda5                            14G  4,8G  8,2G  37% /
none                                4,0K     0  4,0K   0% /sys/fs/cgroup
udev                                1,4G  4,0K  1,4G   1% /dev
tmpfs                               276M  896K  275M   1% /run
none                                5,0M     0  5,0M   0% /run/lock
none                                1,4G   25M  1,4G   2% /run/shm
none                                100M   40K  100M   1% /run/user
/dev/mapper/tutorial-logicalVolume   93M  1,6M   85M   2% /tutorial
</pre>

Como, podemos ver, na última linha temos nossa LV virtual. Vamos desmonta-lo usando o comando:

<pre>
# umount /tutorial
</pre>

A seguir vamos listar todos os LVs criados:

<pre>
# lvs
  LV            VG       Attr      LSize   Pool Origin Data%  Move Log Copy%  Convert
  logicalVolume tutorial -wi-ao--- 100,00m  
</pre>
No caso, o único LV que temos é o “logicalVolume” que se encontra dentro de “tutorial”. Então vamos remove-lo usando o comando:

<pre>
# lvremove tutorial/logicalVolume

Do you really want to remove and DISCARD active logical volume logicalVolume? [y/n]: y
Logical volume "logicalVolume" successfully removed
</pre>

Agora é a vez dos VGs, listando e removendo:
<pre>

# vgs
  VG       #PV #LV #SN Attr   VSize  VFree 
  tutorial   2   0   0 wz--n- 16,02g 16,02g

# vgremove tutorial
  Volume group "tutorial" successfully removed
</pre>

E por último os PVs:

<pre>
# pvs
  PV         VG   Fmt  Attr PSize PFree
  /dev/sda6       lvm2 a--  6,98g 6,98g
  /dev/sda7       lvm2 a--  9,04g 9,04g
# pvremove /dev/sda6 /dev/sda7
  Labels on physical volume "/dev/sda6" successfully wiped
  Labels on physical volume "/dev/sda7" successfully wiped
</pre>
Por hoje era isso pessoal, deixem suas dúvidas e sugestões nos comentários abaixo e até a próxima.