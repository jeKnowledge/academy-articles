---
layout: post
title:  "Tenho um Arduino. E agora?! <br> (Parte 2)"
date:   2016-03-31 08:32:13
categories: getting started arduino beginners microcontroller
permalink: arduino-101-parte-2
---

No último artigo, completámos o Blink, o *Hello World* do Arduino. Neste artigo, vamos passar ao segundo projeto, o *Fade*. No *Fade*, vamos controlar a luminosidade de um LED com recurso a um potenciómetro.

## Potenciómetros a.k.a. Resistências Variáveis

Com este projeto, pretende-se dar a conhecer o potenciómetro, também conhecido como resistência variável, como componente eletrónico. O potenciómetro incluído no kit tem um valor bastante elevado, 100K Ohm (potenciómetro mais à direita na imagem). Esta é a resistência máxima que este potenciómetro consegue criar.

Todos os componentes que vemos na imagem são potenciómetros. Este tipo de componente pode ter formas bastante distintas.

![]({{ site.baseurl}}/img/tenhoArduino2/potenciometros.jpg)

Ora, e para que queremos nós uma resistência variável? Para muitas coisas! Se quiseres ligar um ecrã LCD ao teu Arduino e mostrar texto nele, podes conseguir não ver o que lá diz se o Contraste tiver mal ajustado. Para ajustares o Contraste do ecrã, podes usar o potenciómetro que vem incluído na parte de trás do ecrã. Neste caso, o potenciómetro é aquele quadrado azul que pode ser ajustado com uma chave de fendas. Este é o tipo de potenciómetro que está na imagem anterior, no meio dos outros dois.

![]({{ site.baseurl}}/img/tenhoArduino2/ecra.jpg)

Mas, não é tudo! Quando queres mudar o volume do rádio, usas um potenciómetro para o fazer! Os analógicos dos comandos da tua PS4 são basicamente um botão (que te permite carregar no analógico para baixo) e dois potenciómetros: um para o eixo dos XX e outro para o eixo dos YY. Usar potenciómetros faz com que seja possível quantificar o movimento, isto é, se apenas deste um toque ou se empurraste o analógico ao máximo.

O potenciómetro do nosso kit é um potenciómetro de 100K Ohm, ou seja, de uma resistência já bastante elevada. Para ajustar a potência, os potenciómetros têm geralmente um veio que podemos rodar. Os nossos não têm um veio mas têm uma ranhura que podemos rodar com a ajuda de uma chave de fendas. Existem ainda dois tipos de potenciómetros. Os potenciómetros tipo A variam a resistência de modo linear enquanto que os do tipo B o fazem de um modo logarítmico. O nosso potenciómetro é do tipo A.

## Como funciona um potenciómetro?

Um potenciómetro funciona como um divisor de tensão. Um quê?! Um divisor de tensão! Este é um dos circuitos mais utilizados em eletrónica e basicamente o que faz é devolver na saída uma fração da tensão introduzida. Mas um divisor de tensão permite-nos fazer muito mais do que isto. Outro uso muito comum deste tipo de montagem prende-se com a conversão de uma variação de resistência numa variação de diferença de potencial, que é a grandeza que o nosso Arduino mede. Vamos a um pequeno exemplo que torna tudo mais fácil. Um divisor de tensão é composto por duas resistências como podemos ver na imagem abaixo.

![]({{ site.baseurl}}/img/tenhoArduino2/Voltage_Divider.png)

Como podemos ver, as resistências, R1 e R2, têm o mesmo valor, 100 Ohm. Vamos supor que a tensão de entrada é +5V, como se se do nosso Arduino se tratasse. Existe uma fórmula que nos estabelece uma relação entre o valor de cada uma das resistências e o Vout.

![]({{ site.baseurl}}/img/tenhoArduino2/formula.png)

Vamos então substituir os valores e ver o que obtemos:

![]({{ site.baseurl}}/img/tenhoArduino2/formula2.png)

A tensão inicial Vin, de +5V, passou para +2.5V. Se aumentássemos o valor de R2, obteríamos uma fração ainda maior da tensão inicial.

Um potenciómetro funciona com base neste princípio. Existe uma resistência contínua que é separada por um contacto que está ligado ao veio que rodamos. Assim, acabamos por ter também duas resistências, R1 e R2, exatamente como no nosso exemplo cujo valor varia.

## Montar o circuito do *Fade*

Um potenciómetro tem três ligações. Um é o vosso Vin, outro o GROUND e, finalmente, o Vout, que ligamos a um pin analógico do nosso Arduino. No nosso caso, o pinout é o seguinte:

![]({{ site.baseurl}}/img/tenhoArduino2/pinout.jpg)

Fazer as ligações ao nosso Arduino é relativamente fácil. Basta seguires o esquema abaixo,

![]({{ site.baseurl}}/img/tenhoArduino2/esquema.png)

Se tiveres dificuldade em determinar o posicionamento do potenciómetro, vira-o para ti. Se tiveres duas ligações à frente e uma a trás é porque o tens bem posicionado.

## Programar o *Fade*

Este é um programa bastante simples em termos de código. A primeira coisa a fazer é definir o pin A0 como um INPUT, já que vamos estar a ler a diferença de potencial. Já sabemos que isto se faz no `void setup()`.

