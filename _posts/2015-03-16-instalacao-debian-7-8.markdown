---
layout: post
title:  "Instalação Debian 7.8"
date:   2015-03-16 00:00
categories: linux
---

Hoje veremos todos os passos para realizar a instalação da distribuição Debian em uma máquina. Usarei a versão mais recente do Debian, que é a 7.8 (Wheezy). Esta pode ser baixada em  <a href="https://www.debian.org/distrib/netinst">https://www.debian.org/distrib/netinst</a>. Nesta página encontraremos os links para as diferentes arquiteturas que o Debian tem disponibilidade. Se você está instalando o mesmo em um PC comum, que é o que eu acredito que queira fazer, selecione a opção i386(para máquinas com menos que 3GB de RAM) ou amd64.  A versão netinst é uma versão mais enxuta do Debian, porém que exige ao longo do processo de instalação, conexão com a internet para que alguns arquivos adicionais sejam baixados.

Após realizar o download, grave a imagem em alguma mídia e faça o boot da máquina pela mesma. Caso deseje, pode também fazer a instalação em uma máquina virtual, usando algum software de virtualização como o VirtualBox. 

<div class="img-container center">
<a class="fancybox" rel="group" href="/images/primeiro_passo.png" title="Passo 1"><img src="/images/primeiro_passo.png" alt="primeiro_passo" alt="" /></a>
</div>

Após realizar o boot pela mídia, uma tela de boas vindas é exibida. Para realizar a instalação podemos selecionar tanto "Install" quanto "Graphical install".  Iremos realizar a instalação em modo texto neste post. Ambas instalações tem os mesmos passos. As opções "Advanced options", "Help" e "Install with speech synthesis" não serão abordadas neste curso, mas se tratam respectivamente de: opções avançadas de instalação e aceso ao "Rescue mode", para recuperação de panes no sistema. "Help" é uma opção que exibe um manual da instalação do Debian, caso necessite de mais detalhes e a última opção se trata do modo de instalação com acessibilidade 


Nas próximas telas iremos encontrar várias opções de configuração, tanto da instalação quanto do sistema Debian. Eis elas:

* Linguage utilizada na instalação.
* Localização: utilizado para melhoramento na experiência do usuário, sugerindo fuso horário local, entre outras opções que sejam influenciadas pela geolocalização.
* Configuração do teclado: mostra a lista de layouts de teclado disponíveis, para o padrão brasileiro selecione 'brazilian'.
* Nome da máquina (hostname): digite um nome que identifique a máquina em questão na rede.
* Nome do domínio(domain name): caso sua rede tenha um domínio, digite aqui o nome do mesmo. Caso contrário deixe em branco.

Após as etapas acima, chegamos na etapa de configuração da senha de root. O root no Linux, é o usuário administrador, que possui permissões para fazer quaisquer alterações no sistema, e portanto só deve ser utilizado por pessoas experientes. Defina uma senha forte para este usuário, visto que uma senha fraca pode causar uma séria vulnerabilidade de segurança no sistema. Na próxima tela, re-confime a senha.

Após está etapa mais algumas tarefas de configuração: 

* Usuário para tarefas não adminsitrativas: será pedido um usuário para tarefas normais no Debian, digite os dados pedidos e prossiga.
* Configuração do fuso horário

Após estas etapas, estamos prontos para fazer a configuração do particionamento do disco, que veremos na próxima aula. Até lá.

Olá pessoal, prosseguindo a nossa instalação do Debian, iremos ver nesta aula como fazer o particionamento na instalação do Debian

Ao chegar em "Partition disks", será perguntado o modo como deseja particionar o disco da máquina. São nos apresentadas 4 opções:

* Guided - use entired disk: nesta opção o Linux irá ocupar todo o espaço disponível no disco
* Guided - use entired disk and set up LVM: nesta opção também será todo o espaço do disco. Porém, poderemos criar partições extras usando o LVM. O LVM é um utilitário do Linux, que permite criar "partições virtuais"  dentro de uma partição lógica normal. Não é escopo deste curso.
* Guided - use entired disk and set up encrypted LVM: Mesma opção anterior só que com criptografia.
* Manual: Versão onde temos total controle de como será particionado o disco. 

Neste curso, veremos como criar as partições usando a opção manual, que é a mais completa de todas. Ao selecionar manual veremos a tela abaixo. 

<div class="img-container center">
<a class="fancybox" rel="group" href="/images/particao_disco_inicio.png" title="Passo 1 Particionamento"><img src="/images/particao_disco_inicio.png" alt="primeiro_passo_particionamento"></a>
</div>

Nesta tela, a primeira opção nos leva para o particionamento assistido do menu anterior. Não usaremos esta opção. Logo abaixo, serão listado as partições criadas nos discos instalados na máquina. A próxima opção cancela todas as alterações no disco. E a última opção é para confirmar as alterações nas partições, escrevendo as alterações definitivamente  no disco. Lembrando que antes de selecionar esta opção, todas as alterações estão apenas em memória, o disco continua como era antes do particionamento. No meu caso selecionarei o disco "SCSI1" para iniciar o particionamento (Este nome pode variar conforme a tecnologia de disco utilizada, e a quantidade de discos instalados na máquina).

Ao selecionar o disco, é apresentada uma mensagem pedindo a criação de uma nova tabela de partições no disco. Esta mensagem aparece pelo fato do disco em questão estar vazio. Tudo bem com isso então selecionarei sim.

