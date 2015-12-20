---
layout: post
title:  "Active Record"
date:   2015-09-07 00:00
categories: ruby-on-rails
---

Continuando a série de fundamentos do rails, vamos nest post entender como funciona o ActiveRecord do rails, responsável pelo acesso ao banco de dados, assim como a criação de models, que representam a estrutura do banco.

## Geração de models

Assim como nos controllers, o rails nos fornece um excelente gerador para os models. Vamos criar um model para a nossa tabela articles, com o seguinte comando:

<pre>rails generate model article</pre>

Note a convenção no comando, de usar o nome da tabela na sua forma singular. De qualquer maneira, ao executar este comando será exibido um erro:

<pre>conflict    db/migrate/20150919170443_create_articles.rb
Another migration is already named create_articles: /vagrant/blog/db/migrate/20150919161211_create_articles.rb. Use --force to replace this migration or --skip to ignore conflicted file.
</pre>

Isto ocorre, porque ao criar um novo model, o rails já cria a sua migration na pasta db/migrates, e como no último post, fizemos esta etapa manualmente para aprendizado, houve um conflito. Desta vez, usaremos o comando abaixo para contornar este erro.

<pre>rails generate model article --skip</pre>

Ao criar o model article, são criados o model em si na pasta ‘/app/models/’ assim como arquivos para testes. Além disso é gerada uma migration com o nome do model no plural na pasta ‘db/migration’. Por isso a importancia, da conveção de nomes no rails.

## Métodos do ActiveRecord

O active record fornece várias métodos interessantes para as mais diversas operações no banco de dados. Vamos usar algumas delas. Primeiramente, vamos acessar o console do rails, que é uma ferramenta bastante interessante, permitindo testar comandos rapidamente. Para isso digite:

<pre>rails console</pre>

No console do rails, digite:

<pre>@article = Article.new</pre>

Este comando irá criar um novo artigo. Note que podemos escrever esta linha nos nossos controllers. Vamos imprimr no console as informações deste artigo digitando:

<pre>@article</pre>

Teremos a seguinte saída:</p<

<pre>=> #<Article id: nil, title: nil, body: nil, published_at: nil, excerpt: nil></pre>

Indicando que temos um objeto do tipo Article, que não possui nenhuma propriedade preenchida, vamos preencher alguns dados de teste.


<pre>
@article.title = 'Titulo'
@article.body = "Corpo"
@article.published_at = Date.current()
@article.excerpt = 'Resumo'
</pre>

Apenas alguns dados de teste. Em published_at, usei a biblioteca Date para pegar a data atual. Para salvar o artigo no banco digite o comando

<pre>@article.save</pre>

E pronto, temos um novo registro no nosso banco de dados. Para acessa-lo vamos buscar todos os registros da tabela articles, com o comando:

<pre>
Article.all

 Article Load (0.3ms)  SELECT `articles`.* FROM `articles`
=> #<ActiveRecord::Relation [#<Article id: 1, title: "Titulo", body: "Corpo", published_at: "2015-09-19 00:00:00", excerpt: "Resumo">]>
</pre>

Como podemos ver, o comando foi traduzido em uma SQL query. e nos traz apenas um artigo, o que acabamos de criar. Vamos criar mais um registro:

<pre>
@article2 = Article.new
@article2.title = 'Titulo'
@article2.body = "Corpo"
@article2.published_at = Date.current()
@article2.excerpt = 'Resumo'
@article2.save
</pre>

E novamente o comando Article.all para exibir todos os artigos:

<pre>
irb(main):027:0> Article.all
  Article Load (0.2ms)  SELECT `articles`.* FROM `articles`
=> #<ActiveRecord::Relation [#<Article id: 1, title: "Titulo", body: "Corpo", published_at: "2015-09-19 00:00:00", excerpt: "Resumo">, #<Article id: 2, title: "Titulo", body: "Corpo", published_at: "2015-09-19 00:00:00", excerpt: "Resumo">]>
</pre>

E como podemos ver, agora temos 2 registros na tabela articles. Outra operação bastante utilziada é a busca por determinado registro, que pode ser feita pelo metódo “find”, da seguinte maneira:


<pre>@oldArticle = Article.find(1)</pre>

No método find utilizamos o id do registro como parâmetro, agora podemos imprimir as propriedades do registro ‘oldArticle’, como o seu titulo por exemplo:

<pre>
@oldArticle.title
=> "Titulo"
</pre>

O rails também permite buscar os registros utilizando outros campos de uma maneira bastante simples. Se quisermos buscar pelo campo ‘title’, utilizamos o comando ‘find_by_title’:

<pre>
@anotherArticle = Article.find_by_title('Titulo')

  Article Load (0.3ms)  SELECT  `articles`.* FROM `articles` WHERE `articles`.`title` = 'Titulo' LIMIT 1
=> #<Article id: 1, title: "Titulo", body: "Corpo", published_at: "2015-09-19 00:00:00", excerpt: "Resumo">
</pre>

E como podemos ver, o artigo mais antigo que possui o campo title com o valor informado é retornado. Poderiamos usar outros campos como o published_at, ficando o método:


<pre>find_by_published_at</pre>

Ainda para finalizar, podemos fazer o update destes artigos, através do método update;

<pre>
@anotherArticle.update(title: 'Novo Titulo')
</pre>

Como podemos ver o Active Record é uma ferramenta muita poderosa que agiliza o acesso ao banco de dados. Operações de CRUD se tornam realmente triviais no código, o que acaba gerando códigos mais limpos e ganho de tempo com operações repetitivas. No próximo post, iremos ver o active record na prática, dentro dos controllers, até lá.