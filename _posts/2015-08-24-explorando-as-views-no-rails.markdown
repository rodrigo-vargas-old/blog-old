---
layout: post
title:  "Explorando as views no Rails"
date:   2015-08-24 00:00
categories: ruby-on-rails
---

Nest post, iremos ver como usar as views no rails, como organiza-las, passar variáveis para as mesmas, entre outras funcionalidades.

Abrindo a view home.html.erb, dentro de pages e analisando o seu código no browser, vimos que é gerado toda a estrutura html, inclusive com javascripts e arquivos css. Esta estrutura toda é definida por padrão em ‘/app/views/layouts/application.html.erb’. Este arquivo é gerado automaticamente pelo rails, e é o arquivo de layout de todas as páginas que não tiverem algum definido.

O comando <%= yield => é o responsável por definir a área aonde os trechos de código das views serão impressos.

<pre>
&lt;body&gt;

<%= yield %>

&lt;/body&gt;
</pre>

Aqui vemos, que todo o conteúdo será englobado pela tag <body>. Caso quisessemos, poderiamos colocar uma div em volta do yield, e isso valeria para todas as páginas que tivessem este layout como layout padrão, poupando assim o trabalho de ter que copiar esta div em cada uma das views.

## Passando váriaveis para as views 

Passar variáveis para as views no rails, é bastante simples. Vamos abrir o arquivo ‘/app/controllers/pages_controller.rb’. Na action contact, vamos definir uma váriavel chamada name desta maneira:

<pre>@name = ‘Rodrigo’</pre>

E na view, vamos referenciar está variavel da seguinte forma:

<pre>
&lt;h1&gt;Contate o &lt;%= @name %>&lt;/h1&gt;
</pre>

Desta forma o resultado no browser será:

<pre>Contate o Rodrigo</pre>

Bastante simples não? Ainda poderiamos passar um array de elementos, desta maneira no controller:

<pre>
@array = ['Rodrigo', 'Vargas']
</pre>


E desta maneira na view:


<pre>
&lt;% @array.each do |element| %&gt;
    &lt;%= element %&gt;
  &lt;% end %&gt;
</pre>


Repare que toda vez que queremos que a linha em questão imprima algo, usamos a sintaxe <%= . Caso seja apenas uma expressão sem retorno, utilizamos o <%  na frente.



Ainda existem outras expressões interessantes para usar na view, como por exemplo, o if. Que pode ser bastante útil, quando não queremos testar se um conjunto de elementos é vazio. Por exemplo, se temos uma lista de cores:


<pre>
@skills = ['Rails', 'Css', 'MySql']
</pre>


E na view:


<pre>
&lt;h2&gt;Minhas habilidades&lt;/h2&gt;
&tl;ul&gt;
  &tl;% @skills.each do |skill| %&gt;
    &tl;li&gt;&tl;%= skill %&gt;&tl;/li&gt;
  &tl;% end %&gt;
&lt;/ul&gt;
</pre>

Podemos imprimir isto na página, e teremos uma lista. Mas caso esta lista fosse vazia, como por exemplo:

<pre>
@skills = []
</pre>

Teriamos um elemento ul vazio no nosso html, além do título. Então poderiamos aplicar o if para melhorar a nossa visualização, ficando:

<pre>
&lt;% if @skills.count > 0 %&gt;
  &lt;h2&gt;Minhas habilidades&lt;/h2&gt;
  &lt;ul&gt;
    &lt;% @skills.each do |skill| %&gt;
      &lt;li&gt;&lt;%= skill %&gt;&lt;/li&gt;
    &lt;% end %&gt;
  &lt;/ul&gt;
<% end %>
</pre>

Desta maneira, caso não existam elementos na váriavel @skills, nada será impresso.

## Nomeando as yields

Outro recurso interessante nas views, é a possibilidade de termos várias delas na nossa página, assim podemos ter um yield para o conteúdo e outro para uma seção de javascripts na página, vamos ver um exemplo. Voltando ao nosso arquivo ‘/app/views/layouts/application.html.erb’, temos no body:

<pre>
&lt;body&gt;

&lt;%= yield %&gt;

&lt;/body&gt;
</pre>

Vamos modificar este trecho para:

<pre>
&lt;body&gt;
  &lt;div class="content"&gt;
    &lt;%= yield :content %&gt;
  &lt;/div&gt;
  &lt;%= yield :scripts_footer %&gt;
&lt;/body&gt;
</pre>

E na nossa view, vamos criar um script na seção scripts_footer e mudar o conteúdo para ser exibido em sua respectiva seção.

<pre>
&lt;% content_for :content do %&gt;
  &lt;h1&gt;Contate o 
    &lt;% @array.each do |element| %&gt;
      &lt;%= element %&gt;
    &lt;% end %&gt;
  &lt;/h1&gt;

  &lt;% if @skills.count &gt; 0 %&gt;
    &lt;h2&gt;Minhas habilidades&lt;/h2&gt;
    &lt;ul&gt;
      &lt;% @skills.each do |skill| %&gt;
        &lt;li&gt;&lt;%= skill %&gt;&lt;/li&gt;
      &lt;% end %&gt;
    &lt;/ul&gt;
  &lt;% end %&gt;
&lt;% end %&gt;

&lt;% content_for :scripts_footer do %&gt;
  &lt;script type="text/javascript"&gt;
    alert('Foo bar');
  &lt;/script&gt;
&lt;% end %&gt;
</pre>

Desta maneira, será gerado o seguinte HTML (apenas mostrando a parte importante):

<pre>
&lt;body&gt;
  &lt;div class="content"&gt;
      &lt;h1&gt;Contate o 
      Rodrigo
      Vargas
  &lt;/h1&gt;

  &lt;/div&gt;
    &lt;script type="text/javascript"&gt;
    alert('Foo bar');
  &lt;/script&gt;
&lt;/body&gt;
</pre>

É um recurso bastante poderoso, que permite uma boa organização dos elementos na página. 

## Definindo diferentes layouts

Muitas vezes, nos deparamos com situações, onde temos layouts diferentes para grupos de páginas diferentes. Uma visualização em modo admin, uma página com um cabeçalho diferente, enfim, são várias as situações e certamente você já passou ou irá passar por uma delas. O rails permite esta flexibidade, podemos definir um layout para um controller específico, ou até mesmo, para uma view em específico. Existem outras opções, mas iremos focar nestas duas neste post.

Para definir uma view para um controller, use o comando layout no início do controller, como no exemplo abaixo, onde definimos o layout “pages” para o pages controller:


<pre>class PagesController &lt; ApplicationController
  layout "pages"

  def home
  end

  def contact
    @name = 'Rodrigo'
    @array = ['Rodrigo', 'Vargas']
    @skills = []
    #@skills = ['Rails', 'Css', 'MySql']
  end
end
</pre>

E claro, vamos criar um arquivo chamado ‘pages.html.erb’ em ‘/app/views/layouts’, que é onde é definido os layouts do rails. Insira algum conteúdo no page layout e verifique a diferença entre os layouts.

Caso queira aplicar este layout apenas para uma view em específico, podemos utilizar a sintaxe abaixo no comando ‘layout’:

<pre>
layout "pages", :only => :contact
</pre>

Desta forma, estamos fazendo o override do layout “application” padrão para a action ‘contact’. Só tenha em mente, que isto valerá para todos as actions, então devemos definir o layout para as outras, caso existam.

<pre>
layout "pages", :only => :contact
layout "application", :except => :home
</pre>

Bastante simples certo? Por hoje era isso, até o próximo post da série.
