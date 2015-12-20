---
layout: post
title:  "Fluxo de funcionamento no Rails"
date:   2015-09-14 00:00
categories: ruby-on-rails
---

Continuando a criação do blog e do aprendizado das funcionalidades básicas do rails, hoje iremos ver como funciona o fluxo MVC no rails. 

## Rotas e Controllers

Vamos começar criando a listagem dos nossos artigos. Para isso vamos acessar o arquivos routes.rb e definir uma rota para o índice de artigos, desta maneira:

<pre>get 'articles' => 'articles#index'</pre>

Isto indica que toda requisição GET para a url articles, será interceptada e dirigida para a action index que fica no articles controller. Como o controller articles ainda não existe, devemos fazer a criação do mesmo, através do comando:

<pre>rails generate controller articles</pre>

Com o controller gerado, podemos acessa-lo no caminho ‘/app/controllers/articles_controller.rb’. Vamos definir um método index da seguinte forma:

<pre>
  def index
    render :text => 'Todos os artigos'
  end
</pre>

Apenas como exemplo, este método irá imprimir na tela a string ‘Todos os artigos’. Inicie o servidor rails e verifique no browser em ‘/articles’ a sua saída.

Se tudo estiver correto você verá uma página em branco com a mensagem ‘Todos os artigos’. Isto indica que a configuração da rota está correta. 

## Criando a página de posts

Prosseguindo, agora podemos fazer o fetch de todos os artigos como vimos no post anterior sobre Active Record, vamos então buscar todos os artigos utilizando o método all do Active Record.

<pre>@articles = Article.all</pre>

Ainda vamos mudar o nosso método render para imprimir os registros em um formato JSON, algo que pode ser utilizado por web apps que fornecem dados para consumo por webservice.

<pre>
  def index
    @articles = Article.all
    render :json => @articles
  end
</pre>

Se acessarmos agora no browser o ‘/articles’ teremos todos os artigos impressos em formato JSON.

<pre>
[{"id":1,"title":"Novo Titulo","body":"Corpo","published_at":"2015-09-19T00:00:00.000Z","excerpt":"Resumo"},{"id":2,"title":"Titulo","body":"Corpo","published_at":"2015-09-19T00:00:00.000Z","excerpt":"Resumo"}]
</pre>

Agora que vimos que está tudo ok com o nosso controller e model, podemos criar a view para a página de índice dos artigos. Mas antes remove a linha que faz o render em json, pois com este render, não será carregada a view para index. Ficando o método index deste modo:

<pre>
  def index
    @articles = Article.all
  end
</pre>

Crie uma nova view em ‘/app/views/articles/index.html.erb’.
Recapitulando, em nosso arquivo de layout(‘/app/layouts/application.html.erb’), temos o seguinte formato:

<pre>

&lt;body&gt;
  &lt;div class="content"&gt;
    &lt;%= yield :content %&gt;
  &lt;/div&gt;
  &lt;%= yield :scripts_footer %&gt;
&lt;/body&gt;

</pre>

Então na nossa view, vamos colocar o conteúdo dentro do yield content, ficando a view desta forma:

<pre>
&lt;% content_for :content do %&gt;
  &lt;h1&gt;Artigos&lt;/h1&gt;
&lt;% end %&gt;
</pre>

Acessando agora o browser, devemos ter em ‘/articles’ uma página em branco com apenas um título h1 “Artigos”. Continuando, vamos agora imprimir a lista de artigos, iterando sobre os articles salvos na váriavel @articles, vinda do controller, com o método “each”:

<pre>
&lt;% content_for :content do %&gt;
  &lt;h1&gt;Artigos&lt;/h1&gt;

  &lt;% @articles.each do |article| %&gt;
    &lt;article&gt;
      &lt;h2&gt;&lt;%= article.title %&gt;&lt;/h2&gt;
      &lt;div class="body"&gt;&lt;%= article.body %&gt;&lt;/div&gt;
    &lt;/article&gt;
  &lt;% end %&gt;  
&lt;% end %&gt;
</pre>

Dentro de &lt;article&gt;, fizemos a impressão do valor do campo title e do campo body, de cada um dos artigos.

Acessando o browser, veremos uma listagem com dois artigos, que foram criados no post passado através do console do rails.
Agora que vimos como imprimir todos os artigos, vamos criar uma página que representará cada post do nosso blog.

## Exibindo itens, o método show

Assim como na funcionalidade de exibição de todos os artigos, para criar a funcionalidade de exibição de um único artigo iniciaremos pela criaçaõ da rota. Então, acesse novamente o arquivo routes.rb e adicione uma nova rota.

<pre>get 'articles/:id' => 'articles#show'</pre>

Na rota acima, estamos fazendo o uso de um coringa, o ‘:id’. Então se quando o endereço for ‘articles/1’, o rails identificará que o ‘1’ representa o id e passará essa informação para o controller. Devemos tomar cuidado pois se o endeço for algo como ‘articles/teste’, teste será o id. Se o controller estiver esperando um inteiro, isto pode ser um problema. Então, tenha em mente isto quando for criar um novo controller.


No nosso caso, poderiamos usar o título em formato slug para o post, mas como ainda não temos um slug definido, vamos utilizar o id. Prosseguindo, vamos fazer a busca deste post no controller articles. Acesse o articles_controller.rb e defina um novo método chamado show:

<pre>
  def show
    render :json => params[:id]
  end 
</pre>

Explicando o trecho acima, para buscar o parâmetro passado pela nossa rota, utilizaremos o mesmo nome definido lá como índice na váriavel params[] para buscar o seu valor. Utilizei o render :json para que possamos ver no browser isto em funcionamento. Acesse o endereço ‘/articles/foo’ e você verá a string ‘foo’ na página. Acesse o endereço ‘/articles/1’ e você verá um ‘1’ na página, exatamente o valor passado na url. Note que não temos nenhuma restrição quanto ao tipo da variável.


