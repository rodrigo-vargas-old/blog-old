---
layout: post
title:  "Bubblesort"
date:   2015-06-06 00:00
categories: classificacao-de-dados
---

Olá. No post de hoje irei iniciar uma série que fala sobre métodos de classificação. Conhecer os métodos de conhecidos mais usados e saber aplica-los em problemas reais é uma boa habilidade para um programador. Sendo assim, no post de hoje irei falar sobre o bubblesort.

## Funcionamento

O bubblesort é o método mais simples para ordenação de uma lista. Em contrapartida a sua eficiência é muito baixa, se aproximando de N². O que isto significa? Significa que a medida que n cresce, a quantidade de iterações necessária para classificar cresce exponencialmente.

O algoritmo de bubblesort consiste em pegar inicialmente o primeiro elemento do vetor e verificar se o elemento imediatamente posterior é maior que o mesmo. Caso seja verdadeiro, os dois elementos são trocados de posição. Após isso, partimos para o próximo elemento, que pode ser inclusive o mesmo primeiro elemento, que foi para a segunda posição. Com este método vamos deslocando os elementos pouco a pouco para suas posições corretas. De modo que ao final de n² iterações teremos o vetor ordenado. Esta ordenação pode ocorrer antes, caso o vetor esteja próximo da ordenação.

## Implementação em C

Abaixo segue implementação da função de bubblesort:

<pre>
void bubbleSort(int *vetor, int size)
{
  int switched = 1;
  int iteracao = 0;
  int x;

  while(switched == 1)
  {
    printf("Iteração %i \n", iteracao);   
    printf("\n");

    switched = 0;
    
    for (x = 0; x < size-1; x++)
    {
      if (vetor[x] > vetor[x + 1])
      {
        int aux;
        aux = vetor[x];
        vetor[x] = vetor[x + 1];
        vetor[x + 1] = aux;
        switched = 1;
      }
    }

    iteracao++;
  }
}
</pre>

Nesta implementação temos uma pequena melhoria. A váriavel “switched”, verifica sempre se houve alguma troca no vetor durante a iteração. Caso não houver, o vetor está ordenado e não precisa de iterações adicionais. Outra melhoria que podemos fazer no bubblesort, seria a comparação do elemento atual com o elemento que vem antes do elemento analisado. Desta forma, estamos agilizando o deslocamento dos elementos para suas corretas posições. Segue o código.

<pre>
void bubbleSort2(int *vetor, int size)
{
  int switched = 1;
  int iteracao = 0;

  while(switched == 1)
  {
    printf("Iteração %i \n", iteracao);
    int x;

    switched = 0;
    
    for (x = 1; x < size-1; x++)
    {
      if (vetor[x] > vetor[x + 1])
      {
        int aux;
        aux = vetor[x];
        vetor[x] = vetor[x + 1];
        vetor[x + 1] = aux;
        switched = 1;
      }

      if(vetor[x - 1] > vetor[x])
      {
        int aux;
        aux = vetor[x - 1];
        vetor[x - 1] = vetor[x];
        vetor[x] = aux;
        switched = 1;
      }
    }

    iteracao++;
  }
}
</pre>

E por hoje era isso pessoal. Dúvidas, críticas e sugestões nos comentários abaixo e até a próxima.
