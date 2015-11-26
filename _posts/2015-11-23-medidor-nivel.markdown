---
layout: post
title:  "Medidor de Nível com Arduino e Processing"
date:   2015-11-23 18:32:13
categories: arduino processing sensing water interface
---

Neste tutorial, vamos fazer um medidor de nível que mostra a informação em tempo real através de uma interface em Processing. Recomenda-se que já tenham feito o tutorial "Interface LED com Arduino e Processing", uma vez que não irei explicar tão pormenorizadamente certos aspetos relacionados com o Processing. Podes ver como irá ser a nossa interface, e o que nos propomos a fazer neste tutorial, no vídeo abaixo!

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://player.vimeo.com/video/146691549' frameborder='0' webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>

## Hardware

**Material necessário:** 4 resistências de 820 ou 1K Ohms, 1 garrafa de água de 1.5L, tesoura, copo medidor ou copo de plástico, caneta de acetato, patafix e alguns fios de montagem (longos).

## Preparação

Este projeto envolve um pouco de bricolage. Vamos começar por cortar a nossa garrafa de 1.5L como se mostra na imagem.

![]({{ site.baseurl}}/img/monitorNivel/garrafaCortada.jpg)

Com um copo medidor, ou se não tiveres um, um copo de plástico de 0.2L (dos mais comuns que geralmente toda a gente tem em casa), adiciona água à garrafa já cortada. Se tiveres um copo medidor então enche até aos 0.2L, se tiveres um copo de plástico, enche mesmo até ao topo. Depois de fazeres isto, faz uma marcação com a caneta de acetato a dizer 0.2. Isto ajuda-nos a colocar os fios, posteriormente, e a ter uma indicação visual da capacidade do recipiente.

![]({{ site.baseurl}}/img/monitorNivel/garrafaCopo.jpg)

Vai adicionando água para fazeres as marcações dos 0.4L, dos 0.6L e dos 0.8L. Quando tiveres acabado, é tempo de começar a colocar os fios! Mas antes, um pequeno esclarecimento.

## Como é que o nosso medidor de nível funciona? 

O nosso medidor de nível irá ser capaz de detetar cinco estados diferentes: vazio, 0.2L, 0.4L, 0.6L e 0.8L. Este medidor é um modelo em pequena escala de um medidor de nível de varetas que é bastante usado a nível industrial. Este medidor não nos diz exatamente a capacidade contida no recipiente, mas é capaz de nos dar uma ideia bastante razoável do líquido que contém, na forma de um intervalo.

Assim, se o nosso medidor mostrar 0.4L, então sabemos que o medidor contém entre 0.4L e 0.6L, exclusive. Tudo isto funciona porque a água conduz eletricidade devido às caraterísticas polares das moléculas que a compõem. Iremos colocar um fio no fundo do recipiente a fornecer uma tensão de +5V. Depois, teremos quatro fios colados à parede do recipiente, em cada uma das marcações. Estes fios serão _INPUTS_, logo o seu estado estará a ser lido, no nosso caso, de 3 em 3 segundos. Esse estado será depois enviado pelo Arduino para o nosso programa de Processing que nos irá mostrar uma representação gráfica do nível de água na garrafa.

## Fazer as ligações!

Agora que já entendemos o que se vai passar e que já temos suporte para montar os nossos fios, vamos a isso! Vamos precisar de 5 fios de ligação relativamente compridos (30 a 50 cm cada um) para podermos colocá-los dentro do nosso recipiente e ligá-los à breadboard. Para isso, dobra a extremidade de cada fio, num ângulo de 90º, para que fique perfeitamente alinhado com a marcação. Cola a extremidade do fio com patafix, que acaba por ser a melhor forma de fixação, já que é bastante resistente à água. Quando acabares, deves ter 4 fios de um dos lados, em cada uma das marcações, e um quinto fio até ao fundo que irá debitar os +5 Volts.

![]({{ site.baseurl}}/img/monitorNivel/monitor4.jpg) 

Começamos então por ligar o nosso fio solto aos +5V do Arduino e um outro fio ao Ground.