Entendido isto, agora temos a possibilidade de buscar o artigo no nosso banco de dados, vamos utilizar então o params[:id] como parametro para o método find do Active Record, e vamos imprimir este artigo em formato JSON para verificar que tudo está ok. Abaixo o método show atualizado

<pre>
  def show
    @article = Article.find(params[:id])
    render :json => @article
  end 
</pre>

Se tudo estiver ok, acessando a url ‘/articles/1’ você verá o seguinte objeto JSON:

<pre>
{"id":1,"title":"Novo Titulo","body":"Corpo","published_at":"2015-09-19T00:00:00.000Z","excerpt":"Resumo"}
</pre>

Legal. Agora temos todas as informações do nosso artigo. Mas o que acontece quando não buscarmos um artigo que não existe no banco como por exemplo o 3? Vamos ver o que acontece acessando a url ‘/articles/3’. A seguinte mensagem de erro é exibida:

<pre>Couldn't find Article with 'id'=3</pre>

No ambiente produtivo, isto será substítuida por uma mensagem de erro com o status 404 e com menos detalhes. Algo que o rails já gerencia para nós. Muito bom. Prosseguindo, remova a linha abaixo do método show:

<pre>render :json => @article</pre>

E vamos criar a view show, que exibirá as informações do artigo. Crie um novo arquivo em ‘/app/views/articles/show.html.erb’. Assim como a view show, vamos colocar o html referente a exibição do post, referenciando o yield content, deste modo:

<pre>
&lt;% content_for :content do %&gt;
  &lt;h1&gt;&lt;%= @article.title %&gt;&lt;/h1&gt;
  &lt;article&gt;
    &lt;%= @article.body %&gt;
  &lt;/article&gt;
&lt;% end %&gt; 
</pre>

Agora se acessarmos ‘/articles/1’ veremos uma bonita view do nosso post. Ainda falta uma maneira de acessar os nossos artigos sem precisar saber a url diretamente. Vamos voltar então em nossa view show e definir os links para as suas páginas detalhe.

Poderíamos fazer a seguinte modificação na view ‘index.html.erb’

<pre>
&lt;% content_for :content do %&gt;
  &lt;h1&gt;Artigos&lt;/h1&gt;

  &lt;hr /&gt;

  &lt;% @articles.each do |article| %&gt;
    &lt;article&gt;
      &lt;a href="/articles/&lt;%= article.id %&gt;"&gt;
        &lt;h2&gt;&lt;%= article.title %&gt;&lt;/h2&gt;
      &lt;/a&gt;
      &lt;div class="body"&gt;&lt;%= article.body %&gt;&lt;/div&gt;
    &lt;/article&gt;
  &lt;% end %&gt;  
&lt;% end %&gt;
</pre>

Como podemos ver, foi adicionado uma tag a, em volta do h2 com o seguinte href:

<pre>href="/articles/<%= article.id %>"</pre>


Isto irá funcionar, pois faremos a impressão do id correspondente, o que coincide com nossa rota. Mas pensando em um pouco, e se mudarmos a nossa rota para ‘/artigos’, ou talvez /’article’, todos os nossos links quebrariam. Felizmente, temos um excelente helper no rails, chamado link_to. Para utiliza-lo vamos usar a seguinte forma:


<pre>

&lt;% content_for :content do %&gt;
  &lt;h1&gt;Artigos&lt;/h1&gt;

  &lt;hr /&gt;

  &lt;% @articles.each do |article| %&gt;
    &lt;article&gt;
      &lt;%= link_to raw('&lt;h2&gt;' + article.title + '&lt;/h2&gt;'), controller: 'articles', action: 'show', id: article.id %&gt;
      &lt;div class="body"&gt;&lt;%= article.body %&gt;&lt;/div&gt;
    &lt;/article&gt;
  &lt;% end %&gt;  
&lt;% end %&gt;

</pre>

No primeiro parametro, utilizamos o método raw e dentro do mesmo concatenamos html com o nome do artigo. Após passamos o controller a action e o parâmetro id. Desta forma, poderemos alterar o formato da url, sem precisar nos preocupar com links quebrados.

Ainda, podemos definir esta url de um jeito mais simples. Podemos nomear nossa rota e utiliza-la aqui. Da seguinte forma, acesse o arquivo routes.rb e substitua a rota:

<pre>
get 'articles/:id' => 'articles#show'
</pre>

Por:
<pre>
  get 'articles/:id' => 'articles#show', :as => :article
</pre>

Desta forma, podemos referenciar esta rota com um método chamado article_path. Vamos fazer isto alterando a view index.html.erb para:

<pre>
&lt;% content_for :content do %&gt;
  &lt;h1&gt;Artigos&lt;/h1&gt;

  &lt;hr /&gt;

  &lt;% @articles.each do |article| %&gt;
    &lt;article&gt;
      &lt;%= link_to raw('&lt;h2&gt;' + article.title + '&lt;/h2&gt;'), article_path(article) %&gt;
      &lt;div class="body"&gt;&lt;%= article.body %&gt;&lt;/div&gt;
    &lt;/article&gt;
  &lt;% end %&gt;  
&lt;% end %&gt;
</pre>

Dentro de article_path passamos o article. O Rails irá entender que o parâmetro será a chava primária do model. Então se quisessemos passar outro parametro que senão a chave primária, podemos utilizar ‘article.propriedade’ Sendo propriedade um campo qualquer dentro do registro. Bastante simples certo?

Por hoje era isso pessoal, até o próximo post.