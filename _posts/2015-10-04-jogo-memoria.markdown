---
layout: post
title:  "Jogo da Memória"
date:   2015-10-04 01:32:13
categories: arduino led random serial memory game
tags: [Arduino,LED,Serial,Jogo]
---

Neste tutorial, vamos fazer um jogo da memória recorrendo ao nosso Arduino. Vamos usar 3 LEDs como indicadores luminosos (e fica, desde já, o desafio de quem quiser adicionar mais LEDs para tornar o desafio mais complicado!) que deveremos posteriormente replicar no Serial Monitor do Arduino. Esta é a primeira vez que vamos usar o Serial Monitor diretamente embora já tenhamos usado a comunicação série no tutorial da Interface LED com Arduino e Processing.

## Hardware

**Material necessário:** 3 LEDs (um verde, um amarelo e um vermelho), 3 resistências de 220 Ohms e alguns fios de montagem.

Para ajudar à montagem, criou-se o seguinte esquema de ligações usando o programa *Fritzing*.

![]({{ site.baseurl}}/img/esquema.png)

Acaba por ser exatamente o mesmo esquema do tutorial anterior mas com um fim totalmente diferente.

>PRO TIP: Se os teus LEDs tiverem com pouco brilho, isso deve-se ao facto das resistências terem um valor demasiado alto. Podes usar resistências menores ou mesmo não colocar nenhuma. Apesar de isto não ser a melhor prática, com correntes tão pequenas como a dos LEDs não há grande problema.

## Desenvolvimento do programa

Este jogo envolve vários conceitos/etapas que vamos abordando ao longo do tutorial. A primeira coisa a fazer é definir os nosso três LEDs ligados nos pins 8, 9 e 10 como outputs.

{% highlight c++ %}
void setup()
{
  // Definir LEDs como OUTPUTs
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
}
{% endhighlight %}

Em seguida, precisamos também de iniciar a comunicação série que já sabemos como se faz. Vamos ainda adicionar a *seed* para o nosso gerador de números aleatórios que vai gerar as diferentes sequências (a mesma função que usámos no "Dado Virtual"; para mais informações consulta esse tutorial). Finalmente, e como o `void setup()` só corre uma vez, vamos escrever duas linhas de código que irão aparecer no Serial Monitor.

{% highlight c++ %}
void setup()
{
  // Iniciar comunicacao serie
  Serial.begin(9600);
  // Definir LEDs como OUTPUTs
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
  // Definir a seed do gerador de números aleatórios
  randomSeed(analogRead(0));
}
{% endhighlight %}

Pela primeira vez, vamos fazer um *print* que é algo muito comum em programação. Fazer *print* significa escrever algo na consola para que o utilizador saiba o que fazer a seguir. Para fazermos um print para o Serial Monitor usamos o método `Serial.print()` ou `Serial.println()`. A diferença entre os dois assenta basicamente no facto deste último método fazer uma quebra de linha entre cada linha. Se usássemos o `Serial.print()` a informação iria aparecer toda lado a lado sem qualquer tipo de espaçamento entre ela.

Vamos então informar o jogador que o jogo começou e o que ele precisa de fazer para começar a jogar.

{% highlight c++ %}
void setup()
{
  // Iniciar comunicacao serie
  Serial.begin(9600);
  // Definir LEDs como OUTPUTs
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
  // Definir a seed do gerador de números aleatórios
  randomSeed(analogRead(0));
  // Dar as instrucoes de inicio de jogo ao jogador
  Serial.println("**** INICIO DO JOGO ****");
  Serial.println("Prime s para comecar");
}
{% endhighlight %}

Assim, quando corrermos o nosso programa e abrirmos o Serial Monitor, estas duas linhas de texto irão aparecer.

### Início de cada nível

O nosso Jogo da Memória será dividido em 5 níveis diferentes cuja dificuldade vai aumentando substancialmente. O jogador sabe em que nível está uma vez que também isso será mostrado na consola. No entanto, deve ser feito um aviso de quando a sequência está pronta a ser mostrada. Uma boa alternativa é piscar todos os LEDs duas vezes, esperar um segundo e mostrar a sequência. Assim, o jogador nunca é apanhado desprevenido. Para facilitar as coisas, vamos criar uma função `void blinkTwice()` que faz exatamente o que o nome diz: faz com que todos os LEDs pisquem duas vezes. Ora vocês já consequem claramente escrever este tipo de função. Escreve-a e depois podes confirmar abaixo (mas não sem tentares primeiro!) a solução.