Como as portas analógicas estão sujeitas a muito ruído (medições que não nos interessam), vamos usar quatro resistências *pull-down*. Estas resistências filtram o sinal lido, e removem este ruído indesejado. Podem fazer a experiência: Se ligarem diretamente os fios do medidor às portas analógicas, não vão obter nenhum resultado com o qual possam trabalhar. No entanto, se usarem as resistências deste modo, vão obter 0 ou um valor semelhante quando não tiverem água e um valor acima de 5 ou 10, quando tiverem.

Podem ver como montar as resistências *pull-down* abaixo.

![]({{ site.baseurl}}/img/monitorNivel/monitor1.jpg) 

![]({{ site.baseurl}}/img/monitorNivel/monitor2.jpg) 

Não te esqueças de ligar as alimentações!

![]({{ site.baseurl}}/img/monitorNivel/monitor4.jpg) 

E estamos prontos para começar a programar!

## Programar o Arduino

Esta é, talvez, a parte mais fácil e direta deste projeto. Começamos então por definir os pinos que vamos utilizar e a sua função, assim como iniciar a comunicação série. Precisamos da comunicação série, porque vamos estar a enviar dados do Arduino para o Processing.

{% highlight c++ %}
void setup() {
  pinMode(A0, INPUT);
  pinMode(A2, INPUT);
  pinMode(A3, INPUT);
  pinMode(A5, INPUT);
  Serial.begin(9600);
}
{% endhighlight %}

Uma vez que vamos obter valores analógicos, não digitais, vamos precisar de os converter para booleanos, isto é, `true` ou `false`. E tudo isto porque temos dois estados possíveis, com ou sem água. Começamos por criar, então, quatro variáveis do tipo `boolean`.

> FUN FACT: Estas variáveis têm o nome de *boolean* em homenagem ao matemático George Boole que inventou a lógica booleana, extremamente útil na computação.

 {% highlight c++ %}
void loop() {
  boolean leitura1 = false;
  boolean leitura2 = false;
  boolean leitura3 = false;
  boolean leitura4 = false;
}
{% endhighlight %}

A razão pela qual estas variáveis são inicializadas a `false` é porque, só se houver água, é que estas passam a `true`. A próxima secção de código pretende exatamente fazer isso.

Portanto, se houver água, a variável passa a `true`. Se não houver, mantém-se a `false`. Para isto, temos de criar uma condição onde, a partir de uma leitura, seja feita esta decisão.

Nesta parte, recomenda-se que seja feita uma pequena experiência, fazendo print dos valores lidos pelos pinos analógicos no *Serial Monitor*. Aquando do teste, obteve-se 0 quando não existia água e valores acima de 15 quando havia. Mas sistemas diferentes podem ter leituras diferentes, por isso, é recomendável que se faça este tipo de calibração.

No setup utilizado, optou-se por usar a condição `if (analogRead(A0) > 5)` para todos os pinos. Se esta condição se verificar, então a variável passa a true.

 {% highlight c++ %}
void loop() {
    boolean leitura1 = false;
    boolean leitura2 = false;
    boolean leitura3 = false;
    boolean leitura4 = false;

    if (analogRead(A0) > 5)
    {
      leitura1 = true;
    }
}
{% endhighlight %}
 
Completa os outros três casos para as restantes três variáveis! A vantagem deste tipo de estrutura é que a cada iteração do loop, as variáveis voltam a ter o valor inicial de `false`.

> PRO TIP: Se o teu código começar a ficar com uma indentação estranha 
(espaçamento das várias secções de código), prime Ctrl+T e o IDE do Arduino organiza o teu código por ti!

