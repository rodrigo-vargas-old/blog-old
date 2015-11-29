---
layout: post
title:  "Listas Lineares - Parte 1"
date:   2015-06-01 00:00
categories: estrutura-de-dados
---

Hoje, começo uma série que irá mostrar os principais tipos de estrutura de dados, explicando-os com exemplos em c. Começaremos pela lista linear. A lista linear é uma estrutura de dados, onde temos uma disposição lógica linear de elementos. Nem sempre esta disposição será acompanhada de uma disposição sequencial dos elementos na memória do computador, que é o caso dos arrays. Para explicar as listas separamos elas em grupos, segundo suas propriedades, a primeira propriedade que veremos é a forma de alocação.

## Formas de Alocação

Quanto a forma de alocação, as listas lineares se dividem em dois tipos:

* Sequenciais: São os conhecidos vetores ou arrays. São estaticamente declarados. Onde todos os elementos da lista se encontram alocados sequencialmente na memória. Exemplos de uso: os vagões de um trem.
* Encadeada: Cada elemento de um lista linear encadeada possui além do seu valor, informações a respeito do próximo nodo na lista e/ou do nodo anterior. Geralmente esta informação é a posição de memória onde o próximo nodo está alocado. Exemplo de uso: a sala de espera de um consultório, onde os pacientes se sentam em diferentes lugares, porém, existe uma sequência lógica no atendimento.


Para ilustrar o funcionamento destas listas, implementaremos os 4 modelos de listas lineares:

* Lista Sequencial;
* Lista Encadeada;
* Lista Duplamente Encadeada;
* Lista Duplamente Encadeada Circular.


Cada uma destas listas, possuirá as seguintes operações.

## Operações

* Criação de uma lista
* Remoção de uma lista
* Inserção de um elemento da lista
* Remoção de um elemento da lista
* Acesso de um elemento da lista
* Alteração de um elemento da lista
* Combinação de duas ou mais listas
* Classificação da lista
* Cópia da lista
* Localizar nodo através de info


## Lista Sequencial

Na lista sequencial, os métodos tem menos lógica, pelo fato de estarmos trabalhando com váriaveis declaradas estaticamente. Abaixo os métodos desenvolvidos e uma breve descrição dos mesmos:

<pre>
void criaLista()
{
  int x;
  for (x = 0; x < TAMANHO_LISTA; x++)
    lista[x] = ' ';
}
</pre>

O método cria lista, não propriamente cria a lista mas sim inicializa a mesma colocando um elemento vazio em sua posição.

<pre>
void insereElemento(char elemento, int posicao)
{
  lista[posicao] = elemento;
}
</pre>

Este método salva o valor repassado no elemento do array apontando por “index”.

<pre>
void removeElemento(int posicao)
{
  lista[posicao] = ' ';
}
</pre>

Apenas remove o valor colocado na posição do array informada.

<pre>
void leituraLista(char* listaToRead, int tamanho)
{
  int x;
  
  for(x = 0; x < tamanho; x++)
    printf("%c\n", listaToRead[x]);
}
</pre>

Faz a exibição da lista no terminal.

<pre>
char acessaElemento(int posicao)
{
  return lista[posicao];
}
</pre>

Retorna o elemento na posição indicada pela variável “posicao”.

<pre>
char* combinaListas(char* lista1, int sizeLista1, char* lista2, int sizeLista2)
{
  int sizeListaResultante = sizeLista1 + sizeLista2;

  int x;
  for (x = 0; x < sizeLista1; x++)
    lista[x] = lista1[x];

  for (x = sizeLista1; x < sizeListaResultante; x++)
    lista[x] = lista2[x - sizeLista1];

  return lista;
}
</pre>

Combina duas listas, alocando em memória uma nova lista com a soma do tamanho das duas listas repassadas, e atribuindo os valores da lista 1 no início da nova lista e os valores da lista 2 nas próximas posições, nada complexo.

<pre>
char* copiaLista(char* lista, int sizeLista, char* listaDestino)
{
  int x;
  for (x = 0; x < sizeLista; x++)
  {
    listaDestino[x] = lista[x];
  }
}
</pre>