{% highlight c++ %}
void blinkTwice()
{
  // Ligar todos os LEDs
  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
  digitalWrite(10, HIGH);
  //Mantê-los ligados 1 segundo
  delay(1000);
  // Desligar todos os LEDs
  digitalWrite(8, LOW);
  digitalWrite(9, LOW);
  digitalWrite(10, LOW);
  // Mantê-los desligados 1 segundo
  delay(1000);
  // Ligar todo os LEDs
  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
  digitalWrite(10, HIGH);
  // Mantê-los ligados 1 segundo
  delay(1000);
  // Desligar todos os LEDs
  digitalWrite(8, LOW);
  digitalWrite(9, LOW);
  digitalWrite(10, LOW);
}
{% endhighlight %}

Como podemos ver, este código é redundante (repete-se desnecessariamente). No fim deste tutorial, já terão as ferramentas necessárias para escrever esta função de um modo mais compacto.

As outras instruções serão dadas no `void loop()` que será a última coisa a ser escrita já que vai juntar todas as componentes do jogo.

### Gerar sequência aleatória

O nosso jogo vai gerar uma sequência aleatória que a cada número gerado tem um LED associado. A ordem proposta é a que se encontra na imagem abaixo.

![]({{ site.baseurl}}/img/id_led.jpg)

Para gerarmos a nossa sequência aleatória vamos definir uma função só para este efeito. Neste caso, é a função `void generateSequence (int tempo, int sequencia)`. A primeira coisa a fazer é chamar a função `blinkTwice()` e dar um delay de 1 segundo antes da sequência começar.

{% highlight c++ %}
void generateSequence (int tempo, int sequencia)
{
  // Piscar os LEDs todos duas vezes
  blinkTwice();
  // Esperar 1 segundo para que o jogo comece
  delay(1000);
 }
{% endhighlight %}

Como podemos ver, esta função gera uma sequência baseada em dois parâmetros: o tempo que cada LED está aceso e o número de LEDs acesos que a sequência tem. Antes de gerarmos os nossos números aleatoriamente, precisamos de um sítio para os guardar. Para isso, usamos um tipo de estrutura dado conhecida como lista. Em C++ (que é a linguagem em que o Arduino IDE é baseado) precisamos sempre de definir o tamanho de uma lista. Por esta razão é que um dos argumentos da nossa função (`int sequencia`) é o tamanho da sequência. Assim, não precisamos de reescrever uma função sempre que queremos adicionar mais elementos à nossa sequência.

Podemos ter uma lista de vários tipos de variáveis. No nosso caso, será uma lista que guarda inteiros. Não nos devemos esquecer que as listas são naturalmente indexadas a zero e que ter em conta os limites da nossa lista é essencial. Se prestarmos atenção, nós já conhecemos o conceito de lista intuitivamente. Uma String é uma lista de caracteres (char) que podemos chamar usando os respetivos índices tal como fazemos numa lista.

{% highlight c++ %}
  // Criar uma lista de inteiros com o tamanho que e passado como argumento
  int ordemLeds[sequencia];
{% endhighlight %}

O `int` desta linha refere-se ao facto de ser uma lista de inteiros. `ordemLeds` é simplesmente o nome que foi dado à lista e `sequencia` é o tamanho da lista. Geralmente, é um número mas neste caso é uma variável que remete para um inteiro.

Agora que já temos um sítio para guardar a sequência de números, podemos gerá-la. Queremos gerar tantos números quantos os indicados na variável `sequência`. Para preenchermos a lista, usamos um ciclo `for`. Um ciclo `for`é útil quando pretendemos repetir uma dada instrução um determinado número de vezes.

Quando usamos um ciclo `for`, precisamos de inicializar uma variável que vai contar o número de iterações e definir o seu valor mínimo e o seu valor máximo para o qual o ciclo deixa de correr. Temos ainda de dizer se queremos que a variável aumente ou diminua a cada iteração.

{% highlight c++ %}
  // Gerar sequencia aleatoria
  for (int i = 0; i < sequencia; i++)
  {
    numeroGerado = random(1, 4);
    ordemLeds[i] = numeroGerado;
  }
{% endhighlight %}

É quase uma convenção usar o i como a variável que vai contar as iterações. Inicializamo-la a 0 na primeira condição do ciclo. Na segunda condição, definimos o valor máximo da variável. Atenção que se trata de um menor e não de um menor ou igual. Por exemplo, se `sequencia == 3` então i irá tomar os seguintes valores por ordem: 0, 1, 2. O ciclo itera 3 vezes. A terceira condição do ciclo diz-nos se a variável incrementa (i++) ou decrementa (i--). Esta notação significa que `i++ == i + 1` e `i-- == i - 1`.

