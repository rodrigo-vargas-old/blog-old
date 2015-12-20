---
layout: post
title:  "Selection Sort"
date:   2015-07-20 00:00
categories: classificacao-de-dados
---

Hoje neste post, veremos mais um método de classificação, o selection sort. Este método é chamado assim pelo fato que seu algoritmo seleciona vários elementos em sequencia e coloca-os na ordem correta no vetor. Apesar deste algoritmo ter uma eficiencia baixa, é um pouco melhor que o método bubblesort e não utiliza nenhum espaço adicional, com exceção de algumas váriaveis de controle.

## Funcionamento

O algoritmo do select consiste em fazer a varredura completa do vetor começando de 0 até n. É encontrado o menor elemento e o mesmo é substituido na posição 0. Após isso, é incrementado o indice para 1. E é feita nova varredura de 1 até n. E assim sucessivamente até o vetor estar completamente ordenado. Note então que se somarmos todas as varreduras encontraremos um total de n² iterações visto que, temos (n - 1) + (n - 2) + (n - 3) + ... + 1 = n * (n - l)/2. A seguir a implementação em C.

## Implementação em C

<pre>
void selectsort(int *vector, int size)
{
  int x;
  for(x = 0; x < size; x++)
  {
    int smallValue = vector[x];
    int smallIndex = x;

    int y;
    for (y = x; y < size; y++)
    {
      if (vector[y] < smallValue)
      {
        smallValue = vector[y];
        smallIndex = y;
      }
    }

    int aux;

    aux = vector[x];
    vector[x] = vector[smallIndex];
    vector[smallIndex] = aux;
  }
}
</pre>

<pre>
void selectsort10Register()
{

  int vector[10] = {5, 6, 7, 4, 3, 2, 1, 9, 8, 0};
  

  selectsort(vector, 10);

  int x;

  for(x = 0; x < 10; x++)
    printf("%i ", vector[x]);
}
</pre>

Como podemos ver este é um algoritmo bastante simples mas que em compensação não deve ser usado para casos onde n não for muito pequeno.

Por hoje era isso pessoal, dúvidas críticas e sugestões nos comentários e até a próxima.