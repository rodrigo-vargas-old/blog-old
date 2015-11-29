---
layout: post
title:  "Listas Lineares - Parte 2"
date:   2015-06-08 00:00
categories: estruturas-de-dados
---

Continuando nossa série, e ainda falando de listas lineares, vamos ver hoje os dois últimos tipos de listas que ficaram faltando. A lista duplamente encadeada e a lista duplamente encadeada circular.

## Lista Duplamente Encadeada

A lista duplamente encadeada, difere de sua versão mais simples, a lista encadeada, pelo fato de possuir informação a respeito do elemento que está imediatamente antes a sua alocação. Um exemplo simples, considere a lista abaixo: 

<pre>
|   Celta   |------->|  Vectra  |------->|  Astra    |<br>
</pre>
Numa lista encadeada simples, o elemento “Vectra” enxerga apenas o elemento “Astra”. Não é possível acessar o elemento “Celta”, partindo de “Vectra”. Teríamos que iniciar uma leitura novamente pela raiz até chegarmos no elemento “Vectra”. Em uma lista grande teremos sérios problemas de performance. Na lista duplamente encadeada, Vectra possuirá os endereços de “Celta”, seu antecessor, e de “Astra” seu sucessor. Abaixo irei mostrar quais trechos do código deverão ser adaptados com base no código de lista encadeada mostrado no post anterior.

<pre>
struct elem{
  elemento *prev;
  char valor[10];
  elemento *next;
};
</pre>

Iniciando pela nossa estrutura, temos a adição da varivel prev. Que simboliza o elemento anterior.

<pre>
void insereElemento(char *valor, elemento *nodoRaiz)
{
  elemento *aux, *novo;

  aux = nodoRaiz;
  
  while (aux->next != NULL)
    aux = aux->next;
  
  novo = malloc(sizeof(elemento));
  strcpy(novo->valor, valor);
  aux->next = novo;
  novo->prev = aux;
}
</pre>

A função Insere elemento necessita agora sempre preencher o endereço do novo item na lista na váriavel prev da mesma.

<pre>
void removeElemento(int index, elemento *nodoRaiz)
{
  int x = 0;
  elemento *aux;

  aux = nodoRaiz;

  while(x != index)
  {
    aux = aux->next;
    x++;
  }

  if (aux == nodoRaiz)
    nodoRaiz = aux->next;
  else
    aux->prev->next = aux->next;

  free(aux);
}
</pre>

Em removeElemento, o link entre os elementos ficou mais fácil. Agora podemos acessar a o valor de next do elemento anterior, apenas com a linha “aux->prev->next = aux->next;”.

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
  nodoRaizLista2->prev = ptrAux;
}
</pre>

Em combina listas, apenas temos que ter o cuidado de atribuir o valor prev ao primeiro elemento da segunda lista. Assim como em “copiaLista”.

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
      novoNodo->prev = ptrAux2;

      ptrAux2 = ptrAux2->next;
    }

    ptrAux = ptrAux->next;
  }

  return nodoRaizListaDestino;
}
</pre>

Apenas modificações simples como visto, alguns bytes a mais em memória e teremos bastante flexibidade para tratar listas grandes.

No link do github você encontra o código para esta estrutura.

## Lista Duplamente Encadeada Circular

A lista duplamente encadeada circular, varia muito pouco em relação a lista duplamente encadeada, apenas pelo fato do último elemento sempre se ter relação com o primeiro, e o primeiro sempre ter relação com o último, formando um círculo fechado, onde não teremos ponteiros que no tenham valores. Abaixo irei explicar a diferença em alguns métodos:

<pre>
elemento* criaLista(char *elementoInicial)
{
  elemento *nodoRaiz = malloc(sizeof(elemento));
  strcpy(nodoRaiz->valor, elementoInicial);
  nodoRaiz->prev = nodoRaiz;
  nodoRaiz->next = nodoRaiz;

  return nodoRaiz;
}
</pre>

Neste método temos a referencia dos ponteiros prev e next para o mesmo elemento, para mantermos a regra de nenhum ponteiro nulo.

<pre>
void insereElemento(char *valor, elemento *nodoRaiz)
{
  elemento *aux, *novo;

  aux = nodoRaiz;
  
  int start = 1;

  while (aux->next != nodoRaiz && start != 1)
  {
    start = 0;
    aux = aux->next;
  }
  
  novo = malloc(sizeof(elemento));
  strcpy(novo->valor, valor);
  aux->next = novo;
  novo->prev = aux;
  novo->next = nodoRaiz;
  nodoRaiz->prev = novo;
}
</pre>

Em insereElemento temos a tarefa de adicionar no sucessor do último elemento, o ponteiro para a raiz. Além de também apontar o ponteiro de “prev” do nodoRaiz para o novo elemento.

<pre>
void removeLista(elemento *nodoRaiz)
{
  elemento *next, *aux;

  aux = nodoRaiz;

  while (aux->next != nodoRaiz)
  {
    next = aux->next;
    free(aux);
  }

  free(aux);
}
</pre>

Agora na função removeLista, temos a mundaça onde o nosso teste não irá encontrar um ponteiro nulo, então temos que verificar se o ponteiro next do nodo auxiliar é igual ao nodo raiz, o primeiro elemento. Ao final da iteração, ainda temos mais uma chamada ao free para eliminar o último elemento.

<pre>
void leLista(elemento *nodoRaiz)
{
  elemento *aux;

  aux = nodoRaiz;

  int start = 1;

  while (aux != nodoRaiz && start != 1)
  {
    start = 0;
    printf("%s\n", aux->valor);
    aux = aux->next;
  }
}
</pre>

Em leLista, temos a adição de uma lógica, para consideramos o nodoRaiz o nodo final, mas adicionando um teste para evitar sair da iteração logo na primeira vez.

Por hoje era isso pessoal, críticas e sugestões nos comentários e até a próxima.