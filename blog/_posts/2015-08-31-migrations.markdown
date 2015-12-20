---
layout: post
title:  "Migrations"
date:   2015-08-31 00:00
categories: ruby-on-rails
---

No post de hoje vou mostrar uma funcionalidade muito poderoso do Rails, as migrations. Migrations são como um controle de versão para o seu banco de dados. Normalmente, quando estamos criando um novo banco de dados, uma tabela, fizemos o mesmo criando um schema SQL ou algo assim. Rodamos na nossa máquina e a tabela está criada. 

Mas o que fazer quando estamos em um time, onde temos que manter o banco de dados consistente entre todos os desenvolvedores? Talvez possamos passar esse esquema para a equipe toda, é uma possibilidade, porém isto tende a ficar muito complexo com o crescimento da aplicação, assim como pequenas alterações na tabela, podem significar uma dor de cabeça para manter a sincronia entre as alterações, entre outros problemas. Com as migrations, todo este controle passa a ser feito pelo framework, assim focamos no que realmente importa. Acredite em mim, apesar de ser um conceito novo, ele é realmente muito bom.

## Estrutura de uma migration

No rails, uma migration tem a seguinte forma:

<pre>
class CreateArticles < ActiveRecord::Migration
  def change
    create_table :articles do |t|

      t.timestamps null: false
    end
  end
end
</pre>

Nesta migration estamos executando o comando “create table”, em seguida temos o nome da tabela “articles”. Dentro desta tabela temos apenas o timestamps, que vai indicar para o rails que serão geradas duas colunas para controle das alterações, a created_at e a updated_at, algo bastante interessante para manter o controle das edições. Apesar de não ser exibido, por padrão toda migration possui um campo id, que é sua chave primária.

## Gerando novas migrations

Mas ficar gerando classes para toda alteração no banco de dados é demorado. Por isso, o rails possui um excelente gerador de migrations out-of-box.
Para gerar uma nova migration, usamos o seguinte comando:


<pre>rails generate migration < nome_da_migration > < fields ></pre>

Como vimos acima, temos dois parâmetros importantes, o nome da migration e os fields. O gerador do rails, permite que sejam indicados os campos da migration, deixando a tarefa da geração ainda mais rápida. Como por exemplo:

<pre>rails generate migration CreateArticles title:string body:text</pre>

Este comando irá gerar a seguinte migration no diretório ‘db/migrate’, o nome dela será composto de um timestamp concatenado com o sufixo “_create_articles.rb”

<pre>
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.text :body
    end
  end
end
</pre>

Como podemos ver, foi gerada uma migration sem timestamps, e com os campos title e body. Legal não? Analisando ainda, vemos que nesta migration temos uma function chamada “change”. Isto indica ao rails, que quando for executada esta migration com o comando migrate, este método deve ser invocado, criando a tabela. A função change suporta por padrão várias modificações no banco. E quando for executado um rollback, o processo inverso da migrate, esta tabela deve ser destruída.

Mas e quando temos algo mais específico, como a adição de uma coluna? Sem problemas, o método change identificará isso e usará a forma reversa para destruir a coluna. 

## Migrate e rollback

Com a migration criada, vamos executar o comando migrate, como mostrado abaixo:

<pre>rake db:migrate</pre>

Ao executar isto, obteremos uma mensagem de sucesso. Mas vamos verificar como isto ficou no nosso banco de dados. No terminal, acesse o mysql com o comando:

<pre>mysql -u <usuario> -p</pre>

Digite sua senha. Após digite o comando abaixo para definir a database que vamos usar:

<pre>use blog_development;</pre>

Digite o comando abaixo para vermos as tabelas da nossa base de dados:

<pre>
mysql> show tables;
+----------------------------+
| Tables_in_blog_development |
+----------------------------+
| articles                   |
| schema_migrations          |
+----------------------------+
</pre>

Analisando a saída, verificamos a criação da nossa tabela articles assim como a existência de uma tabela chamada “schema_migrations”. Esta tabela é utilizada pelo rails, para manter o controle das migrations, normalmente, não precisaremos interagir com ela.

Continuando nossa análise, digite o comando abaixo para exibir a estrutura da tabela articles:

<pre>describe articles;</pre>

Isto irá gerar a seguinte saída:

<pre>
mysql> describe articles;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| title | varchar(255) | YES  |     | NULL    |                |
| body  | text         | YES  |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
</pre>

Analisando a tabela, podemos verificar que foi gerado um campo chamado id, que é um inteiro e chave primária. O campo title é um varchar, visto que indicamos que ele é uma string, e o campo body é do tipo text, como indicado na migration.

Bastante simples certo? E o que aconteceria caso fosse esquecido um campo ou ocorresse algum erro de digitação? Sem problemas, para isto temos o comando de “rollback”, vamos executa-lo desta forma:

<pre>rake db:rollback</pre>

E a tabela articles foi destrúida, vamos alterar nossa migration para:

<pre>
class CreateArticles < ActiveRecord::Migration
  def change
    create_table :articles do |t|
      t.string :title
      t.text :body
      t.datetime :published_at
    end
  end
end
</pre>

Após, execute novamente o comando:

<pre>rake db:migrate</pre>


O comando rollback irá destruir a tabela e todos os dados existentes. Então como faremos caso a aplicação já esteja em produção e seja necessário fazer uma alteração na mesma?
Deveremos criar uma nova migration. Como exemplo, vamos inserir uma coluna chamada “excerpt” na tabela articles. Criaremos a migration desta forma:


<Pre>rails generate migration AddExcerptToArticles excerpt:string</pre>

Note a convenção usada na migration. Add <nome_da_coluna> To <nome_da_tabela>. É interessante manter este padrão para as migrations, visto que o rails tem uma relação muito forte com suas convenções.

Continuando, o comando anterior irá gerar a seguinte migration:

<pre>
class AddExcerptToArticles < ActiveRecord::Migration
  def change
    add_column :articles, :excerpt, :string
  end
end
</pre>

Novamente, temos o método change fazendo todo o trabalho de geração para nós. Vamos agora executa-la:

<pre>rake db:migrate</pre>

Conferindo novamenta a table articles, teremos:

<pre>
mysql> describe articles;
+--------------+--------------+------+-----+---------+----------------+
| Field        | Type         | Null | Key | Default | Extra          |
+--------------+--------------+------+-----+---------+----------------+
| id           | int(11)      | NO   | PRI | NULL    | auto_increment |
| title        | varchar(255) | YES  |     | NULL    |                |
| body         | text         | YES  |     | NULL    |                |
| published_at | datetime     | YES  |     | NULL    |                |
| excerpt      | varchar(255) | YES  |     | NULL    |                |
+--------------+--------------+------+-----+---------+----------------+
</pre>

Algo realmente legal. Com as migrations temos muita agilidade e organização na construção do nosso banco de dados. Caso tiveremos cometido algum erro na criação da migration, podemos voltar usando o comando rollback. Neste caso, apenas a última migration executada será revertida.

Por hoje era isso pessoal, no próximo post veremos como acessar os dados do banco através do ActiveRecord do rails, até lá.
