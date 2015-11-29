---
layout: post
title:  "Quicksort"
date:   2015-07-13 00:00
categories: classificacao-de-dados
---

<p>Continuando a série sobre algoritmos de classificação, no post de hoje veremos o funcionamento do algoritmo quicksort. Este algoritmo faz a classificação de um vetor usando trocas por partição, ou seja o vetor principal é separado em subvetores, agilizando o processo de classificação.</p>

<h2>Funcionamento</h2>

<p>Considere o seguinte vetor inicial:</p>

<pre>6 5 8 3 2 9 0 1 4 7 </pre>

<p>Escolhemos um elemento deste vetor para ser o nosso pivô. Vamos pegar o primeiro para simplificar o entendimento. Após a escolha do pivo, instanciamos dois ponteiros, down e up, no início e fim, respectivamente do vetor.</p>

<pre>
d                         u
6 5 8 3 2 9 0 1 4 7 
</pre>

<p>O algoritmo quicksort consiste em 3 etapas.</p>

<p>
<ul>
<li>Incrementar down até o mesmo ser maior que o elemento pivo (6).</li>
<li>Decrementar up até o mesmo ser menor ou igual a anchor.</li>
<li>Caso up seja maior que down, alterar de posição os elementos x[up] e x[down].</li>
<li>Volte ao passo 1.</li>
</ul>

<p>Repita estes procedimentos até que o passo na posição 3 falhe (down > up). Quando isto ocorrer a posição ideal para o pivô será encontrada. Então basta fazer a troca de x[pivo] por x[up].</p>

<p>Exemplo</p>

<pre>
d                 u
6 5 8 3 2 9 0 1 4 7

  d               u
6 5 8 3 2 9 0 1 4 7

  d             u
6 5 8 3 2 9 0 1 4 7

  d             u
6 4 8 3 2 9 0 1 5 7

    d           u
6 4 8 3 2 9 0 1 5 7

    d           u
6 4 5 3 2 9 0 1 8 7

      d         u
6 4 5 3 2 9 0 1 8 7

        d       u
6 4 5 3 2 9 0 1 8 7

          d     u
6 4 5 3 2 9 0 1 8 7

          d   u
6 4 5 3 2 9 0 1 8 7

          d   u
6 4 5 3 2 1 0 9 8 7

            d u
6 4 5 3 2 1 0 9 8 7

              d
              u
6 4 5 3 2 1 0 9 8 7

            u d
6 4 5 3 2 1 0 9 8 7
</pre>

<p>Neste ponto, o teste down < up irá falhar e a posição ideal para o elemento pivo (6) é encontrada. Agora fizemos a troca de x[up] por x[pivo].</p>

<pre>0 4 5 3 2 1 6 9 8 7</pre>

<p>Com o elemento 6 devidamente posicionado, podemos agora partir para a classificação dos subvetores {0 4 5 3 2 1} e { 9 8 7}. O mesmo processo é aplicado. Note que se o subvetor tem tamanho 1, ele já está automaticamente classificado. Implementaremos agora esta rotina em C e analisaremos sua execução.</p>


<h2>Implementação em C</h2>

<pre>
void quickSort(int *vetor, int start, int end)
{
  printf("%i - %i\n", start, end);
  if (end - start <= 1)
    return;

  int x;
  for (x = start; x <= end; x++)
  {
    printf("%i ", vetor[x]);
  }
  printf("\n");
  int up, down;

  int anchor = vetor[start];

  down = start;
  up = end;

  while (down < up)
  {
    while(vetor[down] <= anchor)
      down++;

    while(vetor[up] > anchor)
      up--;

    if(up > down)
    {
      int aux;
      aux = vetor[up];
      vetor[up] = vetor[down];
      vetor[down] = aux;
    }

     for (x = start; x <= end; x++)
    {
      printf("%i ", vetor[x]);
    }


    printf("\n");
  }

  int aux2 = vetor[up];
  vetor[up] = vetor[start];
  vetor[start] = aux2;

  for (x = start; x <= end; x++)
  {
    printf("%i ", vetor[x]);
  }
  printf("\n");

  quickSort(vetor, start, up-1);
  quickSort(vetor, up+ 1, end);
}
</pre>

<p>Iniciamos o algoritmo com alguns printf apenas para gera uma saída que possamos analisar em seguida. Abaixo temos a declaração dos ponteiros up e down, e da váriavel anchor que será o pivo. Neste algoritmo estamos assumindo que o pivo sempre será o primeiro elemento do subvetor, mas existem melhores opções para isso. Vamos usar esta para simplicar o código.</p>

<p>Em seguida começa o loop inicial, que checará se down é maior que up, que como vimos, é a condição de saída para indicar a descoberta da posição ideal do elemento pívo. Em seguida, realizamos os 3 passos citados, e por fim, fizemos um print da situação. Após o laço while, fizemos a troca do elemento na posição up pelo elemento na posição start.</p>

<p>A saída da função será:</p>

<pre>
6 5 8 3 2 9 0 1 4 7 
//Ordena vetor inicial
0 - 9
6 5 8 3 2 9 0 1 4 7 
6 5 4 3 2 9 0 1 8 7 
6 5 4 3 2 1 0 9 8 7 
6 5 4 3 2 1 0 9 8 7 
0 5 4 3 2 1 6 9 8 7 
//Subvetor 0-5
0 - 5
0 5 4 3 2 1 
0 5 4 3 2 1 
0 5 4 3 2 1 
0 - -1 // 0 já está ordenado, não precisa mexer
1 - 5
// Ordena subvetor de 1 até 5
5 4 3 2 1 
5 4 3 2 1 
1 4 3 2 5 
1 - 4
1 4 3 2 
1 4 3 2 
1 4 3 2 
1 - 0
2 - 4
4 3 2 
4 3 2 
2 3 4 
2 - 3
5 - 4
6 - 5
7 - 9
9 8 7 
9 8 7 
7 8 9 
7 - 8
10 - 9
Vetor ordenado 
0 1 2 3 4 5 6 7 8 9 
</pre>

<p>Por hoje era isso pessoal, dúvidas críticas e sugestões nos comentários e até semana que vem.</p>