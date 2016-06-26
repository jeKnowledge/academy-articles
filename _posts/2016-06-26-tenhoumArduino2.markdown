---
layout: post
title:  "Tenho um Arduino. E agora?! <br> (Parte 2)"
date:   2016-06-26 08:32:13
categories: getting started arduino beginners microcontroller
permalink: arduino-101-parte-2
---

## LED RGB

Após o primeiro contacto com um LED e com o *Blink*, costumo introduzir o LED RGB. O LED RGB é constituído por três LEDs dentro de um só. Quando queremos representar cores digitalmente, usamos o RGB que quer dizer *Red, Green, Blue*, ou seja, Vermelho, Verde e Azul. Apenas com estas três cores, conseguimos representar todas as cores possíveis. 

> FUN FACT: Se quiseres ver o RGB em ação, podes usar três candeeiros, cada um deles coberto com uma película verde, azul ou vermelha e sobrepor a luz de cada um. Podes ver que realmente os nossos olhos não conseguem distinguir as cores como uma mistura de duas ou de três cores fundamentais.

Provavelmente já viste muitos LEDs RGB mas nem sequer reparaste que eram LEDs RGB! Uma das utilizações mais populares hoje em dia são as fitas de LEDs que podem ser controladas com um comando. Estes LEDs são muito compactos para que muitos possam ser colocadas na fita. Já o LED RGB que vamos utilizar nas nossas montagens é um LED do tipo que está na foto.

<!-- Inserir foto LED RGB -->

Já reparaste nos terminais do LED? São quatro e têm diferentes comprimentos exatamente para que possamos saber que terminais corresponde a que LEDs. A razão pela qual existem quatro terminações é porque uma é o LED azul, outra o LED verde, outra o LED vermelho e, finalmente, há um *Ground* comum para os três LEDs. 

## Cores, cores e mais cores

Então e como posso montar o meu LED RGB e começar a representar cores? É bastante simples. Vamos ligar cada um dos LEDs a entradas digitais e o *Ground* ao GRD do Arduino. Neste caso, optámos por ligar o R (Vermelho) ao pino 9, o G (Verde) ao pino 10 e o B (Azul) ao pino 11. Não esquecer as resistências para limitar a corrente de cada um dos LEDs.

<!-- Inserir esquema montagem -->

Se reparares, todos estes pinos no Arduino têm um til ao seu lado. Isto é porque são pinos PWM (*Pulse Width Modulation*). De um modo muito simplista, o PWM permite obter de pinos digitais um comportamento analógico que é o que pretendemos neste caso. Nós queremos controlar a intensidade de cada um dos três LEDs que nos vai fazer obter as várias cores. Ora, para isso vamos utilizar a função `analogWrite()`. Esta função pode-nos induzir em erro pelo seu nome. Apesar de se chamar `analogWrite()`, não está em nada relacionada com os pinos analógicos. Esta função tem como argumento um valor de 0 a 255 o que corresponde a uma resolução de 8 bits (2^8 = 256).

Antes de continuarmos, gostaria só de alertar para um facto que muitas vezes passa despercebido e que origina muita confusão quando se usam estes componentes. Existem dois tipos de LEDs RGB: ânodo comum ou cátodo comum. Os LEDs RGB de cátodo comum são os mais utilizados e é deste tipo que falamos acima e no qual se baseia a nossa montagem. O outro tipo, ânodo comum, é o mesmo componente só que com as alimentações invertidas. Isto quer dizer que o GRD passa a estar ligado aos +5V e que para ligarmos o LED temos de o pôr a LOW e não a HIGH. 

Por exemplo, se quisermos representar a cor vermelha num LED de cátodo comum (que é o que estamos a utilizar), temos de colocar o *Red* a 255, o *Green* a 0 e o *Blue* a 0. No entanto, se estivéssemos a utilizar um LED de ânodo comum, teríamos de fazer exatamente o contrário, ou seja, colocar o *Red* a 0, o "Green" a 255 e o *Blue* a 255. 

Depois de ter tudo montado, o código é, na realidade, muito simples.Este conceito é relativamente simples de entender. O maior problema, muitas vezes, é que não temos qualquer tipo de informação de que LED se trata. Felizmente, os LEDs são Díodos Emissores de Luz. Como um díodo é um componente elétrico que só deixa passar corrente numa direção, se o ligarmos ao contrário não corremos o risco de estragar nada (isto com os +5V do Arduino). Resumindo, se não soubermos, temos de ir por tentativa e erro de modo a descobrir a montagem correta.

Vamos então ao código. Se consultarmos a documentação do Arduino, verificamos que os pinos nos quais vamos usar a função `analogWrite()` não precisam de ser declarados como *outputs* no `void setup()`. Só precisamos de acrescentar no `void loop()` o seguinte:

{% highlight c++ %}
void loop() {
   // Mostra a cor vermelha
  analogWrite(9, 255);
  analogWrite(10, 0);
  analogWrite(11, 0);
}
{% endhighlight %}  

No entanto, este código só mostra vermelho. Se quisermos mostrar outras cores, podemos adicionar outras linhas.

{% highlight c++ %}
void loop() {
   // Mostra a cor vermelha
  analogWrite(9, 255);
  analogWrite(10, 0);
  analogWrite(11, 0);
  delay(1000);

  // Mostra a cor amarela
  analogWrite(9, 255);
  analogWrite(10, 255);
  analogWrite(11, 0);
  delay(1000);

  //Mostra a cor roxa
  analogWrite(9, 80);
  analogWrite(10, 0);
  analogWrite(11, 80);
  delay(1000);
}
{% endhighlight %}  

Este código acima, por exemplo, mostra as cores vermelha, amarela e roxa. Abaixo, podes encontrar algumas das cores RGB mais populares. Para saberes que combinações correspondem a outras cores que queiras representar, podes fazer uma breve pesquisa no Google por *RGB Colour Tables*. Irás encontrar imensas tabelas com os respetivos valores RGB.

| Cor      		                                 | RGB            | 
| :---------------------------------------------:|:--------------:| 
|<span style="color:#FF0000;">Vermelho</span>    | 255, 0, 0      | 
|<span style="color:#00FFFF;">Azul Cian</span>   | 0, 255, 255    |  
|<span style="color:#00FF00;">Verde Alface</span>| 0, 255, 0      | 
|<span style="color:#FF00FF;">Magenta</span>     | 255, 0, 255    | 
|<span style="color:#0000FF;">Azul Escuro</span> | 0, 0, 128      |  
|<span style="color:#008000;">Verde Escuro</span>| 0, 128, 0      |

O código que utilizámos é bastante repetitivo e, para evitar isso, podemos usar funções, que tornam o nosso código muito mais eficiente e compacto. Num dos próximos tutoriais iremos aprender como fazer isso. Até à próxima!    