<div class="img-container center">
<a class="fancybox" rel="group" href="/images/particao_disco_segundo_passo.png" title="Passo 2 Particionamento"><img src="/images/particao_disco_segundo_passo.png" alt="segundo_passo_particionamento"></a>
</div>

Como podemos ver na figura acima, agora temos uma tabela de partições onde o único registro existente exibe o espaço livre para partições. Posicione o cursor sobre este espaço livre para criarmos nossa primeira partição. Então são exibidas 3 opções: 

* Create a new partition: exibe tela para criação manual da partição
* Automatically partition the free space: é uma opção bem interessante, onde são geradas as partições automaticamente para o usuário. Ainda podemos escolher entre as opções de criar apenas uma partição principal, uma partição principal + partição para /home e uma opção aonde uma partição é gerada para cada diretório de mais importância no Linux.
* Show Cylinder/Head/Sector information> apenas exibe quais são os números iniciais e finais dso cilindros, cabeças e setores que a partição em questão ocupa.

Neste curso iremos criar a partição manualmente, então selecione a opção Create a new partition. Primeiramente é nos pedido o tamanho da partição, no caso colocarei todo o espaço disponível, mas caso queira fazer mais partições coloque um número menor de sua preferência. Após, selecione o tipo de partição, primária ou lógica, neste caso usarei uma partição do tipo lógica. Na última tela, são exibidas as configurações da partição.

<div class="img-container center">
<a class="fancybox" rel="group" href="/images/particao_terceiro_passo.png" title="Passo 3 Particionamento"><img src="/images/particao_terceiro_passo.png" alt="terceiro_passo_particionamento"></a>
</div>

* Use as: Nesta primeira opção iremos escolher o sistema de arquivos que nossa partição usará. O Ext4 atualmente é o sistema de arquivos mais utilizado no Linux. Vamos então seleciona-lo usando a sua opção com journaling.
* Mount point: Esta opção se refere ao ponto de montagem da partição. O ponto de montagem "/" indica a raiz do sistema, e portanto será aonde será instalado todo o Linux. Podemos também definir pontos de montagem para diretórios específicos no Linux, como por exemplo o "/home" que é aonde fica armazenado os arquivos dos usuário da máquina.
* Mount options: várias opções para definir características especiais para a partição, um exemplo seria a capacidade de gerenciar cotas no disco. Não será abordada agora pois poderá ser reconfigurado com o sistema já instalado.
* Label: Aqui poderemos definir um nome mais amigável para nossa partição que será exibido em alguns pontos do sistema, é uma opção não obrigatório.
* Reserved blocks: esta opção é utilizada pelo usuário root para poder logar no sistema caso todas as partições estejam cheias. Com a capacidade aumentada dos hds modernos, utilizar a opção padrão de 5% de espaço é um exagero, diminua para 1%. 
* Typical usage: Define o tamanho mínimo que um inode ocupará no disco. 
* Bootable flag: Usado para dar boot em discos rígidos antigos, atualmente não é mais utilizado.

Configurada a partição selecione a opção "Done setting up the partition" para voltar a tela inicial, que ficará parecida com a imagem abaixo.

<div class="img-container center">
<a class="fancybox" rel="group" href="/images/particao_quarto_passo.png" title="Passo 4 Particionamento"><img src="/images/particao_quarto_passo.png" alt="quarto_passo_particionamento"></a>
</div>

No caso mostrado, temos apenas uma partição, de 10,7 GB no disco SCSI3, caso tivesse uma partição /home, a mesma seria exibida abaixo da nossa partição raiz ("/"). Vamos agora finalizar o particionamento selecionando a opção "Finish partitioning and write changes to disk". Ao selecionar esta opção, iremos receber um alerta de que não existe uma partição para swap. Não criarei a mesma neste curso, pois a opção em arquivo é mais recomendada, e será visto em um curso mais a frente. Apenas selecione "No" para continuar. Finalmente, selecione "Yes" para confirmar as alterações no disco.

Após este passo o sistema criará as partições e iniciará o processo de cópia dos arquivos. Ao término desta etapa será perguntado sobre o mirror para o package manager, selecione United States. e depois o ftp.us.debian.org. Este mirror, juntamente com o da Alemanha,  é um dos mais rápidos e estáveis do mundo. Na próxima tela será perguntado sobre informação de proxy, caso sua rede não utilize deixe em branco.

Após isso o apt-get buscará os arquivos necessários para a instalação. Ao terminar, será questionado sobre a configuração do "Popularity-Contest". Esta ferramenta avalia a popularidade dos pacotes pelos usuários. Recomendo colocar sim. Na proxima tela será oferecido alguns softwares para instalar no linux. Desmarque todos pois poderemos instalar eles, caso necessários, via APT. Na última etapa  o sistema irá fazer a instalação do GRUB Boot loader no disco, que é software responsável por gerenciar os sistemas instalados na máquina. Apenas selecione Yes.

Após isso, a instalação rodará os últimos passos e o sistema pedirá para reiniciar o computador. E assim terminar o nosso mini curso sobre instalação do Linux. Futuramente teremos atualizações caso algum tópico tenha ficado mal esclarecido ou conforme feedback dos usuários. Comentários abaixo e nos vemos em breve.