---
layout: post
title:  "Introdução sobre rotas, views e controllers no rails"
date:   2015-08-18 00:00
categories: ruby-on-rails
---
No último post, vimos todos os passos para criar um ambiente de desenvolvimento para rails. Prosseguindo a série, neste post iremos iniciar a nossa aplicação Rails. Irei mostrar conceitos básicos sobre routes, controllers e views, que são elementos básicos de aplicações baseadas na arquitetura MVC.


Inicialmente vamos criar a nossa aplicação rails através, até o final da série eu tenho o objetivo de criar um blog, com comentários e posts. 

## Criação da aplicação e configurações iniciais

Para criar uma nova aplicação rails, use o comando:

<pre>rails new blog -d mysql</pre>

Onde “blog” é o nome do aplicativo. Neste caso eu utilizei o comando -d mysql, que especifica que quero trabalhar com a base de dados mysql.

Após a criação, vamos acessar o diretório gerado, chamado “blog”:

<pre>cd blog</pre>

Ao iniciar o servidor usando o comando:

<pre>rails s -b 0.0.0.0</pre>

E acessar o browser, encontramos o seguinte erro:

<pre>Specified 'mysql2' for database adapter, but the gem is not loaded. Add `gem 'mysql2'` to your Gemfile (and ensure its version is at the minimum required by ActiveRecord).</pre>

Como citei no post passado, isto acontece, devido a uma incompatibilidade entre o rails 4.2.4 e a gem mysql2 mais atual. Vamos solucionar isto e já conhecer o arquivo Gemfile. Acesse o arquivo Gemfile que se encontra na raiz do projeto.

O Gemfile é o arquivo responsável por gerenciar todas as gems utilizadas no projeto. Essas gems são bibliotecas utilizadas pelo ruby, que fornecem funcionalidades extras a linguagem. Não entrarei em detalhes visto que não é o foco do post, talvez mais para frente. Para solucionar o problema da gem mysql2, localize a linha onde a mesma é declarada.

<pre>gem ‘mysql2’</pre>

E substitu-a por uma versão menor, no caso a ‘0.3.8’, desta forma:

<pre>gem 'mysql2', '0.3.18'</pre>

Após, rode o comando para atualizar as gems:

<pre>bundle install</pre>

Após terminado, vamos iniciar o servidor do rails novamente com o comando:

<pre>rails s -b 0.0.0.0</pre>

Se você configurou uma senha para o banco de dados, teremos um novo erro no browser.

<pre>Access denied for user 'root'@'localhost' (using password: NO)</pre>

Este é bastante intuitivo, falta a senha para o usuário root. Vamos configurar isto acessando o arquivo config/database.yml. Neste arquivo, temos as informações necessárias para acessar os 3 ambientes que o rails gera por padrão para seus projetos: development, test e production.



Em default, temos as configurações globais, que valem para todos os ambientes. Vamos colocar nela, na linha password a senha do banco de dados. Reinicie o servidor para que as aplicações tenham efeito.


Ao acessar o browser novamente, mais um erro: 

<pre>Unknown database 'blog_development'</pre>

Ocorre porque esta database ainda não existe. Vamos cria-la digitando o seguinte comando no nosso console.


<pre>rake db:create</pre>

Iniciando nosso servidor, e acessando o browser, vimos a página de boas vindas do rails.

## Configurando as rotas no rails

As rotas no rails, são definidas no arquivo config/routes.rb. Vamos acessa-lo. Por padrão, temos um arquivo com inúmeros exemplos, de como definir rotas no rails, podemos apagar todas estes comentários deixando o arquivo com apenas:


<pre>
Rails.application.routes.draw do
  
end
</pre>

A primeira coisa que costumamos fazer nos nossos projetos é definir uma rota chamada ‘root’. Que servirá para indicar para o rails, qual será nossa página inicial. Toda rota é composta de um controller e uma action. O controller será o responsável por dividir as actions da aplicação, como por exemplo, um controller chamado PagesController terá actions referentes a Pages, ou páginas, em portugues. Dentro deste controller, podemos ter uma action chamada home, que mostrará a view da home do site. 


Vamos então apontar nossa rota ‘root’ para um controller chamado PagesController e para uma action chamada ‘home’. Adicione a linha abaixo ao arquivo routes.rb.

<pre>root :to => ‘pages#home’</pre>

Como podemos ver a sintaxe é ‘<nome_do_controller>#<nome_da_action>’. No caso, não precisamos escrever a palavra controller na rota, apenas o nome da mesma.

Ao criar a rota, precisaremos reiniciar o servidor do rails. Após, acesse o browser, teremos o seguinte erro:

<pre>uninitialized constant PagesController</pre>

Um erro esperado, visto que não criamos o controller Pages, vamos fazer isso agora. Acesse o terminal e digite o comando:

<pre>rails generate controller pages</pre>

Analisando a saída do console temos:

<pre>
      create  app/controllers/pages_controller.rb
      invoke  erb
      create    app/views/pages
      invoke  test_unit
      create    test/controllers/pages_controller_test.rb
      invoke  helper
      create    app/helpers/pages_helper.rb
      invoke    test_unit
      invoke  assets
      invoke    coffee
      create      app/assets/javascripts/pages.coffee
      invoke    scss
      create      app/assets/stylesheets/pages.scss
</pre>

Por padrão o gerador do rails, criou o nosso controller na pasta app/controllers. Ainda, temos a criação da pasta padrão para as views deste controller, em app/views/pages. Ainda foram criados testes unitários, helpers e as pastas para os assets. Algo que agiliza muito o desenvolvimento das aplicações.


Vamos acessar agora o nosso controller para criar a action home. Para isto vamos definir a action home deste modo:

<pre>
def home
end
</pre>

Apenas isso? Sim. Caso não haja nenhum redirect ou algo que chame outra action, o rails irá invocar a view correspondente. Como podemos ver no browser.

<pre>Missing template pages/home, application/home with {:locale=>[:en], :formats=>[:html], :variants=>[], :handlers=>[:erb, :builder, :raw, :ruby, :coffee, :jbuilder]}. Searched in: * "/vagrant/blog/app/views"
</pre>

O erro diz que nã foi possível achar o template pages/home e nem o template application/home. Este segundo foi chamado, pois todo controller por padrão, herda do controller application, algo bastante útil, quando queremos definir actions globais.

<p:Vamos criar nossa view. Crie um arquivo chamado home.html.erb na pasta app/views/pagese coloque o seguinte conteúdo.

<pre>Hello World!</pre>

Atualizando a página podemos ver nossa mensagem. Bastante fácil não? Mais um exemplo, vamos criar agora uma página contato. Primeiramente, vamos definir sua rota no arquivo routes.rb.
Desta vez iremos fazer um pouco diferente, visto que a página contato não é a nossa raiz, ela ficará em ‘/contato’. Para isto vamos defini-la deste modo:

<pre>get '/contato' => 'pages#contact' </pre>

Escrevemos primeiramente o tipo da requisição (‘get’), a rota, e após o controller e a action. Após definir a rota, vamos adicionar uma nova action no controller pages.

<pre>
def contact
end
</pre>

E por fim a criação da view ‘contact’ em ‘app/views/pages/contact.html.erb’, com o conteúdo abaixo:

<pre>
&lt;h1&gt;Contate me!&lt;/h1&gt;
</pre>

Após, vamos ver isto no browser no endereço /contato.

Por hoje era isso pessoal, para quem quiser ver o projeto do blog o mesmo se encontra no meu github, acesse-la e divirta-se. No próximo post continuaremos a explorar o básico do rails.