Copia a lista na posição de memória apontada pela varíavel “lista” para a lista apontada por “listaDestino”.

<pre>
char localizaNodo(char*lista, int sizeLista, char elemento)
{
  int x;
  for (x = 0; x < sizeLista; x++)
  {
    if (lista[x] == elemento)
      return x;
  }

  return 0;
}
</pre>

Varre o array procurando um determinado nodo.

No link do github, é possível ver todo o código, inclusive com as funções utilizadas para teste.

## Lista Encadeada

O primeiro método que iremos escrever o responsável por alocar memória e colocar o valor no primeiro elemento da nossa lista, que é se diferencia dos demais pelo fato dele ser o nosso referencial. Se o perdermos, não conseguiremos mais recuperar nossa lista da memória. Para organizar a estrutura da lista, criei uma struct chamada elemento:
typedef struct elem elemento:

<pre>
struct elem{
  char valor[10];
  elemento* next;
};
</pre>

Basicamente  é o valor do elemento com um espaço para armazenar a informação do próximo elemento, encadeando assim a lista. Abaixo o método criaLista:

<pre>
elemento* criaLista(char *elementoInicial)
{
  elemento *nodoRaiz = malloc(sizeof(elemento));
  strcpy(nodoRaiz->valor, elementoInicial);
  
  return nodoRaiz;
}
</pre>

Recebemos um valor em string como parâmetro, em seguida fizemos a alocação do espaço de memória referente a um “elemento” (“size of(elemento)”), copiamos o valor para o local apropriado e em seguida retornamos o valor do endereço de memória para a função chamadora. É importante guardar este valor em alguma variável, visto que outras funções somente irão funcionar com este valor. Agora, vamos para o método que insere novos elementos.

<pre>
void insereElemento(char *valor, elemento *nodoRaiz)
{
  elemento *aux, *prv;

  aux = nodoRaiz;
  
  if (aux == NULL)
    return;
  
  while (aux->next != NULL)
    aux = aux->next;
  
  prv = aux;
  aux = malloc(sizeof(elemento));
  strcpy(aux->valor, valor);
  prv->next = aux;
}
</pre>

Neste caso, recebemos como parâmetro o ponteiro que indica o início de nossa lista encadeada, e o valor do novo elemento. Basicamente, fizemos uma iteração na lista até chegarmos ao seu fim. O objetivo desta iteração é achar o último nodo da lista, para que possamos inserir o valor do endereço de memória do novo nodo, no seu campo “next”. Assim, mantemos o encadeamento da lista. Próximo metodo, removendo a lista.

<pre>
void removeLista(elemento *nodo)
{
  if (nodo->next != NULL)
    removeLista(nodo->next);

  free(nodo);
}
</pre>

Neste método usamos recursividade para excluir todos os elementos da lista. A cada chamada verificamos se o campo next do nodo está vazio, caso não esteja, chamamos novamente a função removeLista, que irá tentar remover o próximo nodo, caso ele não esteja vazio é claro. Ao chegarmos no final da lista, onde o teste falhará, a recursividade tratará de excluir nossa lista do último para o primeiro nodo. O próximo método irá ler toda a lista, veja só:

<pre>
void leLista(elemento *nodoRaiz)
{
  elemento *aux;

  aux = nodoRaiz;

  while (aux != NULL)
  {
    printf("%s\n", aux->valor);
    aux = aux->next;
  }
}
</pre>

Semelhante ao método de inserir um novo elemento, está lista também percorre a lista toda, porém fazendo a exibição do valor de cada nodo.

<pre>
void removeElemento(int index, elemento *nodoRaiz)
{
  int x = 0;
  elemento *aux, *prv;

  aux = nodoRaiz;

  while(x != index)
  {
    prv = aux;
    aux = aux->next;
    x++;
  }

  if (aux == nodoRaiz)
    nodoRaiz = aux->next;
  else
    prv->next = aux->next;

  free(aux);
}
</pre>

