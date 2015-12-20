---
layout: post
title:  "Introdução a linguagem C"
date:   2015-02-23 00:00
categories: c
---

Eae pessoal, tudo jóia? Hoje eu começo uma série de posts que visam repassar o básico sobre a linguagem de programação C. Para isso eu vou fazer um programinha que fiz na faculdade onde a idéia é fazer uma espécie de rede social. Onde temos nodos que representam pessoas. Basicamente um grafo. Então vamos começar?

Todo programa em C tem um metodo chamado "main". Este metódo é responsável por ser a porta de entrada do sistema. Sem este metódo o compilador não saberá por onde o seu programa começa. A função main tem a seguinte sintaxe:
{% highlight c %}
int main (int argc, char** argv)
{
   return 0;
}
{% endhighlight %}

Nesta função temos dois argumentos: argc que é a quantidade de argumentos passados, sendo que o mínimo sempre será um, no caso, o nome do executável. E o segundo argumento é um array com os argumentos propriamente ditos. Note também que temos um retorno int. Este retorno irá para o sistema operacional, que assumirá que tudo ocorreu como esperado se o valor for zero. Também podemos colocar a assinatura do método com retorno void, caso não queiramos retornar algo. Não iremos usar isto no momento, mas é interessante saber.

Vamos colocar então este trecho de código em um arquivo chamado main.c e após compila-lo usando o comando:

gcc -o Grafo main.c

Explicando rapidamente este comando, estamos chamando o compilador gcc, nativo no linux, passando os parâmetros "-o", para compilar, "Grafo" que será o nome do executável e por fim o nome do arquivo "main.c". Executando o arquivo gerado, não obtemos nenhum retorno, pois o mesmo retorna apenas 0, o que significa que tudo está OK para o sistema, que não exibe nada na tela. Então vamos passar a fazer uso agora do printf, que é uma função bastante utilizada para imprimir mensagem na tela.

##Imprimindo mensagens na tela e incluindo arquivos

Vamos exibir uma mensagem de boas vindas para os usuários. Vamos mudar nossa função main para:

{% highlight c %}
int main (int argc, char** argv)
{
   printf("Bem vindo a rede social.\n");
   return 0;
}
{% endhighlight %}

Ao tentar compilar o programa com o gcc, obtemos uma mensagem de erro. Isto acontece, porque printf é uma função que não está definida no nosso programa, e o compilador não conseguiu achar nenhuma referência a ela. Então para isto, vamos adicionar a referência ao arquivo stdio.h, que é o arquivo onde conterá a referência da função printf. Vamos adicionar então o seguinte comando, no início do arquivo main.c.

'#include'

Agora podemos compilar o nosso arquivo com sucesso. Note que o arquivo utilizado tem a extensão ".h". Isto acontece pois este arquivo não tem a implementação da função printf, e sim apenas o protótipo da declaração da função, veremos isso mais a frente. Partiremos agora para uma interação com o usuário, criando um menu de opções.

##Comando while, entrada de dados usando o teclado e usando a função scanf

Para criarmos o nosso menu, primeiramente faremos um laço de iteração, para que o nosso programa possa retornar para o menu várias vezes, até queremos que ele encerre. Para isto adicionaremos um while, com uma condição de saída. Eis como ficará a nossa função main.

{% highlight c %}
int main (int argc, char** argv)
{
  int option;
  option = 0;
  while (option != 9)
  {
    printf("\e[1;1H\e[2J");
    printf("Bem vindo a rede social.\n");
    printf("Digite uma opção do menu.\n");
    printf("1-Retornar ao menu.\n");
    printf("9-Sair.\n");

    scanf("%i", &option);
  }
  
   return 0;
}
{% endhighlight %}

Criamos uma váriavel para receber a opção do usuário. Dentro do while, imprimo alguns comandos para montar o menu, e abaixo uso a funçao scanf. Note que a mesma utiliza como argumento a sintaxe "&nome_da_variavel", isto acontece pois a função scanf, altera o valor da váriavel option. Para isto usamos o &option, porque passamos o endereço de memória da variável option, para que a função scanf possa modificar o seu valor. Se passassemos apenas option, a váriavel teria o seu contexto duplicado dentro da função scanf, impossibilitando a mudança do valor. 

Agora que temos o esboço do nosso menu vamos adicionar algumas funcionalidades a ele, e faze-lo reconhecer as opções digitadas. Mas isto vai ficar para o próximo post. Até lá.