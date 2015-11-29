---
layout: post
title:  "Configurando um ambiente para o Ruby on Rails"
date:   2015-08-10 00:00
categories: ruby-on-rails
---

Se você quer aprender sobre Ruby on Rails, nesta série iremos falar desde como configurar um ambiente para desenvolvimento no Rails, passando pela criação dos controles, models, rotas e todas as configurações necessárias para construirmos nossas primeiras aplicações em Rails. 

Neste primeiro post, irei mostrar como configurar um ambiente rails utilizando dois softwares muito úteis, o virtual box e o vagrant.

## Instalação do Virtual Box e do Vagrant

O Virtual Box é um software que irá emular uma máquina virtual dentro da sua máquina. Deste modo conseguimos isolar nosso ambiente de desenvolvimento da nossa máquina primária. Isto é bastante útil q para não poluirmos nossa máquina principal, com servidores de banco de dados e etc. O Vagrant é responsável por tornar as tarefas de gerência destes ambientes mais fáceis, além de possuir as boxes, que permitem criar ambientes prontos, que podem ser rapidamente baixados e instalados. 

Primeiramente baixe o software Virtual Box no site oficial https://www.virtualbox.org/wiki/Downloads conforme a versão do seu sistema operacional. Faça a mesma coisa para o Vagrant, https://www.vagrantup.com/downloads.html . São apenas instalações simples, qualquer dificuldade posso ajudar nos comentários. Após instalado, abra um novo console, e crie um novo diretório com um nome para identificar sua vm(virtual machine). Em sistema unix você pode usar o comando mkdir para esta tarefa. Após isso acesse esse diretório.

Em seguida, iremos fazer o download de uma box vagrant que servirá como base para nosso ambiente Rails. Eu escolhi para este tutorial, uma box com Ubuntu 14.04 LTS, que é a versão de suporte extendido do Ubuntu atualmente.

Para baixar a box utilize o comando:

<pre>vagrant box add ubuntu/trusty64</pre>

Aguarde o término do download da máquina. Após baixar, use o comando abaixo para iniciar uma nova instância do vagrant.

<pre>vagrant init ubuntu/trusty64</pre>

Este comando irá criar um arquivo chamado VagrantFile no diretório atual. Abra este arquivo e iremos ver algumas linhas do mesmo. Na linha “config.vm.box” temos o nome da box que este arquivo irá iniciar, certifique-se que o valor do mesmo seja:

<pre>config.vm.box = "ubuntu/trusty64"</pre>

Localize a linha:

<pre>  # config.vm.network "private_network", ip: "192.168.33.10"</pre>

Esta linha deve ser descomentada, retirando o ‘#’ na frente da mesma. Assim habilitamos uma rede interna, para que possamos acessar a máquina virtual a partir da nossa. para quem tiver interesse, existem outras configurações intessantes neste arquivo, no momento não irei cita-las, talvez em outra série.

Salve o arquivo e feche-o. Em seguida, volte ao terminal e execute o comando:

<pre>vagrant up</pre>

Após rodar este comando, uma nova máquina será criada e iniciada. Após o término da criação, utilize o comando abaixo, para fazer uma conexão SSH com a vm.

<pre>vagrant ssh</pre>

Dentro do ambiente de desenvolvimento, temos o diretório /vagrant, que será um espelho do conteúdo do diretório que criamos anteriormente, inclusive você verá os arquivos “Vagrantfile” no interior do mesmo.

## Configurando o ambiente Ruby On Rails

Inicialmente, vamos começar atualizando os pacotes de software da VM. Utilize o comando:

<pre>sudo apt-get update</pre>

Após vamos instalar as dependências necessárias para que os compiladores do rails funcionem. Execute o comando abaixo:

<pre>sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev -y</pre>

Após tudo instalado, estamos prontos para baixar o rbenv, um gerenciador de versões para o ruby.

<pre>
cd
git clone git://github.com/sstephenson/rbenv.git .rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
</pre>

