---
layout: post
title:  "Dado Virtual"
date:   2015-08-02 01:32:13
categories:
permalink: dado-virtual
---

O objetivo deste projeto é criar um dado virtual. Para isso, vamos recorrer à função `random()` e a algum *hardware* que já conhecemos, um *display* de 7 segmentos e um botão. No final, sempre que carregarmos no botão, um número aleatório é gerado e mostrado no *display*.

## Hardware

**Material necessário:**

* 1 display de 7 segmentos

* 1 botão

* 1 resistência de 10K Ohms

* alguns fios de montagem

O display de 7 segmentos e o botão são montados de um modo semelhante ao que seguimos no _workshop_. Para ajudar à montagem, criamos o seguinte esquema de ligações usando o programa [Fritzing](http://fritzing.org/).

![]({{ site.baseurl}}/img/dado.png)

## Desenvolvimento do programa

Nesta fase, procuramos entender o que queremos que o nosso código faça. O primeiro passo é configurar o `void setup()`. Configuramos os pins digitais de 2 a 8 como `OUTPUT` (já que estamos a enviar informação para os LEDs dos segmentos) e o pin digital 9 como `INPUT` (já que está a receber informação do botão).

Antes de avançarmos, precisamos de saber que funções temos de criar para este projeto. As funções são sempre seguidas de parênteses para serem facilmente identificadas. Começamos pela função `clear()` que limpa o *display*. Seguimos depois para as funções representativas de cada número a que se deram os nomes de `um()`, `dois()`, `tres()`, `quatro()`, `cinco()` e `seis()`. Temos assim representados os seis números presentes num dado. Todas estas funções estão definidas no exemplo depois do `void loop()`.

Para facilitar, associou-se a cada pin digital (de 2 a 8), uma letra que consta do seguinte diagrama (isto é opcional).

![]({{ site.baseurl}}/img/7segments.png)

Estas variáveis são definidas fora do `void setup()` e do `void loop()`.

Ainda precisamos, no entanto, de saber como vamos gerar os números aleatoriamente. A função `random()` que vem incluída no Arduino IDE permite gerar números inteiros aleatoriamente desde que lhe sejam passados os argumentos corretos.

```
random(valor menor inclusive, valor maior exclusive);
```

Ou seja,

```
random(1, 7);
```

Esta instrução vai gerar números aleatórios entre 1 e 6 (já que o 7 não está incluído). A grande vantagem de usar estas funções pré-definidas é que não precisamos de saber como foram criadas para as utilizarmos. A este conceito damos o nome de *abstração*.

Para usarmos este valor, temos de o associar a uma variável e, neste caso, usamos uma variável do tipo *long* que é o tipo de resultado que a função devolve. Como o nome implica, este tipo de variáveis é usada para guardar números muito grandes. Isto acontece porque esta função pode gerar números aleatórios bastante grandes. Todos estes pormenores foram conhecidos procurando a [documentação da função random()](https://www.arduino.cc/en/reference/random).

Falta apenas um pormenor para podermos prosseguir para a concretização do `void loop()`. A função `random()` precisa de gerar um número diferente a cada iteração do `void loop()`. Para isto, usamos a função `randomSeed()` para ler um valor aleatório dos pins analógicos. Para isto, basta incluir a seguinte instrução no void setup():

```
randomSeed(analogRead(0));
```

Este tipo de pormenores são conhecidos através da consulta da documentação acima citada.

Feito isto, falta-nos apenas pensar o que queremos que aconteça no `void loop()`.

Queremos que, sempre que carregarmos no botão, seja gerado num novo número aleatório que é mostrado no ecrã. Usamos, portanto, uma condição `if()` para descrever o que vai acontecer quando o botão é premido.

Criamos a variável que vai guardar o valor que é gerado aleatoriamente fora de qualquer função. No exemplo, esta variável chama-se *aleatorio*.

Dentro do `if()` criado anteriormente, atribuimos o valor da variável *aleatorio* à função `random()` com os argumentos usados acima.

Gerado o número aleatoriamente, queremos exibir no *display* o número gerado. Usamos outra condição `if()` para fazer esta correspondência. No exemplo, usamos uma estrutura começada por `if()` e seguida por cinco `else if()`. Isto acontece porque só existem as possibilidades: sair 1, 2, 3, 4, 5 ou 6. Se usássemos seis condições `if()`, o programa iria avaliar todas as condições. Apesar de não estar errado, não há necessidade disto e, por isso, devemos tentar sempre otimizar o nosso código.

E é isto. Tenta **sempre** escrever o teu próprio código baseado nesta explicação antes de veres a secção abaixo. Só tentanto e fazendo alguns erros é que se aprende verdadeiramente.

## Software

Abaixo apresenta-se o programa relativo ao dado virtual.

{% highlight c++ %}
// Definir pins por letras de acordo com o esquema
int a = 2;
int b = 3;
int c = 4;
int d = 5;
int e = 6;
int f = 7;
int g = 8;

// Criar a variável que vai guardar o número aleatório e inicializar a zero para evitar possíveis erros
long aleatorio = 0;

void setup() {
  // Configurar os segmentos como OUTPUT e o botão como INPUT
  pinMode(a, OUTPUT);
  pinMode(b, OUTPUT);
  pinMode(c, OUTPUT);
  pinMode(d, OUTPUT);
  pinMode(e, OUTPUT);
  pinMode(f, OUTPUT);
  pinMode(g, OUTPUT);
  pinMode(9, INPUT);
  // Começar o programa com o display desligado
  clear();
  // Assegurar um gerador de números aleatória
  randomSeed(analogRead(0));
}

void loop() {

  // Se o botão for pressionado...
  if (digitalRead(9) == HIGH) {

  	// Gerar numero aleatoriamente e guardá-lo na variável aleatorio
    aleatorio = random(1, 7);

    //Se sair o numero 1, mostrá-lo no display
    if (aleatorio == 1) {
      clear();
      um();
    }

    else if (aleatorio == 2) {
      clear();
      dois();
    }

    else if (aleatorio == 3) {
      clear();
      tres();
    }

    else if (aleatorio == 4) {
      clear();
      quatro();
    }

    else if (aleatorio == 5) {
      clear();
      cinco();
    }

    else if (aleatorio == 6) {
      clear();
      seis();
    }
  }
}

// Limpa o display
void clear() {
  digitalWrite(a, HIGH);
  digitalWrite(b, HIGH);
  digitalWrite(c, HIGH);
  digitalWrite(d, HIGH);
  digitalWrite(e, HIGH);
  digitalWrite(f, HIGH);
  digitalWrite(g, HIGH);
}

// Mostra o numero 1
void um() {
  digitalWrite(b, LOW);
  digitalWrite(c, LOW);
}

// Mostra o numero 2
void dois() {
  digitalWrite(a, LOW);
  digitalWrite(b, LOW);
  digitalWrite(g, LOW);
  digitalWrite(e, LOW);
  digitalWrite(d, LOW);
}

// Mostra o numero 3
void tres() {
  digitalWrite(a, LOW);
  digitalWrite(b, LOW);
  digitalWrite(g, LOW);
  digitalWrite(c, LOW);
  digitalWrite(d, LOW);
}

// Mostra o numero 4
void quatro() {
  digitalWrite(f, LOW);
  digitalWrite(g, LOW);
  digitalWrite(b, LOW);
  digitalWrite(c, LOW);
}

// Mostra o numero 5
void cinco() {
  digitalWrite(a, LOW);
  digitalWrite(f, LOW);
  digitalWrite(g, LOW);
  digitalWrite(c, LOW);
  digitalWrite(d, LOW);
}

// Mostra o numero 6
void seis() {
  digitalWrite(a, LOW);
  digitalWrite(f, LOW);
  digitalWrite(g, LOW);
  digitalWrite(c, LOW);
  digitalWrite(e, LOW);
  digitalWrite(d, LOW);
}

{% endhighlight %}


## Done!

Agora já tens o teu dado virtual e já sabes o que fazer quando não tiveres um dado para jogar *Monopoly*! Por falar em monopólio, imagina que tinhas dois displays de 7 segmentos e que querias mostrar números aleatoriamente de 2 a 12 (equivalente à soma dos dois dados). Consegues pensar como o poderias fazer? Como poderias dizer ao utilizador que tirou o mesmo número nos dois dados?
