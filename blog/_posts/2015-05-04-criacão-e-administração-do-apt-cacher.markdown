---
layout: post
title:  "Criação e administração do APT Cacher"
date:   2015-05-04 00:00
categories: linux
---

Hoje, iremos abordar como instalar e configurar o repositório de pacotes local APT Cacher. O APT Cacher é uma ferramenta que armazena pacotes, evitando que se duas máquinas necessitem de um mesmo pacote, o mesmo tenha que ser baixado duas vezes, onerando desnecessariamente o link de internet. Com o armazenamento em cache do pacote, o mesmo será baixado apenas uma vez e armazenado localemente, podendo assim fornecer o pacote para quaisquer máquinas que necessitem dele.

Instalação

## Para instalar o APT Cacher rode o com

<pre>
# apt-get install apt-cacher
</pre>

Ao ser perguntado sobre qual opção de execução do apt-cacher, teremos três opções:


* daemon
* inetd
* manual

Selecione daemon.

## Configuração do client para uso do APT-Cacher

Para testarmos o funcionamento do APT-Cacher, iremos configurar a máquina servidora para buscar os arquivos do apt-cacher. Para isso, edite o arquivo “/etc/apt/sources.list”. Troque os arquivos que estarão neste formato:

<pre>
deb http://br.archive.ubuntu.com/ubuntu/ vivid main restricted
deb-src http://br.archive.ubuntu.com/ubuntu/ vivid main restricted
</pre>

Para este formato, substituindo o endereço 192.168.0.101 pelo IP ou hostname do seu servidor:

<pre>
# cat /etc/apt/sources.list
deb http://192.168.0.101:3142/br.archive.ubuntu.com/ubuntu/ vivid main restricted
deb-src http://192.168.0.101:3142/br.archive.ubuntu.com/ubuntu/ vivid main restricted
</pre>

Antes de executarmos um “apt-get update” para se checar se tudo está funcionando corretamente, vamos acessar o diretório /var/cache/apt-cacher/packages. Este diretório contém os arquivos dos  pacotes já baixados. Inicialmente esta pasta está vazia, mas ao executar o primeiro apt-get update. Alguns cabeçalhos já serão carregados.

<pre>
# ls /var/cache/apt-cacher/packages/
ftp.us.debian.org_debian_dists_wheezy-updates_Release
ftp.us.debian.org_debian_dists_wheezy-updates_Release.gpg
ftp.us.debian.org_debian_dists_wheezy-updates_main_binary-i386_Packages.bz2
ftp.us.debian.org_debian_dists_wheezy-updates_main_i18n_Translation-en.bz2
ftp.us.debian.org_debian_dists_wheezy-updates_main_source_Sources.bz2
ftp.us.debian.org_debian_dists_wheezy_Release
ftp.us.debian.org_debian_dists_wheezy_Release.gpg
ftp.us.debian.org_debian_dists_wheezy_main_binary-i386_Packages.bz2
ftp.us.debian.org_debian_dists_wheezy_main_i18n_Translation-en.bz2
ftp.us.debian.org_debian_dists_wheezy_main_source_Sources.bz2
security.debian.org_dists_wheezy_updates_Release
security.debian.org_dists_wheezy_updates_Release.gpg
security.debian.org_dists_wheezy_updates_main_binary-i386_Packages.bz2
security.debian.org_dists_wheezy_updates_main_i18n_Translation-en.bz2
security.debian.org_dists_wheezy_updates_main_source_Sources.bz2
</pre>

## Configuração do APT-Cacher na instalação do Debian

Um excelente uso do APT-Cacher é acelerar a instalação do sistema operacional em novas máquinas. Vamos criar uma nova instalação do Debian, apontando para o servidor de apt cacher que criamos.

Inicie a instalação normalmente conforme descrito no post x. Na etapa de seleção do repositório do apt, selecione a primeira opção (digitar informacao manualmente), tecle Enter para selecionar.  Coloque o caminho de rede do servidor no campo, ficando:



<pre>http://192.168.0.101:3142</pre>

Na próxima tela, o sistema irá pedir o diretório onde se encontra o repositório. O valor default é “/debian/”. Substitua-o por “/ftp.us.debian.org/debian”. Na próxima tela será pedida a informação a respeito do proxy, deixe-a em branco, visto que essa informação deverá ser configurada no apt-cacher. O restante da instalação permanece como usual.

É importante salientar, que apenas o repositório principal será utilizado na instalação. o repositório comum e de segurança ainda usarão as definições originais. Com isso não temos um ganho tão significativo de tempo na instalação do Debian.

Visualizando após a instalação o arquivo /etc/apt/source.list, constatamos isto. Configure as outras linhas adicionando o trecho “<ip>:3142”. Execute um apt-get update para se certificar que tudo está correto.

 Bom, por hoje era isto. Dúvidas, comentários e sugestões na área de comentários e até a próxima.