Estes comandos irão baixar o rbenv, e configurar as váriaveis de ambiente necessárias.

## Instalando o Ruby

Após a instalação do rbenv, é bastante simples instalar versões do ruby, irei instalar a versão mais recente do ruby atualmente, a 2.2.3

<pre>rbenv install -v 2.2.3</pre>

Após fazer a instalação da versão é necessário ainda dizer ao rbenv qual a versão do ruby usada atualmente, com o subcomando global.

<pre>rbenv global 2.2.3</pre>

Deste modo, conseguimos fazer a instalação de várias versões do ruby, e depois alterar facilmente entre elas usando o global.
Após isso, podemos ainda rodar o comando abaixo para verificar a versão ativa do ruby.

<pre>ruby -v</pre>

Para finalizar, vamos definir que não queremos que o ruby baixe toda a documentação a cada gem instalada, visto que isto pode ser bastante demorada. Para isso vamos executar:

<pre>echo "gem: --no-document" > ~/.gemrc</pre>

Após instale o gerenciador de gems bundler, para controlar as dependencias dos aplicativos.

<pre>gem install bundler</pre>

## Instalando o Rails

Iremos finalmente instalar a gem Rails, para isto execute o comando abaixo (poderá ser adicionado o parâmetro -v para baixar uma versão em especifico)

<pre>gem install rails</pre>

Toda vez que for instalada uma nova versão do ruby ou de uma gem que forneça comandos, será necessário rodar o comando:

<pre>rbenv rehash</pre>

Ao final, verifique se ocorreu tudo corretamente executando o comando:

<pre>
rails -v

#Rails 4.2.4
</pre>

## Instalando o Javascript Runtime

Como no pipeline dos no rails, o mesmo faz uso de coffeescripts e miniaturaização automática de assets, é necessário ter instalado algum javascript runtime. Neste tutorial iremos instalar o node js.

Para isso primeiramente adicione o repositório dele através do comando:

<pre>sudo add-apt-repository ppa:chris-lea/node.js</pre>

E em seguida faça o update dos pacotes do apt-get e a instalação do nodejs.

<pre>
sudo apt-get update
sudo apt-get install nodejs
</pre>

## Instalação do Mysql

O Rails suporta vários tipos de bancos, irei instalar agora o mysql, que será usado nos tutoriais desta série. Caso sinta-se a vontade, você poderá instalar outros bancos. Execute o comando abaixo para instalar os pacotes referentes ao mysql.

<pre>sudo apt-get install mysql-server mysql-client libmysqlclient-dev</pre>

Após instale a gem mysql2, responsável por fornecer as funções do mysql nas aplicações ruby.

<pre>gem install mysql2</pre>

Com isso o ambiente está configurado. Vamos criar uma aplicação de testes para verificar se tudo está funcionando. Para isso execute o comando:

<pre>rails new testapp -d mysql</pre>

Isto irá criar um novo app utilizando o banco de dados mysql por padrão.
Acesse o diretório criado testapp
Devido a uma incompatibilidade da versão 4.2.4 do rails com a gem mysql2, devemos editar o arquivo Gemfile, para buscar uma versão mais antiga desta gem. Para isto localize a linha:


<pre>gem 'mysql2'</pre>

E substitua por:

<pre> gem 'mysql2', '~> 0.3.18'</pre>

Caso você tenha definido uma senha para o banco de dados, você deve informa-la no arquivo config/database.yml preenchendo o valor do campo password:

<pre>
  default: &default
  adapter: mysql2
  encoding: utf8
  pool: 5
  username: root
  password:
  socket: /var/run/mysqld/mysqld.sock
</pre>

Após, execute o comando abaixo para criar o banco de dados:

<pre>rake db:create</pre>

Para visualizar a aplicação no ar, acesse o endereço da máquina (que pode ser obtido através do comando ifconfig) na porta 3000, no meu caso é http://192.168.33.10:3000/.

No próximo post, veremos como criar nossa primeira aplicação rails, até lá.
 