Em removeElemento, iteramos a lista para achar o índice a ser removido, ao encontra-lo, rearranjamos a ligação entre os nodos, fazendo que o nodo anterior ao que seja exclúido, se ligue ao seu próximo, caso o nodo a serir excluido seja o primeiro, ainda temos que ter o cuidado de referenciar o nodo seguinte como sendo a nova raiz.

<pre>
elemento* acessaElemento(int index, elemento *nodoRaiz)
{
  int x = 0;
  elemento *aux;

  aux = nodoRaiz;

  while(x != index)
  {
    aux = aux->next;
    x++;
  }

  return aux;
}
</pre>

Semelhante ao le lista, porm com um alvo.

<pre>
elemento* updateElemento(int index, elemento *nodoRaiz, char *novoValor)
{
  elemento *nodoParaEditar = acessaElemento(index, nodoRaiz);

  strcpy(nodoParaEditar->valor, novoValor);
}
</pre>

Nesta função, fazemos o reaproveitamento de “acessaElemento” para conseguirmos a posição de memória do nodo a ser excluído.

<pre>
elemento* combinaListas(elemento *nodoRaizLista1, elemento *nodoRaizLista2)
{
  elemento *ptrAux;
  ptrAux = nodoRaizLista1;

  while(ptrAux->next != NULL)
  {
    ptrAux = ptrAux->next;
  }

  ptrAux->next = nodoRaizLista2;
}
</pre>

Aqui um método um pouco diferente. Nele percorremos a primeira lista até o final, e fazemos a ligação do último nodo com o primeiro nodo da segunda lista.

<pre>
elemento* copiaListaSlow(elemento *nodoRaizListaFonte)
{
  elemento *ptrAux, *ptrAux2;
  elemento *nodoRaizListaDestino;

  ptrAux = nodoRaizListaFonte;

  while(nodoRaizListaFonte->next != NULL)
  {
    if (nodoRaizListaDestino == NULL)
    {
      nodoRaizListaDestino = criaLista(nodoRaizListaFonte->valor);
      ptrAux2 = nodoRaizListaDestino;
    }
    else
    {
      insereElemento(nodoRaizListaFonte->valor, nodoRaizListaDestino);
    }
  }

  return nodoRaizListaDestino;
}
</pre>

Nesta função, temos o mesmo objetivo, mas reaproveitando os métodos já criados. Note que devido ao número maior de iterações para achar o elemento, este método se mostra mais demorado para conseguir o mesmo objetivo;

<pre>
elemento* copiaLista(elemento *nodoRaizListaFonte)
{
  elemento *ptrAux, *ptrAux2;
  elemento *nodoRaizListaDestino;

  ptrAux = nodoRaizListaFonte;

  while(ptrAux != NULL)
  {
    if (nodoRaizListaDestino == NULL)
    {
      nodoRaizListaDestino = criaLista(ptrAux->valor);
      ptrAux2 = nodoRaizListaDestino;
    }
    else
    {
      elemento *novoNodo = malloc(sizeof(elemento));
      strcpy(novoNodo->valor, ptrAux->valor);
      ptrAux2->next = novoNodo;
      ptrAux2 = ptrAux2->next;
    }

    ptrAux = ptrAux->next;
  }

  return nodoRaizListaDestino;
}
</pre>

Em copia lista, replicamos a lista fazendo a iteração da primeira lista, e inserindo novos elementos em uma segunda lista. No primeiro nodo, fizemos a criação da lista, salvando seu valor raiz, e em seguida realizamos n inserções. Até chegarmos ao final da lista fonte.

<pre>
elemento* buscaNodo(char *valor, elemento *nodoRaiz)
{
  elemento *ptrAux;
  ptrAux = nodoRaiz;

  while(ptrAux != NULL)
  {
    if (strcmp(ptrAux->valor, valor) == 0)
      break;

    ptrAux = ptrAux->next;
  }

  return ptrAux;
}
</pre>

Semelhante a buscaNodo, porém com um valor ao invés de um índice.

Na página do github temos todos os métodos usados para testar o programa, passe lá de uma olhadas neles.

No próximo post, iremos ver os outros dois tipos de lista linear, a duplamente encadeada e a duplamente encadeada circular.