Portanto, a cada iteração do nosso ciclo, um número aleatório é gerado entre 1 e 3. Lembra-te que o método `random()` tem dois argumentos quando queremos gerar números num dado intervalo. Como queremos gerar números entre 1 e 3, passamos como argumentos 1 e 4 `random(min inclusive, max exclusive)`.   

Depois de gerado o número, guardamo-lo na lista que criámos anteriormente. Agora que a sequência aleatória já foi gerada e guardada, temos de pôr os nossos LEDs a piscar de acordo com ela. Para isso, usamos novamente um ciclo `for`. Consegues pensar como podemos fazer isto?

As três condições são iguais às do último ciclo com a exceção de que substituímos o nome da variável. Começamos a ler a lista começando no índice 0. Se este valor for igual a 1, ligamos o primeiro LED; se for igual a 2, ligamos o segundo LED e assim sucessivamente. Usamos três condições do tipo `if` para chegar a este resultado.

{% highlight c++ %}
// Piscar os LEDs na sequencia gerada
  for (int j = 0; j < sequencia; j++)
  {
    led = ordemLeds[j];

    if (led == 1)
    {
      digitalWrite(8, HIGH);
      delay(tempo);
      digitalWrite(8, LOW);
      delay(tempo);
    }

    else if (led == 2)
    {
      digitalWrite(9, HIGH);
      delay(tempo);
      digitalWrite(9, LOW);
      delay(tempo);
    }

    else if (led == 3)
    {
      digitalWrite(10, HIGH);
      delay(tempo);
      digitalWrite(10, LOW);
      delay(tempo);
    }
{% endhighlight %}

A linha `led = ordemLeds[j];` usa uma variável chamada led para guardar o elemento da lista de índice j. Isto não é estritamente necessário para torna o código mais fácil de escrever. A variável `led` é um `int` que deve ser inicializado no início do código com a instrução `int led`.

Como deves ter reparado, não fechámos o nosso ciclo `for`. A última coisa a fazer é converter a nossa lista numa String (o porquê desta decisão será claro em breve). Para isso, inicializámos uma String também no início do código chamada `sequenciaNumerica`.

Para fazer esta conversão, concatenamos a String `sequenciaNumerica`, ou seja, adicionamos os elementos à String por ordem. Assim, depois do último `else if ()` adicionamos a linha `sequenciaNumerica = sequenciaNumerica + led;`. Para percebermos melhor o que está a acontecer, vamos ter em conta que na primeira iteração, `sequenciaNumerica == ""`, ou seja, trata-se de uma String vazia.

Quando chegamos ao fim da primeira iteração do ciclo `for`, juntamos o primeiro elemento da nossa lista à nossa String. Por exemplo, se o primeiro índice for 1, passamos de `sequenciaNumerica = ""` para `sequenciaNumerica == "1"`.

Falta apenas um pequeno pormenor para encerrarmos o método `generateSequence()`. Ainda não estabelecemos que a String inicialmente é uma String vazia. Para isso, adicionamos a linha `sequenciaNumerica = "";` antes do último ciclo `for`.

Posto isto, o nosso método `generateSequence` completo é então:

{% highlight c++ %}
void generateSequence (int tempo, int sequencia)
{
  // Piscar os LEDs todos duas vezes
  blinkTwice();
  // Esperar 1 segundo para que o jogo comece
  delay(1000);

  // Criar uma lista de inteiros com o tamanho que e passado como argumento
  int ordemLeds[sequencia];

  // Gerar sequencia aleatoria
  for (int i = 0; i < sequencia; i++)
  {
    numeroGerado = random(1, 4);
    ordemLeds[i] = numeroGerado;
  }

  // Inicialmente, a String sequenciaNumerica é uma String vazia
  sequenciaNumerica = "";

  // Piscar os LEDs na sequencia gerada
  for (int j = 0; j < sequencia; j++)
  {
    led = ordemLeds[j];

    if (led == 1)
    {
      digitalWrite(8, HIGH);
      delay(tempo);
      digitalWrite(8, LOW);
      delay(tempo);
    }

    else if (led == 2)
    {
      digitalWrite(9, HIGH);
      delay(tempo);
      digitalWrite(9, LOW);
      delay(tempo);
    }

    else if (led == 3)
    {
      digitalWrite(10, HIGH);
      delay(tempo);
      digitalWrite(10, LOW);
      delay(tempo);
    }
    // Converter a lista numa String   
    sequenciaNumerica = sequenciaNumerica + led;
  }
}
{% endhighlight %}

### Validar a resposta do jogador

De seguida, precisamos de criar um método que valide a resposta do jogador. A melhor maneira de expressar a vitória ou a derrota num jogo é uma variável `boolean`. Tem o valor `true` se o jogador introduziu a resposta certa e o valor `false` se o jogador introduzir a resposta errada.

Começamos por inicializar uma variável `win` do tipo `boolean` cujo resultado o método vai devolver. O seu valor por defeito é `true`. A razão pela qual lhe damos um valor inicial é porque se este valor estivesse totalmente dependente de uma condição `if()`, o método daria erro já que a condição poderia nunca se concretizar e não haveria nenhum valor para devolver.

Em seguida, fazemos um print para o Serial Monitor para que o jogador saiba que já pode introduzir a sua resposta. Até agora, temos:

{% highlight c++ %}
boolean checkUserInput()
{
  // Variável que guarda o resultado do jogo
  boolean win = true;

  Serial.println("**** Insere a tua resposta ****");

{% endhighlight %}

Para esperarmos por uma resposta do utilizador, usamos uma condição `if` vazia que apenas é validade quando o utilizador insere a sua resposta. O método `Serial.available()` é diferente de 0 quando o jogador introduzir com o Serial Monitor.

{% highlight c++ %}

  // Aguardar a resposta do utilizador
  while (Serial.available() == 0)
  {}

{% endhighlight %}

Depois do jogador introduzir a sua resposta, vamos lê-la e guarda-la numa String `user`, inicializada no início do nosso programa. Esta é a razão pela qual no método que gera a sequência, convertemos a lista numa String.

{% highlight c++ %}

  // Guardar o valor introduzido pelo utilizador no Serial Monitor numa String user
  if (Serial.available())
  {
    user = Serial.readString();
  }

{% endhighlight %}

Agora que já temos a resposta do jogador guardada na String `user`, basta compararmos com a String `sequenciaNumerica` que gerámos. Para isso usamos o método `equals()` que compara Strings e devolve `true` se elas forem iguais. Se elas forem diferentes, `win` passa a `false`. Caso contrário, mantém-se inalterada como `true`. Depois, basta fazer `return win;`.

{% highlight c++ %}

  // Se as sequências não coincidirem, a variável win passa a false
  if (!user.equals(sequenciaNumerica))
  {
    win = false;
  }

  return win;

{% endhighlight %}

A condição do `if()` pode parecer um pouco estranha mas é apenas uma maneira de escrever as mesmas coisas de uma forma mais condensada. O ponto de exclamação antes de invocarmos o método, nega a condição. Isto é precisamente o mesmo que ter:

{% highlight c++ %}

  user.equals(sequenciaNumerica) != true;

{% endhighlight %}

onde `!=` é o operador diferente de.

### Juntar tudo!

O mais difícil já está feito. Agora só temos de juntar tudo no `void loop()`.Para iniciar o jogo, vamos exigir que o jogador envie o caracter 's' através do Serial Monitor. Isto é o equivalente ao "Press Start" que vemos muitas vezes nos jogos. Isto não é feito ao acaso: assim, o jogo só começa quando o jogador estiver preparado e tiver aberto o Serial Monitor.

{% highlight c++ %}

  void loop() {
  if (Serial.available())
  {
    if (Serial.reads() == 's')
    {

    }
  }
}

{% endhighlight %}

Agora podemos começar a juntar os níveis e podemos fazer as combinações de tempo entre os LEDs acenderem e do tamanho da sequência que quisermos. Podes tornar o jogo o mais fácil ou o mais difícil que quiseres. O jogo deste tutorial tem 5 níveis com as seguintes caraterísticas.

| Nível     | Tempo (em segundos)          | Tamanho da sequência |
| ------------- |:-------------:| -----:|
| 1 | 1   | 3|
| 2 | 0.5 | 3|
| 3 | 1   | 4|
| 4 | 0.5 | 4|
| 5 | 0.5 | 5|

Como construímos os níveis? Basicamente, usando condições `if` encadeadas.

{% highlight c++ %}

if (Serial.available())
  {
    if (Serial.read() == 's')
    {
      Serial.println("----- NIVEL 1 -----");
      generateSequence(1000, 3);

      if (checkUserInput())
      {
        Serial.println("CORRETO!");
        Serial.println("----- NIVEL 2 -----");
        generateSequence(500, 3);

      else
      {
        Serial.println("GAME OVER :(");
      }  


{% endhighlight %}

Fazemos isto sucessivamente até termos tantos níveis quanto os desejados. Podes fazer imensas combinações diferentes e adicionar mais LEDs para tornares o jogo mais desafiante! Abaixo, podes ver um vídeo do jogo em ação!

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://player.vimeo.com/video/142503173' frameborder='0' webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>

### Código Completo

{% highlight c++ %}

long numeroGerado;
int led;
String sequenciaNumerica = "";
String user;
String copia;

void setup() {
  // Iniciar comunicacao serie
  Serial.begin(9600);
  // Definir LEDs como OUTPUTs
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  pinMode(10, OUTPUT);
  // Definir a seed do gerador de números aleatórios
  randomSeed(analogRead(0));
  // Dar as instrucoes de inicio de jogo ao jogador
  Serial.println("**** INICIO DO JOGO ****");
  Serial.println("Prime s para comecar");
}

void loop() {
  if (Serial.available())
  {
    if (Serial.read() == 's')
    {
      Serial.println("----- NIVEL 1 -----");
      generateSequence(1000, 3);

      if (checkUserInput())
      {
        Serial.println("CORRETO!");
        Serial.println("----- NIVEL 2 -----");
        generateSequence(500, 3);

        if (checkUserInput())
        {
          Serial.println("CORRETO!");
          Serial.println("----- NIVEL 3 -----");
          generateSequence(1000, 4);

          if (checkUserInput())
          {
            Serial.println("CORRETO!");
            Serial.println("----- NIVEL 4 -----");
            generateSequence(500, 4);

            if (checkUserInput())
            {
              Serial.println("CORRETO!");
              Serial.println("----- NIVEL 5 -----");
              generateSequence(500, 5);

              if (checkUserInput())
              {
                Serial.println("CORRETO!");
                Serial.println("PARABÉNS! GANHASTE O JOGO!");
              }

              else
              {
                Serial.println("GAME OVER :(");
              }
            }

            else
            {
              Serial.println("GAME OVER :(");
            }
          }

          else
          {
            Serial.println("GAME OVER :(");
          }
        }

        else
        {
          Serial.println("GAME OVER :(");
        }
      }

      else
      {
        Serial.println("GAME OVER :(");
      }
    }
  }
}

void generateSequence (int tempo, int sequencia)
{
  // Piscar os LEDs todos duas vezes
  blinkTwice();
  // Esperar 1 segundo para que o jogo comece
  delay(1000);

  // Criar uma lista de inteiros com o tamanho que e passado como argumento
  int ordemLeds[sequencia];

  // Gerar sequencia aleatoria
  for (int i = 0; i < sequencia; i++)
  {
    numeroGerado = random(1, 4);
    ordemLeds[i] = numeroGerado;
  }

  // Inicialmente, a String sequenciaNumerica é uma String vazia
  sequenciaNumerica = "";

  // Piscar os LEDs na sequencia gerada
  for (int j = 0; j < sequencia; j++)
  {
    led = ordemLeds[j];

    if (led == 1)
    {
      digitalWrite(8, HIGH);
      delay(tempo);
      digitalWrite(8, LOW);
      delay(tempo);
    }

    else if (led == 2)
    {
      digitalWrite(9, HIGH);
      delay(tempo);
      digitalWrite(9, LOW);
      delay(tempo);
    }

    else if (led == 3)
    {
      digitalWrite(10, HIGH);
      delay(tempo);
      digitalWrite(10, LOW);
      delay(tempo);
    }
    // Converter a lista numa String   
    sequenciaNumerica = sequenciaNumerica + led;
  }
}

boolean checkUserInput()
{
  // Variável que guarda o resultado do jogo
  boolean win = true;

  Serial.println("**** Insere a tua resposta ****");

  // Aguardar a resposta do utilizador
  while (Serial.available() == 0)
  {}

  // Guardar o valor introduzido pelo utilizador no Serial Monitor numa String user
  if (Serial.available())
  {
    user = Serial.readString();
  }

  // Se as sequências não coincidirem, a variável win passa a false
  if (!user.equals(sequenciaNumerica))
  {
    win = false;
  }

  return win;
}


void blinkTwice()
{
  // Ligar todos os LEDs
  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
  digitalWrite(10, HIGH);
  //Mantê-los ligados 1 segundo
  delay(1000);
  // Desligar todos os LEDs
  digitalWrite(8, LOW);
  digitalWrite(9, LOW);
  digitalWrite(10, LOW);
  // Mantê-los desligados 1 segundo
  delay(1000);
  // Ligar todo os LEDs
  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
  digitalWrite(10, HIGH);
  // Mantê-los ligados 1 segundo
  delay(1000);
  // Desligar todos os LEDs
  digitalWrite(8, LOW);
  digitalWrite(9, LOW);
  digitalWrite(10, LOW);
}

{% endhighlight %}
