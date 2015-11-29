---
layout: post
title:  "Classificação por árvore"
date:   2015-07-27 00:00
categories: classificacao-de-dados
---
<p>Hoje, trago mais um método de classificação por seleção, a classificação por árvore binária. Neste método utilizamos uma árvore binária como fila de prioridade para fazer a disposição de todos os elementos do vetor de entrada.</p>

<h2>Funcionamento</h2>

<p>Este método de classificação consiste em fazer a varredura de todos os elementos do vetor de entrada e inseri-los em uma árvore binária. Essa inserção deve respeitar a regra:</p>

<p>
<ul>
<li>Se elemento a ser inserido for maior que o nodo atual, inserir o mesmo na subarvore a direita;</li>
<li>Se elemento a ser inserido for menor que o nodo atual, inserir o mesmo na subarvore a esquerda.</li>
</ul>

<p>Após realizar a inserção de todos os elementos do vetor, basta fazer uma varredura em ordem na árvore criada para obtermos o vetor classificado.</p>

<h2>Eficiencia</h2>

<p>A eficiencia deste algoritmo irá váriar conforme a ordenação inicial do vetor de entrada. Caso o mesmo esteja perfeitamente ordenado, temos uma eficiencia de n². Pois a árvore gerada, será basicamente um vetor, forçando o algoritmo a fazer a comparação de cada novo elemento inserido com todos os já inseridos, como mostrado na figura abaixo.</p>


<pre>
Vetor inicia: { 4, 9 , 12 }
         4
          \
           9
            \
             12
</pre>

<p>O mesmo irá acontecer para vetor inversamente ordenados. Para vetores totalmente desclassificados, o algoritmo será bem mais eficiente, visto que o mesmo irá gerar árvores mais bem equilibradas. Fazendo uma análise prática, este último caso acontece com mais frequencia, o que garante uma boa eficiencia deste algoritmo na maioria dos casos.</p>

<h2>Implementação em C</h2>

<p>Inicialmente vamos precisar da estrutura para representar os nodos da árvore, a struct tree.</p>

<pre>
struct tree{
  int value;
  tree *left;
  tree *right;
};
</pre>

<p>Em seguida criamos o método initialize para representar uma subarvore vazia.</p>

<pre>
tree* initialize()
{
  return NULL;
}
</pre>

<p>Em “makeTree”, fizemos a alocação do espaço de memória e atribuição do valor para o novo nodo.</p>

<pre>
tree* makeTree(int value, tree *left, tree *right)
{
  tree *newTree = malloc(sizeof(tree));

  newTree->value = value;
  newTree->left = left;
  newTree->right = right;

  return newTree;
}
</pre>

<p>Após a declaração das funções base, vamos para a função principal, binaryTreeSort.</p>

<pre>
void binaryTreeSort(int *vector, int size)
{
  tree *root;

  int x;
  for(x = 0; x< size; x++)
  {
    if (x == 0)
      root = makeTree(vector[x], initialize(), initialize());
    else
      insert(root, vector[x]);
  }

  readInOrder(root);
}
</pre>

<p>Aqui é aonde percorremos o vetor e montamos a árvore. A função insert (descrita abaixo) realiza a verificação de qual subarvore o elemento deve ser inserido;</p>

<pre>
void insert(tree *rootNode, int value)
{
  if (value < rootNode->value)
  {
    // Left
    if (rootNode->left == NULL)
      rootNode->left = makeTree(value, initialize(), initialize());
    else
      insert(rootNode->left, value);
  }
  else
  {
    // Right
    if (rootNode->right == NULL)
      rootNode->right = makeTree(value, initialize(), initialize());
    else
      insert(rootNode->right, value);
  }
}
</pre>

<p>Por fim, temos a função “readInOrder” que realiza a leitura final da árvore.</p>

<pre>
// Read in order
void readInOrder(tree *element)
{
  /* percorre sae, trata raiz, percorre sad; */
  if (element == NULL)
    return;

  readInOrder(element->left);
  printf("%i ", element->value);
  readInOrder(element->right);
}
</pre>

<p>Por hoje era isso pessoal, dúvidas, críticas e sugestões nos comentários abaixo e até a próxima.</p>