{% highlight c++ %}
void setup() {
  // Definir pin analógico que lê o valor da diferença de potencial
  pinMode(A0, INPUT);
}
{% endhighlight %}

De seguida, vamos inicializar o *Serial Monitor*. O *Serial Monitor* é uma maneira de termos acesso aos dados que estão a ser lidos pelo Arduino. Ou seja, é através do *Serial Monitor* que vamos ver a variação do valor a ser lido à medida que vamos rodando o potenciómetro.

Para iniciar o *Serial Monitor*, fazemos:

{% highlight c++ %}
void setup() {
  // Definir pin analógico que lê o valor da diferença de potencial
  pinMode(A0, INPUT);
  // Inicializar a comunicação série
  Serial.begin(9600);
}
{% endhighlight %}

O argumento que passamos à função `Serial.begin()` é o chamado *baud rate*. Ou seja, se o *baud rate* for de 9600, isto quer dizer que serão transferidos, no máximo, 9600 bits por segundo. Este não é o *baud rate* máximo do Arduino. É apenas o valor usualmente utilizado.

Finalmente, apenas temos de especificar o que queremos que apareça no nosso *Serial Monitor*. Para isso, fazemos um *print* do valor. Em programação, um *print* é "imprimir" um valor para o terminal, para o podermos ver. Para isto acontecer, escrevemos:

{% highlight c++ %}
void loop() {
  // Fazer print para o Serial Monitor do valor lido em A0
  Serial.println(analogRead(A0));
}
{% endhighlight %}

E estamos prontos!

## *Fade* em ação

Liga o teu Arduino ao computador e faz *upload* do programa que acabaste de escrever. Liga o *Serial Monitor* carregando no botão com a lupa, na barra superior direita do IDE.

![]({{ site.baseurl}}/img/tenhoArduino2/serialButton.png)

Vais ver uma grande quantidade de valores, todos iguais, a serem mostrados.

![]({{ site.baseurl}}/img/tenhoArduino2/serial1.png)

Agora, vamos rodar o potenciómetro, com a ajuda de uma chave de fendas, e ver o que acontece.

![]({{ site.baseurl}}/img/tenhoArduino2/adjust.jpg)

No nosso *Serial Monitor* vemos agora que os valores mudaram.

![]({{ site.baseurl}}/img/tenhoArduino2/serial2.png)

Mas o *Fade* não termina por aqui. Afinal de contas, ver números a mudar não é assim tão interessante! O que vamos fazer agora é ligar um LED e controlar a sua intensidade com a ajuda do potenciómetro. O circuito passa agora então a ser o seguinte,



Ou seja,

{% highlight c++ %}
void setup() {
  pinMode(13, OUTPUT);
}
{% endhighlight %}

Agora que já definimos a função do pino, resta-nos configurar o comportamento do LED. Para que ele pisque, então tem de acender e apagar. Vamos começar por acender o nosso LED com a função também já pré-definida `digitalWrite()`. Esta função permite-nos alterar o estado de um pino digital. Ou seja, dar-lhe o valor de 0 ou 1. Ou, no caso desta função, LOW ou HIGH. Esta função também possui dois argumentos: o número do pino a que se refere a instrução e se o pino passa a HIGH ou LOW.

Logo, para acendermos o LED escrevemos:

{% highlight c++ %}
void setup() {
  pinMode(13, OUTPUT);
}

void loop() {
	digitalWrite(13, HIGH);
}
{% endhighlight %}

No entanto, como sabemos, o nosso microcontrolador processa esta informação muito rapidamente. Por isso, devemos especificar quanto tempo queremos que o nosso LED fique acesso. Para isso, vamos usar uma outra função pré-definida, o `delay()`. Esta função aguarda o tempo que lhe passarmos como argumento até executar a próxima linha de código. O tempo é dado em milisegundos. Ou seja, se quisermos que o LED esteja 1 segundo ligado, devemos passar 1000 como argumento. Temos então,

{% highlight c++ %}
void setup() {
  pinMode(13, OUTPUT);
}

void loop() {
	digitalWrite(13, HIGH);
	delay(1000);
}
{% endhighlight %}

Agora que programámos o LED para estar aceso durante 1 segundo, queremos também que ele esteja apagado durante 1 segundo, ou qualquer outra duração temporal. Adicionando estas duas linhas ficamos com o nosso programa completo:

{% highlight c++ %}
void setup() {
  pinMode(13, OUTPUT);
}

void loop() {
	digitalWrite(13, HIGH);
	delay(1000);
	digitalWrite(13, LOW);
	delay(1000);
}
{% endhighlight %}

Podemos verificar se o teu código compila corretamente carregando no primeiro botão da Barra de Ferramentas do Arduino IDE e, de seguida, usando o segundo botão para fazermos download do programa para o Arduino.

![]({{ site.baseurl}}/img/tenhoArduino/botoes.PNG)

Se tudo tiver corrido bem, tens agora o teu LED a piscar com a frequência que definiste! Parabéns por teres concluído o primeiro tutorial de Arduino. Na continuação deste tutorial, iremos ter vários pequenos projetos que fazem uso de outros componentes do nosso kit.

Até à próxima <span style="display: inline-block; margin-left: 2px; margin-right: 0px">:smile:</span>
