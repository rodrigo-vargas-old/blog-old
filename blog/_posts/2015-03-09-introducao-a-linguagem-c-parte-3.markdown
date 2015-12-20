---
layout: post
title: "Introdução a linguagem C (Parte 3)"
date: 2015-03-09 00:00
categories: c
---

Continuando nossa série sobre a linguagem C. Iremos ver neste post como funcionam as strings em C. Para ficar claro desde já, a linguagem C não possui o tipo string nativamente. O que entendemos por string, o compilador C entende por um array de char com um '\0' no final. Mas porque isso? No C, cada char pode ser representado por um número, como no código abaixo:

{% highlight c %}
void main (int argc, char** argv)
{
  char ola[11];
  ola[0] = 78;
  ola[1] = 111;
  ola[2] = 119;
  ola[3] = 32;
  ola[4] = 72;
  ola[5] = 105;
  ola[6] = 114;
  ola[7] = 105;
  ola[8] = 110;
  ola[9] = 103;
  ola[10] = 0;

  printf(ola);
}
{% endhighlight %}

Se executarmos este código, iremos ver que uma saída em char vai ocorrer. Cada número representa um caractere da tabela ASCII. E o 0 seria o caractere escolhido pelo compilador para a delimitação. Deste modo então sabemos que se uma string nada mais é que um array de char, para criarmos um array de strings precisamos de uma matriz. Aperfeiçoando então nosso projeto, vamos adicionar uma capacidade para strings de 25 caracteres nos nomes. Adicionaremos uma constante para o número de caracteres do nome, e modificaremos o array para uma matriz, deste modo:

{% highlight c %}
#include 

#define NUM_MAX_USERS 5
#define TAMANHO_NOME 25

char usuarios[NUM_MAX_USERS][TAMANHO_NOME];
{% endhighlight %}

Adicionei também a definição do header , que usaremos logo a seguir. Na nossa função add_users(), vamos modifica-la para ler strings. Na função scanf, iremos modificar o %c para %s, permitindo ler strings sem espaços, visto que o mesmo é um delimitador nesta função, contornaremos este problema mais a frente. Deste modo teremos.

{% highlight c %}
int add_users()
{
  char user[TAMANHO_NOME];
  int isError = 1;
  clear_scr();
  printf("Digite o nome do usuário com até %i caracteres.\n", TAMANHO_NOME);
  scanf ("%s", user);

  //Search for a empty position
  int x= 0;
  for (x = 0; x < NUM_MAX_USERS; x++)
  {
    if (usuarios[x][0] == 0)
    {
      strcpy(usuarios[x], user);
      isError = 0;
      break;
    }
  }

  return isError;
}
{% endhighlight %}

Note que realizamos algumas modificações para ler as strings. Como por exemplo, agora usamos o inteiro 0 para ser o nosso delimitador. Isto foi feito nas outras funções que acessam o array, no caso a "initializeArray()" e a show_users(). Outra modificação importante, foi o uso da função strcpy(), que permite a cópia de strings. Esta função facilita um pouco as coisas, pois sem ela teríamos que copiar a string caractere a caractere. Ela possui uma limitação no tamanho, mas por enquanto deve quebrar o galho. Abaixo como ficaram as funções modificadas

{% highlight c %}
void initializeArray()
{
  int x= 0;
  for (x = 0; x < NUM_MAX_USERS; x++)
    usuarios[x][0] = 0;
}

void show_users()
{
  int x = 0;
  for (x = 0; x < NUM_MAX_USERS; x++)
  {
    if (usuarios[x][0] != 0)
      printf("%s\n", usuarios[x]);
  }

  sleep(3); 
}
{% endhighlight %}

No próximo post, faremos a alocação dinamica de memória para as strings, possibiltando ter um número variável de usuários. Até lá.