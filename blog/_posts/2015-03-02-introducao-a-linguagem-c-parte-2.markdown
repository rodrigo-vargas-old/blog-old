---
layout: post
title: "Introdução a linguagem C (Parte 2)"
date: 2015-03-02 00:00
categories: c
---

## Criando métodos no C e utilizando o switch case

Para iniciarmos, vamos adicionar uma nova entrada no nosso menu, chamada exibir créditos do criador. O programa irá reconhecer se a opção escolhida for 8, chamaremos o método "show_credits", que exibirá algumas informações a respeito do criador do programa. Para fazer o reconhecimento da opção utilzada iremos utilizar o comando switch, que funciona da mesma maneira que uma série de ifs (testes condicionais) encadeados, porém é mais legível. O código ficará:

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
    printf("8-Exibir créditos.\n");
    printf("9-Sair.\n");

    scanf(" %i", &option);

    switch(option)
    {
      case 8:
      {
        show_credits();
      }
      default:{}
    }
  }
  
   return 0;
}
{% endhighlight %}

Vamos criar agora, o método show_credits(). Para deixar um pouco mais legível o código, vamos também criar uma função para "printf("\e[1;1H\e[2J");", que é o nosso comando para limpar a tela, assim temos dois novos métodos. O código ficará da seguinte maneira.

{% highlight c %}
#include 
#include 

void clear_scr()
{
  printf("\e[1;1H\e[2J");
}


void show_credits()
{
  clear_scr();
  printf("Criado por: Rodrigo Vargas\n");
  printf("Visite www.rodrigovg.com.br \n");
  sleep(3); 
}

int main (int argc, char** argv)
{
  int option;
  option = 0;
  while (option != 9)
  {
    clear_scr();
    printf("Bem vindo a rede social.\n");
    printf("Digite uma opção do menu.\n");
    printf("1-Retornar ao menu.\n");
    printf("8-Exibir créditos.\n");
    printf("9-Sair.\n");

    scanf(" %i", &option);

    switch(option)
    {
      case 8:
      {
        show_credits();
      }
      default:{}
    }
  }
  
   return 0;
}
{% endhighlight %}

## Trabalhando com arrays no C

Partimos agora para criar o cadastro de usuários na nossa rede social. Para isso, inicialmente faremos algo simples, um array de char e estático. Mais a frente increntamos ele para algo mais interessante. Definiremos este array como uma váriavel global para que possamos acessar de diferentes pontos do nosso código. Declare-o abaixo das inclusões de bibliteca, deste modo:

{% highlight c %}
#include 
#include 

#define NUM_MAX_USERS 5

char usuarios[NUM_MAX_USERS];
{% endhighlight %}

É uma boa prática criar um valor fixo para o número de usuários, visto que vamos precisar saber o tamanho do array em outros pontos do código. Agora iremos criar trêsnovas funções, uma para incluir usuários (addUser), outra para listar os usuários já cadastrados (showUsers), e uma que iremos chamar no inicio do programa e que servirá para inicializar o array de elementos (initialize_array), ficando desta maneira:

{% highlight c %}
void initializeArray()
{
  int x= 0;
  for (x = 0; x < NUM_MAX_USERS; x++)
    usuarios[x] = '0';
}

void show_credits()
{
  clear_scr();
  printf("Criado por: Rodrigo Vargas\n");
  printf("Visite www.rodrigovg.com.br \n");
  sleep(3); 
}

int add_users()
{
  char user;
  int isError = 1;
  clear_scr();
  printf("Digite a primeira letra do usuário.\n");
  scanf(" %c", &user);
  
  //Search for a empty position
  int x= 0;
  for (x = 0; x < NUM_MAX_USERS; x++)
  {
    if (usuarios[x] == '0')
    {
      usuarios[x] = user;
      isError = 0;
      break;
    }
  }

  return isError;
}
{% endhighlight %}

E nosso switch ficará deste modo:

{% highlight c %}
switch(option)
    {
      case 1: 
      {
        if (add_users() == 1)
          printf("Lista cheia, não é mais possível adicionar usuários;\n");
        else
          printf("Usuário adicionado com sucesso.\n");
        sleep(2);
        break;
      }

      case 2:
      {
        show_users();
        break;
      }

      case 8:
      {
        show_credits();
        break;
      }
      default:{}
    }
{% endhighlight %}

Note que temos um retorno na função add_user, que retorna um 0 caso a inclusão tenha ocorrido sem erros. No próximo post iremos expandir este array para algo dinâmico e veremos como se comportam as strings na linguagem C. Até lá.