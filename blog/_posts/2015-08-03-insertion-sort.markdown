---
layout: post
title:  "Insertion Sort"
date:   2015-08-03 00:00
categories: classificacao-de-dados
---
No post de hoje, irei explicar o algoritmo de ordenação “Insertion Sort”. Este algoritmo faz a ordenação através de inserções sucessivas, geralmente já na posição correta. Apesar do seu nome ser insertion sort, existem outros algoritmos que funcionam de forma semelhante, não sendo este o único algoritmo a ordenar por inserção.

## Funcionamento

A idéia do Insertion Sort é, dado um vetor v desordenado, remover elementos deste vetor e inserir os mesmos em outro vetor, já nas posições corretas. Não necessariamente temos que replicar o vetor v para um outro vetor, podemos usar as posições iniciais do mesmo isto. Criando um limite imaginário no vetor, iremos mover os elementos para a esquerda, começando com v[0]. Visto que v[0] irá ser o único elemento na parte ordenada, o mesmo se ordena sem precisarmos deslocar nenhum elemento de posição, partimos então para v[1]. Enquanto v[1] for menor que seu sucessor v[0], iremos fazer deslocamentos até encontrar a posição ideal. Então teremos que sempre v[x] deve ser maior que v[x - 1], caso contrário o mesmo deve ser deslocado para a esquerda.

## Exemplo

Dado o vetor:

<pre>{ | 3, 2, 4, 1 }</pre>

Imagine que o sinal ‘|’ representa a fronteira imaginária entre elementos ordenados e não ordenados. Primeiramente, deslocamos 3 para a parte ordenada.

<pre>{ 3, | 2, 4, 1 }</pre>

Próximo elemento, 2, é menor que o seu antecessor ‘3’. Então, movemos o ‘2’ para a área ordenada e fazemos a troca.

<pre>{2 ,3, | 4, 1 }</pre>

Após é a vez do elemento 4.

<pre>{2 ,3, 4, | 1 }</pre>

4 já se ordenou sem trocas. Vamos agora para 1.

<pre>{ 2, 3, 4, | 1 }</pre>

1 é menor que todos os elementos, mas em tempo de execução do código não sabemos isso, então teremos que fazer vários deslocamentos até 1 ficar em sua posição correta.

<pre>{ 2, 3, 1, 4 |}</pre>

<pre>{ 2, 1, 3, 4 |}</pre>

<pre>{ 1, 2,  3,  4 |}</pre>

Assim, completamos a ordenção do vetor.

## Implementação em C

Abaixo a implementação da função insertion sort.

<pre>
void insertionSort(int *array, int size)
{
  int x;
  for (x = 1; x < size; x++)
  {   
    int j = x;

    int aux;
    aux = array[x];
    
    while (j != 0 && aux < array[j - 1])
    {
      array[j] = array[j - 1];
      j--;
    }

    array[j] = aux;
    int i;
    for (i = 0; i < size; i++)
    {
      printf("%i ", array[i]);
    }
    printf("\n");
  }
}
</pre>

O algoritmo aqui é bastante simples, iniciamos a leitura na posição 1, assumindo que array[0] está ordenado, conforme explicamos antes. Após, carregamos a váriavel j, com o valor de x, para que possamos fazer a leitura do vetor, do elemento onde x aponta, que é o primeiro elemento não ordenado do vetor, passando por todos os elementos ordenados anteriores ao mesmo. Paramos quando acharmos a posição correta, sendo ela 0, a primeira posição do vetor, ou quando o elemento for maior que a posição anterior que j aponta.

Saída gerada a cada iteração

<pre>
6 5 8 3 2 9 0 1 4 7 
5 6 8 3 2 9 0 1 4 7 
5 6 8 3 2 9 0 1 4 7 
3 5 6 8 2 9 0 1 4 7 
2 3 5 6 8 9 0 1 4 7 
2 3 5 6 8 9 0 1 4 7 
0 2 3 5 6 8 9 1 4 7 
0 1 2 3 5 6 8 9 4 7 
0 1 2 3 4 5 6 8 9 7 
0 1 2 3 4 5 6 7 8 9 
</pre>

Por hoje era isso pessoal, dúvidas, sugestões e críticas nos comentários abaixo e até semana que vem.