Depois disto, o teu `void loop()` deve ser semelhante ao código que se segue.

 {% highlight c++ %}
  void loop() {

    boolean leitura1 = false;
    boolean leitura2 = false;
    boolean leitura3 = false;
    boolean leitura4 = false;

    if (analogRead(A0) > 5)
    {
      leitura1 = true;
    }

    if (analogRead(A2) > 5)
    {
      leitura2 = true;
    }

    if (analogRead(A3) > 5)
    {
      leitura3 = true;
    }

    if (analogRead(A5) > 5)
    {
      leitura4 = true;
    }
{% endhighlight %}

E estamos quase a terminar! Já só falta determinarmos os cinco estados possíveis do nosso medidor. Vamos fazer isso através de condições `if` e de operadores `&&` (AND).

Por exemplo, para o nível 4 que corresponde aos 0.8L, ou seja, todas as leituras devem estar a positivo, fazemos:

{% highlight c++ %}
if (leitura4 && leitura3 && leitura2 && leitura1)
{
  Serial.println('4');
} 
{% endhighlight %}

Ou seja, se `leitura4`, `leitura3`, `leitura2` e `leitura1` forem `true`, então a condição é satisfeita e o Arduino faz print do número 4 para o *Serial Monitor*. É isto que o operador `&&` faz. Se todas as condições forem cumpridas, então o resultado total é `true`. Bastaria que uma das leituras fosse `false` para a condição não se verificar.

Agora podes completar o resto do código para os estados 3, 2, 1 e 0 (vazio). Para estes estados, podes usar condições `else if()` uma vez que só vamos querer selecionar um deles. No fim, vamos ainda acrescentar um delay de 3 segundos que ajuda a que os valores sejam enviados na ordem correta. Para além disto, 3 segundos é mais do que suficiente para se verificar qual é o novo nível.

Podes encontrar o código para o Arduino completo no final deste tutorial, para se quiseres comparar com o teu. Se tudo tiver corrido bem, quando abrires o Serial Monitor, deves obter um 0, se não tiveres água, ou um outro número, de 1 a 4, consoante o nível que tenhas.

Da parte do Arduino está tudo, vamos agora avançar para o Processing!

## Desenvolver a interface em Processing

Agora que o nosso Arduino "já sabe" o nível de água dentro do nosso recipiente, vamos enviar esta informação para o Processing e esperar que a nossa interface nos devolva uma representação gráfica do nível de água no recipiente.

Abrimos um novo *sketch* de Processing e começamos por preencher o `void setup()`. 

Começamos por criar uma janela com a instrução `size(width, height)`. Neste caso, desenhámos uma janela com as dimensões 450x650 pixeis. Vamos ainda adicionar os atributos do texto, ou seja, como queremos que este seja mostrado. Isto inclui tamanho e alinhamento. Vamos ainda adicionar uma função, `smooth()` que toma como argumento um inteiro e que serve para esbater as fronteiras das formas. Vai nos ser útil quando quisermos mostrar o indicador de *status* do medidor.

Temos assim,

{% highlight c++ %}
void setup()
{
  //Criar janela
  size(450, 650);
  // Definir atributos do texto
  textAlign(CENTER);
  textSize(26);
  // Esbater os contornos das formas 
  smooth(2);
}
{% endhighlight %}

Seguimos agora para o `void draw()`. É aqui que vamos desenhar o nosso medidor de nível. Começamos por selecionar um fundo preto. Esta é a primeira instrução e serve para que, a cada iteração do loop, tudo seja apagado e desenhado novamente para evitar que as formas se sobreponham. 

Além do nosso fundo preto, adicionamos ainda o título da nossa interface "Arduino Level Monitor" assim como a designação de "Status" do lado direito. Este fornecerá uma indicação na forma de cor, que vai de vermelho a verde, acerca do nível medido.

Temos então, 

{% highlight c++ %}
void draw() {

  // Atualizar o background a cada iteração para nao ocorrer overwrite
  background(0);
  // Escrever Arduino Level Monitor a branco, no topo
  fill(255, 255, 255);
  text("Arduino Level Monitor", 225, 50);
  // Escrever "Status"
  text("Status", 350, 270);
}
{% endhighlight %} 

Vamos agora adicionar a representação do nosso medidor que acaba por ser, somente, um retângulo com um fundo azul.

{% highlight c++ %}

  // Criacao do medidor
  fill(135, 206, 250);
  rect(50, 80, 200, 490);

{% endhighlight %}

Depois disto, queremos adicionar as marcas e as indicações que nos indicam onde fica cada capacidade. Para adicionarmos as marcas, usamos a função `line ()` onde especificamos as coordenadas iniciais e as coordenadas finais dos dois pontos que constituem o segmento de reta. Vamos ainda usar as funções `stroke()` e `strokeWeight()`. A primeira, recebe como argumento a cor da linha e a segunda recebe a espessura da linha. No nosso caso, selecionámos preto para a primeira `stroke(0)` e `strokeWeight(4)` para a segunda.

No final, não nos podemos esquecer de chamar a função `noStroke()`. Se não o fizermos, todas as linhas terão mais espessura. 

{% highlight c++ %}

  // Marcacoes no medidor
  fill(0);
  text("0.2 l", 150, 480);
  text("0.4 l", 150, 380);
  text("0.6 l", 150, 280);
  text("0.8 l", 150, 180);

  // Linhas de marcacao no medidor, contornos a preto, mais grossos
  fill(0);
  stroke(0);
  strokeWeight(4);
  
  // Linhas da esquerda
  line(60, 170, 100, 170);
  line(60, 270, 100, 270);
  line(60, 370, 100, 370);
  line(60, 470, 100, 470);
  
  // Linhas da direita
  line(200, 170, 240, 170);
  line(200, 270, 240, 270);
  line(200, 370, 240, 370);
  line(200, 470, 240, 470);
  // Fechar o stroke
  noStroke();

{% endhighlight %}

Adicionamos ainda um delay de 3 segundos, como fizemos no Arduino.

{% highlight c++ %}
  // Esperar 3 segundos
  delay(3000);
{% endhighlight %}

Vamos agora programar a comunicação série. Vamos voltar a usar a biblioteca Serial do Processing que nos facilita o envio de dados do Arduino para o Processing. Como este tópico já foi abordado no Tutorial da Interface LED, não vou estar a repetir o que já foi dito sobre isto.

Basicamente, antes do `void setup()`, começamos por importar a biblioteca.

{% highlight c++ %}
  // Importar biblioteca serie
  import processing.serial.*;
{% endhighlight %}

Abaixo desta linha, criamos o objeto série minha Porta. Ficamos então com,

{% highlight c++ %}
  // Importar biblioteca serie
  import processing.serial.*;
  // Criar objeto serie
  Serial minhaPorta;
{% endhighlight %}

Feito isto, vamos adicionar no `void setup()`, alguns parâmetros adicionais para configurar a nossa comunicação série.

{% highlight c++ %}
  // Definir comunicacao serie  
  String nomePorta = Serial.list()[0];
  minhaPorta = new Serial(this, nomePorta, 9600);
{% endhighlight %}

Vamos agora escrever a função `void serialEvent (Serial myPort)`que recebe como argumento um objeto série como o que criámos antes do `void setup()`. Esta função descreve o que acontece quando um evento série é registado, ou seja, quando o Processing se encontra a receber informação.

É nesta função que escrevemos o que queremos que apareça em função do número recebido pelo Processing e que o Arduino está a enviar.

Vamos começar por criar uma variável do tipo `char` que nos vai guardar o valor que estará a ser enviado pela porta série. Reparem que quando programámos o Arduino, enviámos também uma variável do tipo `char`. Daí termos definido `Serial.println('0');`, por exemplo, onde `'0'` é um char por estar dentro de `''`. Se estivesse dentro de aspas, seria uma `String` e não um `char`. Aqui não faz sentido usar uma `String`, já que o número a enviar cabe perfeitamente numa variável do tipo `char`. Se quiséssemos enviar uma palavra ou uma frase, aí sim, teríamos de usar uma variável do tipo `String`.

{% highlight c++ %}
void serialEvent (Serial myPort)
{
  // Guardar valor na porta serie numa variavel char
  char medida = myPort.readChar();
}
{% endhighlight %}

Vamos agora definir uma condição para cada estado: 0, 1, 2, 3 e 4. Fazemos isto através de uma série de condições `if()`. Para o caso em que a variável *medida* tenha o valor 1, temos:

{% highlight c++ %}
void serialEvent (Serial myPort)
{
  // Guardar valor na porta serie numa variavel char
  char medida = myPort.readChar();

  if (medida == '1')
  {
    // Desenhar agua ate aos 0.2L
    fill(30, 144, 255, 100);
    rect(50, 570, 200, -100);
    // Desenhar circulo vermelho
    fill(255, 0, 0);
    ellipse(350, 320, 40, 40);
  }  
{% endhighlight %}

Logo quando o Arduino estiver a enviar um 1, então sabemos que a marca dos 0.2L já foi ultrapassada. Para mostrar isto, colocamos o botão do status a vermelho e mostramos um retângulo a tocar nos 0.2L, que é o nível que temos absoluta certeza de que está preenchido. Existem, aqui, dois pormenores importantes. Por um lado, o `fill()` do retângulo tem quatro argumentos e, por outro, existe uma coordenada negativa no `rect()`. Em relação à primeira questão, o quarto valor é a opacidade, que quisemos adicionar, neste caso, porque queríamos continuar a ver as marcas do mostrador. O valor da opacidade vai de 0 a 255 e podes alterá-lo para ver como isso influencia a representação da água no medidor. Já a questão das coordenadas negativas, tem a haver com o facto de, querermos que o nosso retângulo "cresça" de baixo para cima. E para fazer isto, a única alternativa é usar uma coordenada negativa para a altura do retângulo que se comporta ao contrário do que seria habitual.

Ainda não podes correr o teu código, mas se tudo correr bem no final, para 0.2L a representação será esta.

![]({{ site.baseurl}}/img/monitorNivel/level1.PNG) 

Agora que já sabes como fazer, basta preencheres o código que falta para os estados 2, 3 e 4. Para o indicador de status, podes usar verde para o estado 4 (0.8L), amarelo para o estado 3 (0.6L) e laranja para o estado 2 (0.4L). Repara que para desenharmos um círculo, usamos a função `ellipse()` já que uma circunferência é uma elipse cuja altura e largura são iguais. Não te esqueças de usar `else if()` para exprimires as outras condições. Afinal de contas, só queres escolher um dos estados.

Para o estado 0, optou-se por uma interface um pouco diferente. Irá aparecer um ponto de interrogação no lugar do círculo debaixo do *Status* e uma inscrição a dizer "NO LEVEL DETECTED!" na parte de baixo. Aqui fica o código referente a essa parte. 

{% highlight c++ %}
void serialEvent (Serial myPort)
{
  else if (medida == '0')
  {
    // Escrever a amarelo torrado que o nivel nao foi detetado
    fill(255, 255, 0);
    text("NO LEVEL DETECTED!", 225, 620);
    // Ponto de interrogacao no status
    text("?", 350, 320);
  }  
}  
{% endhighlight %}

Deves visualizar algo deste género quando tiveres tudo pronto.

![]({{ site.baseurl}}/img/monitorNivel/nolevel.PNG) 

Podes ver como fica a função `void serialEvent()` no final deste tutorial. E pronto, estamos quase no fim! Agora que já escrevemos toda a função `void serialEvent()`, basta chamá-la para o objeto que criámos inicialmente.

Portanto, dentro do `void draw()`, adicionamos quase no fim, mas antes do *delay*, a seguinte condição.

{% highlight c++ %}
void draw()
{
  // Se estiver a ser enviado algo pela porta serie..
  while (minhaPorta.available() > 0) {  
    serialEvent(minhaPorta);
  }
}  
{% endhighlight %}

Ou seja, enquanto o Processing estiver a receber dados, chama a função `void serialEvent()`. E é isto!

Verifica se o teu código está conforme o que eu disponibilizo abaixo e vamos pôr as coisas a funcionar! Faz upload do programa para o teu Arduino. Podes ver se está tudo a funcionar bem abrindo o *Serial Monitor*. Se estiveres a receber os códigos corretos, ótimo! Fecha o *Serial Monitor* (isto porque, se o deixares aberto, o Processing vai-te dizer que a porta já está a ser utilizada, o que é bem verdade!), abre o Processing e mete o *sketch* a correr!

Esta interface pode não parecer muito útil mas a verdade é que é possível enviar os dados pela Internet e teres acesso à interface em qualquer parte do mundo. Infelizmente, não temos o material para fazer isso, mas fica a saber que sim, é possível!

## Código do Arduino

{% highlight c++ %}
void setup() {
  // Definir pinos e iniciar a comunicacao serie
  pinMode(A0, INPUT);
  pinMode(A2, INPUT);
  pinMode(A3, INPUT);
  pinMode(A5, INPUT);
  Serial.begin(9600);
}

void loop() {
  // Criar quatro variáveis booleanas e inicia-las a false
  boolean leitura1 = false;
  boolean leitura2 = false;
  boolean leitura3 = false;
  boolean leitura4 = false;

  // Verificar se foi detetado nivel ou nao
  if (analogRead(A0) > 5)
  {
    leitura1 = true;
  }

  if (analogRead(A2) > 5)
  {
    leitura2 = true;
  }

  if (analogRead(A3) > 5)
  {
    leitura3 = true;
  }

  if (analogRead(A5) > 5)
  {
    leitura4 = true;
  }

  // Atribuir um codigo, referente ao nivel, consoante os booleans
  if (leitura4 && leitura3 && leitura2 && leitura1)
  {
    Serial.println('4');
  }

  else if (leitura3 && leitura2 && leitura1)
  {
    Serial.println('3');
  }

  else if (leitura2 && leitura1)
  {
    Serial.println('2');
  }

  else if (leitura1)
  {
    Serial.println('1');
  }

  else
  {
    Serial.println('0');
  }

  // Esperar 3 segundos
  delay(3000);
}
{% endhighlight %}

## Código Processing

{% highlight c++ %}
// Importar biblioteca serie
import processing.serial.*;
// Criar objeto serie
Serial minhaPorta;

void setup()
{
  //Criar janela
  size(450, 650);
  // Definir atributos do texto
  textAlign(CENTER);
  textSize(26);
  // Definir comunicacao serie  
  String nomePorta = Serial.list()[0];
  minhaPorta = new Serial(this, nomePorta, 9600);
  // Esbater os contornos das formas 
  smooth(2);
}
  
void serialEvent (Serial myPort)
{
  // Guardar valor na porta serie numa variavel char
  char medida = myPort.readChar();

  if (medida == '1')
  {
    // Desenhar agua ate aos 0.2L
    fill(30, 144, 255, 100);
    rect(50, 570, 200, -100);
    // Desenhar circulo vermelho
    fill(255, 0, 0);
    ellipse(350, 320, 40, 40);
  } 
  
  else if (medida == '2')
  {
    // Desenhar agua ate aos 0.4L
    fill(30, 144, 255, 100);
    rect(50, 570, 200, -200);
    // Desenhar circulo amarelo torrado
    fill(255, 165, 0);
    ellipse(350, 320, 40, 40);
  } 
  
  else if (medida == '3')
  {
    // Desenhar agua ate aos 0.6L
    fill(30, 144, 255, 100);
    rect(50, 570, 200, -300);
    // Desenhar circulo amarelo
    fill(255, 255, 0);
    ellipse(350, 320, 40, 40);
  } 
  
  else if (medida == '4')
  {
    // Desenhar agua ate aos 0.8L
    fill(30, 144, 255, 100);
    rect(50, 570, 200, -400);
    // Desenhar circulo verde
    fill(0, 255, 0);
    ellipse(350, 320, 40, 40);
  } 
  
  else if (medida == '0')
  {
    // Escrever a amarelo torrado que o nivel nao foi detetado
    fill(255, 255, 0);
    text("NO LEVEL DETECTED!", 225, 620);
    // Ponto de interrogacao no status
    text("?", 350, 320);
  }
}
 
void draw() {

  // Atualizar o background a cada iteração para nao ocorrer overwrite
  background(0);
  // Escrever Arduino Level Monitor a branco, no topo
  fill(255, 255, 255);
  text("Arduino Level Monitor", 225, 50);
  // Escrever "Status"
  text("Status", 350, 270);

  // Criacao do medidor
  fill(135, 206, 250);
  rect(50, 80, 200, 490);

  // Marcacoes no medidor
  fill(0);
  text("0.2 l", 150, 480);
  text("0.4 l", 150, 380);
  text("0.6 l", 150, 280);
  text("0.8 l", 150, 180);

  // Linhas de marcacao no medidor, contornos a preto, mais grossos
  fill(0);
  stroke(0);
  strokeWeight(4);
  
  // Linhas da esquerda
  line(60, 170, 100, 170);
  line(60, 270, 100, 270);
  line(60, 370, 100, 370);
  line(60, 470, 100, 470);
  
  // Linhas da direita
  line(200, 170, 240, 170);
  line(200, 270, 240, 270);
  line(200, 370, 240, 370);
  line(200, 470, 240, 470);
  // Fechar o stroke
  noStroke();
  
  // Se estiver a ser enviado algo pela porta serie..
  while (minhaPorta.available() > 0) {  
    serialEvent(minhaPorta);
  }
  
  // Esperar 3 segundos
  delay(3000);
}
{% endhighlight %}
