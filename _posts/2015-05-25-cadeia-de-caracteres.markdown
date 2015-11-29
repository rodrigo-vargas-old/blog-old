---
layout: post
title:  "Cadeia de Caracteres"
date:   2015-05-25 00:00
categories: estruturas-de-dados
---

Cadeias de caracteres ou strings, são sequências de simbolos, números ou letras de caracteres normalmente terminados por um delimitador. Em c, este delimitador é a expressão ‘/0’.

## Exibindo e lendo uma cadeia de caracteres

Para ler uma cadeia de caracteres em C, usamos o comando “scanf”. E para lermos uma cadeia de caracteres utilizamos o comando “printf”, desta forma:

<pre>
#include <stdio.h>

char texto[15];

void main (int argc, char** argv)
{
  scanf("%s", texto);

  printf("Texto escolhido: %s\n", texto);
}
</pre>

## Manipulando cadeia de caracteres

Em C, temos a biblioteca string.h, que disponibiliza várias operações com strings. Entre algumas podemos citar:


* strlen(texto): Retorna o tamanho da string texto em número de caracteres;
* strstr(texto, busca): Procura a sub-string busca dentro da string texto e retorna um ponteiro para primeira ocorrência dela em texto, ou NULL (nulo) se não encontrar;
* strcpy(destino, fonte): Copia a string fonte para a string destino;
* strcat(destino, fonte): Concatena a string fonte no fim da string destino.


Como exercício, vamos criar funções semelhantes para entender o funcionamento de cada método.

### Strlen

Vamos criar o método “length”:

<pre>
int length(char* string)
{
  char* ptr;
  int tamanho = 0;
  ptr = string;

  while(*ptr != '\0')
  {
    tamanho++;
    ptr++;
  }

  return tamanho;
}
</pre>

Neste método, pedimos como parâmetro o endereço do array a ser mensurado. Em seguida, criamos uma váriavel auxiliar para fazer o incremento do ponteiro e uma para o incremento do parâmetro. Em seguida temos uma iteração que irá procurar pelo delimitador do fim da string. Ao final da iteração retornamos o tamanho da string. Para finalizar vamos criar o método que irá fazer o teste desta string:

<pre>
void testaFuncaoLength()
{
  char stringTeste[10] = "Teste";

  int tamanho = length(stringTeste);

  if (tamanho != 5)
    printf("Falha no teste %i \n", tamanho);
  else
    printf("Função retornou valor correto \n");
}
</pre>

### strstr

Para buscarmos uma string em outra string iremos criar o método indexOf:

<pre>
char* indexOf(char* string, char* substring)
{
  char *ptrString;
  char *firstPosition = 0;
  int match = 0;

  ptrString = substring;

  while (*string != '\0' && *ptrString != '\0')
  {
    if (*string == *ptrString)
    {
      match = 1;
      ptrString++;
      if (firstPosition == 0)
        firstPosition = string;
    }
    else
    {
      match = 0;
      ptrString = substring;
      firstPosition = 0;
    }
    
    string++;
  }

  if (match == 1)
    return firstPosition;
  else
    return 0;
}
</pre>

Explicando o método acima, inicialmente recebemos o endereço da string que conterá a substring e o endereço da substring. Após inicializamos algumas váriaveis auxiliares e chegamos na iteração principal do método. Nesta iteração corremos cada caractere da string principal e comparamos se este é igual ao caractere da substring, caso verdadeiro, incrementamos ambos os ponteiros, da string e o da substring. Caso este teste falhar, incrementamos apenas o ponteiro da string e reiniciamos o ponteiro do substring. Como teste de parada temos que caso a string chegue no final, indica que terminamos de analisar a string principal ou caso a substring chegue ao final, isto indica que achamos a substring dentro da string prinicipal. Qualquer uma destas duas condições indica um resultado final para o nosso método.

Abaixo segue a função que faz o teste da função de indexOf, também foi criada uma função assert para evitar replicar código:

<pre>
void assert(char* expectedValue, char* value)
{
  if (expectedValue != value)
    printf("Falha no teste %c \n", *expectedValue);
  else
    printf("Função retornou valor correto \n");
}

void testaFuncaoIndexOf()
{
  char stringTeste[10] = "Abelha";
  char subString[3] = "elh";
  char subString2[3] = "zzz";

  char* index = indexOf(stringTeste, subString);

  assert(index, &stringTeste[2]);

  char* index2 = indexOf(stringTeste, subString2);
  
  assert(index2, 0);
}
</pre>

### strcpy

Este método replica uma string em outra, vamos implementar seu funcionamento através da função “getString”.

<pre>
void getString(char* destiny, char* source)
{
  while(*source != '\0')
  {
    *destiny = *source;
    source++;
    *destiny++;
  }

  *destiny = *source;
}
</pre>

Esta função é bastante simples, visto que ela apenas percorre os dois arrays e copia char a char os valores. Atente que esta função não funcionará quando o destino for menor que a fonte, problema também encontrada na funço strcpy. Abaixo a função que fará o teste da “getString”.

<pre>
void testaFuncaoGetString()
{
  char teste[10]= "Um teste";
  char destino[10];

  getString(destino, teste);

  printf("%s\n", destino);
}
</pre>

### strcat

Para finalizar, iremos implementar o funcionamento de strcat com a função “concat”.

<pre>
void concat(char* destiny, char*source)
{
  while(*destiny != '\0')
  {
    destiny++;
  }

  while(*source != '\0')
  {
    *destiny = *source;
    destiny++;
    source++;
  }
  destiny++;
  *destiny = '\0';
}
</pre>

Recebemos como parametro, os endereços da string destino e da string fonte. Inicialmente percorremos a string destino até achar o delimitador da mesma. Após isso, copiamos o conteúdo de fonte para ela e por fim adicionamos o delimitador. A função abaixo faz o teste de strcat.

<pre>
void testaFuncaoConcat()
{
  char teste[10]= "Um teste";
  char destino[30] = "Aqui falta ";

  concat(destino, teste);

  printf("%s\n", destino);
}
</pre>

Por hoje era isso pessoal, dúvidas, críticas e comentários aqui embaixo e até a próxima.