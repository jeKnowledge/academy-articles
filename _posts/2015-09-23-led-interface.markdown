---
layout: post
title:  "Interface LED com Arduino e Processing"
date:   2015-09-23 01:32:13
categories: arduino led processing interface
permalink: interface-led-com-arduino-e-processing
---

Antes de começarmos a falar acerca deste projeto, vou responder ao desafio que tinha deixado no primeiro tutorial. Uma forma fácil de identificar se nos saiu um *double* no lançamento é colocar um LED a acender no caso dos dois números aleatórios serem iguais.

Agora que já resolvemos esse ponto, venho então apresentar-vos este segundo tutorial que é um pouco diferente já que se baseia praticamente todo em software. Apresento-vos então o [Processing](https://processing.org/ "Processing"). Criado no MIT em 2001, o Processing é uma linguagem de programação em contexto visual, isto é, vocês programam o que aparece no ecrã. Basicamente, desenhar com código. No vídeo abaixo, podem ver um exemplo do que se pode fazer com Processing.

<style>.embed-container { margin-top: 20px; margin-bottom: 20px; position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://player.vimeo.com/video/1747316' frameborder='0' webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>

Mas, perguntam vocês, o que tem tudo isto a ver com Arduino? Ora, tudo! Neste projeto, vamos criar uma interface em *Processing* com 3 botões que nos irão permitir controlar os nossos 3 LEDs coloridos. Vamos ainda aprender a enviar informação do Processing para o Arduino! No vídeo abaixo, podem ver o resultado final deste tutorial.

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://player.vimeo.com/video/146688640' frameborder='0' webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>

## Hardware

**Material necessário:** 3 LEDs (um verde, um amarelo e um vermelho), 3 resistências de 220 Ohms e alguns fios de montagem.

Para ajudar à montagem, criou-se o seguinte esquema de ligações usando o programa [Fritzing](http://fritzing.org/home/ "Fritzing").

![]({{ site.baseurl}}/img/esquema.png)

## Introdução ao Processing

Para começarmos, precisamos de fazer download da versão mais recente do *Processing* IDE que neste momento é a versão 3. Para isso, basta aceder a [https://processing.org/](https://processing.org/ "Processing") e fazer download da versão adequada. O que estão a descarregar é um ficheiro .zip da versão portátil do *Processing* (não requer instalação). Para poderem usar o programa, basta descompactá-lo para uma localização à vossa escolha. Logo que acabarem de o fazer, vão encontrar o atalho para o executável do programa e basta fazer duplo clique para o abrir.

Quando abrirem o Processing, provavelmente vão ter a sensação que já o viram algures. Na verdade, o IDE do Arduino é baseado no do Processing e são visualmente e funcionalmente muito semelhantes. Existe, no entanto, uma diferença importante. O IDE do Processing, à medida que vocês escrevem, o IDE relata os erros que vão aparecendo. Muitas vezes isto acontece enquanto vocês ainda estão a escrever o código, por isso, não se preocupem!

![]({{ site.baseurl}}/img/processing.png)

Agora já temos tudo o que precisamos para trabalhar. Quando se abre uma nova janela no *Processing*, esta está em branco. No entanto, e à semelhança do Arduino, em *Processing* temos duas estruturas: `void setup()` e `void draw()` que são em tudo semelhantes ao `void setup()` e ao `void loop()` que já conhecemos do Arduino! A lógica também é transversal: utilizamos o `void setup()` para definir as condições iniciais e o `void draw()` para tudo o que se irá alterar à medida que o programa corre.

O nosso objetivo aqui é criar uma interface com 3 botões nos quais podemos clicar para acender os LEDs que quisermos. O primeiro passo em qualquer programa de *Processing* é criar a janela onde vamos desenhar (no nosso caso, a nossa interface gráfica). Usamos o método `size()` que toma como argumentos o comprimento e a largura da janela em pixeis, nesta mesma ordem.

Devemos também definir a cor do fundo da nossa interface. Neste caso, escolhi o preto mas essa escolha fica a vossa critério. Para representar uma cor em *Processing*, é usado o sistema RGB (Red Green Blue). De certeza que já viram esta sigla. Basicamente, todas as cores são uma qualquer combinação destas três cores base. Por exemplo, o vermelho mais vivo representa-se como (255, 0, 0). Se prestarmos atenção, isto faz sentido. Suprimimos qualquer expressão do Green e do Blue e colocamos o Red no seu valor máximo.

Para evitar confusões, estas combinações são para a cor relacionada com a luz, não com os pigmentos (tintas) que é talvez o que conhecemos melhor. Mas porque é que 255 é o valor máximo e 0 o valor mínimo? Ora o valor de cada cor é guardado numa variável que ocupa 1 byte, ou seja, 8 bits. Sabemos que 2^8 = 256. Como em programação contamos a partir do zero, o valor máximo é então 255 e não 256.

Pensemos agora nos extremos do espetro: o branco e o preto. Ora, sabemos que o preto é a ausência de cor, ou seja, (0, 0, 0). Isto pode ser simplificado, escrevendo apenas (0). Já o branco, é a combinação de todas as cores, logo a sua representação RGB é então (255, 255, 255).

Vamos então criar a nossa janela com fundo preto.

{% highlight c++ %}
void setup()
{
  // Criar uma janela com as dimensões 460x230 e preencher a preto
  size(460, 230);
  background(0);
}
{% endhighlight %}

Se correrem o vosso *sketch*, carregando no botão "Play" vão ver uma janela preta como a representada abaixo.

![]({{ site.baseurl}}/img/janela1.png)

Agora que temos a nossa janela, vamos adicionar uma String de texto que diz "Arduino LED Interface" no fundo da nossa janela. Ora, esta String será branca (ou outra cor qualquer que escolhas), estará alinhada no fundo centrada e estará escrita com tamanho de letra 24 pixeis. Começamos então por definir o tamanho do texto com o método `textSize()` que toma como argumento o tamanho da letra. De seguida, alinhamos o texto no centro com o método `textAlign()` e passamos `CENTER` como argumento do mesmo. Também poderíamos passar como argumentos `LEFT` ou `RIGHT`. De seguida, escolhemos a cor das letras com o método `fill()` passando a cor em formato RGB como argumento e, finalmente, passamos a String com o método `text()` onde passamos como argumento a string e as coordenadas do ponto onde queremos começar a escrever o nosso texto.

Ao bloco de código anterior adicionamos,

{% highlight c++ %}
  // Nome da interface a letras brancas no fundo da janela
  textSize(24);
  textAlign(CENTER);
  fill(255, 255, 255);
  text("Arduino LED Interface", 230, 210);
{% endhighlight %}

![]({{ site.baseurl}}/img/janela2.png)

NOTA: Em *Processing*, as coordenadas têm como origem o campo superior esquerdo da janela criada. Apesar de isto equivaler ao quarto quadrante de um referencial cartesiano, não existem coordenadas negativas.

Temos agora o nome da interface escrito e estamos prontos para avançar para o `void draw()`. Vamos agora criar os botões da interface.

A razão pela qual estes são criados nesta secção tem a haver com o facto destes irem mudar de cor sempre que clicamos nos mesmos. Para criar um botão, desenhamos um rectângulo (neste caso, um quadrado) com o método `rect()`. Começamos por indicar as coordenadas do vértice superior esquerdo e depois o comprimento e a altura do mesmo em pixeís. Portanto, 4 argumentos no total. Devemos ainda especificar a cor que queremos para o botão com o método `fill()` assim como o texto que vai aparecer nele.

O botão verde, o primeiro da nossa sequência, é então criado da seguinte forma:

{% highlight c++ %}
void draw() {

  // Criar um retângulo verde com texto a branco

  // Escolher cor do botão
  fill(0, verde, 0);
  // Desenhar o botão (x, y, comprimento, altura)
  rect(20, 20, 140, 140);
  // Escolher cor do texto do botão (neste caso, branco)
  fill(255, 255, 255);
  // Definir tamanho de letra e alinhamento (não mantém a do void setup(), temos de voltar a defini-los)
  textSize(24);
  textAlign(CENTER);
  // Escrever String "Verde" centrada em (90, 95)
  text("Verde", 90, 95);
}
{% endhighlight %}

Em *Processing*, definimos a cor da forma, neste caso, um retângulo, antes de definirmos a forma em si. Quando chamamos o método `fill()` atribuímos a componente do verde a uma variável do tipo `int` chamada verde (trataremos disso mais tarde). A razão porque fizemos isto será clara rapidamente. Entretanto, podem substituir Verde por 255 e ver um botão verde alface na vossa interface.

Agora já sabemos como criar os restantes botões. Para encontrarem os códigos das cores podem procurar por "RGB color table" no Google e encontram todos os códigos, para todas as cores, possíveis e imaginárias. O código para o amarelo é (255, 255, 0) e o código para o vermelho é (255, 0, 0). Para colocar os botões no sítio certo, basta ir adicionando a coordenada inicial do quadrado anterior com o comprimento do mesmo. Coloquem os códigos das cores por enquanto. Deverão ter algo semelhante à seguinte interface.

![]({{ site.baseurl}}/img/janela3.png)

Podes dar uma vista de olhos ao código que originou esta interface mas não sem antes tentares fazer por ti mesmo!

{% highlight c++ %}
void setup() {
  // Criar uma janela com as dimensões 460x230 e preencher a preto
  size(460, 230);
  background(0);

  // Nome da interface a letras brancas no fundo da janela
  textSize(24);
  textAlign(CENTER);
  fill(255, 255, 255);
  text("Arduino LED Interface", 230, 210);
}

void draw() {

  // Criar um retângulo verde com texto a branco
  fill(0, 255, 0);
  rect(20, 20, 140, 140);
  fill(255, 255, 255);
  textSize(24);
  textAlign(CENTER);
  text("Verde", 90, 95);

  // Criar um retângulo amarelo com texto a preto
  fill(255, 255, 0);
  rect(160, 20, 140, 140);
  fill(0);
  text("Amarelo", 230, 95);

  // Criar um retângulo vermelho com texto a branco
  fill(255, 0, 0);
  rect(300, 20, 140, 140);
  fill(255, 255, 255);
  text("Vermelho", 370, 95);
}
{% endhighlight %}

Ok, temos quase a nossa interface pronta! Só falta que os botões mudem de cor quando clicamos neles, para sabermos se os LEDs estão ligados ou desligados. Para isso, fazemos com que o default da nossa interface seja a versão escurecida dos nossos botões. Para isso basta substituir os 255 por um valor mais baixo, por exemplo, 150. Se fizermos a troca, vamos ver como a interface fica quando os botões estão desligados.

![]({{ site.baseurl}}/img/janela4.png)

Para que a cor dos botões possa mudar, os códigos de cores dos mesmos não podem ser fixos, i.e., têm de ser substituídos por uma variável. Por exemplo, no caso do verde, em vez de `fill(0, 255, 0)` teríamos `fill(0, verde, 0)` onde verde seria uma variável do tipo `int` que alternaria entre 255 e 150. Temos então de criar um conjunto de variáveis que guardem os valores das cores e dar-lhes um valor inicial (150, já que a nossa interface começa com os LEDs todos desligados). Estes devem ser inseridos antes do `void setup()` por uma questão de clareza e organização. Não podem nunca ser introduzidos dentro do `void setup()` ou do `void draw()`.

Teremos então algo do género:

{% highlight c++ %}
int vermelho = 150;
int verde = 150;
int amarelo1 = 150;
int amarelo2 = 150;
{% endhighlight c++ %}

O amarelo tem duas componentes (o vermelho e o verde) daí que tenhamos de ter duas variáveis diferentes.

Agora, como fazemos com que o Processing reconheça um clique? Ora, essa função já está de certa forma implementada. A única coisa que temos de fazer é personalizá-la para que esta faça o que queremos. Este método chama-se `void mouseClicked()` e é definido fora do `void setup()` e do `void draw()`.

Basicamente, queremos que:

1. Seja reconhecido um clique do rato
2. Seja reconhecido um clique na área corresponde a cada botão
3. A cor mude

Tomemos o botão verde como exemplo. Como podemos saber onde é que o ponteiro do rato estava quando fizemos o clique? Felizmente para nós, existem duas variáveis que guardam a posição atualizada do rato em relação ao sistema de coordenadas da janela. Estas variáveis chamam-se `mouseX` e `mouseY` e já vêm implementadas no *Processing*. Para o botão verde, sabemos que, 20 < mouseX < 160 e que 20 < mouseY < 160. Mas como introduzir esta condição no nosso código? Vamos criar 4 condições (duas para cada coordenada) e utilizar o operador `&&` também conhecido como operador AND, isto quer dizer que, a condição só se cumpre se todas as condições forem cumpridas. Assim, temos:

{% highlight c++ %}
void mouseClicked() {
  if (mouseX > 20 && mouseX > 160 && mouseY > 20 && mouseY < 160)
  {
  // Se verde for igual a 150
    // verde igual a 255
  // Se verde for igual a 255
    // verde igual a 150
  }
}
{% endhighlight c++ %}

Além da condição que escrevemos, adicionei um comentário de como fazer o botão mudar de cor. Basicamente, se o botão estiver a verde claro, muda para verde escuro e vice-versa. Já sabem o que fazer, agora basta completarem de acordo com os comentários.

Agora também já sabem como completar o código para os botões amarelo e vermelho. Tenta fazer por ti e vê apenas o exemplo depois. Se tudo correr bem, quando clicares num dos botões, ele muda para a sua cor mais viva e se voltares a clicar, ele muda para a sua cor mais escura.

O código completo será disponibilizado no final do tutorial.

Agora que já temos a nossa interface a funcionar, precisamos de enviar informação do *Processing* para o Arduino. Felizmente, alguém escreveu uma biblioteca (*library* em inglês) que nos permite fazer a comunicação de uma forma relativamente simples. As bibliotecas são uma ferramenta muito importante para um programador e introduzem o conceito de *abstração*. Ou seja, o programador não necessita de saber como as funções disponibilizadas na biblioteca foram escritas, apenas precisa de as saber usar.

## Comunicação Processing Arduino

Vamos agora abrir o Arduino IDE. Queria só alertar que entretanto saiu a versão 1.6.5 e que, para quem tiver o Windows 10 e estiver a ter alguns problemas a executar, basta abrir o programa em Modo de Administrador (clicar com o botão do lado direito do rato e selecionar "Run as Administrator").

Ok, então o que precisamos de programar no nosso Arduino? Primeiro, vamos definir os nossos 3 LEDs como OUTPUTS. Para facilitar, podemos usar variáveis com nomes sugestivos para ser mais fácil identificar os LEDs tais como `ledVermelho`, por exemplo. Mais uma vez, relembro que estas variáveis devem ser criadas fora do `void setup()` e do `void loop()`. Precisamos também de declarar uma variável do tipo `char` (character) que vamos usar para guardar o valor que vamos enviar do *Processing* e que nos vai permitir ligar e desligar os LEDs.

Precisamos ainda de iniciar a comunicação série. No workshop, falei muito brevemente deste tópico mas gostaria de falar um pouco mais sobre ele aqui. Basicamente, o Arduino possui dois pinos que lhe permitem comunicar com o computador ou qualquer outra plataforma ou software. Se olharem com atenção para o vosso Arduino, vão reparar que junto aos pinos digitais 0 e 1, existem duas siglas: TX (transmitir) e RX (receber). Existem, igualmente, dois LEDs com as mesmas siglas localizados na direção do pino digital 13. Ora quando estamos a usar comunicação série, como é o nosso caso, não devemos usar os pinos 0 e 1 já que eles já estão ocupados a comunicar.

Quando estivermos a enviar informação para o Arduino a partir do Processing, vão ver o LED do RX a piscar. Esta é uma boa maneira de nos assegurarmos que está tudo bem com a comunicação.

Para utilizarmos a comunicação série, primeiro precisamos de a iniciar dentro do `void setup()` com o método `Serial.begin()`.

Até agora, do lado do Arduino, temos então o seguinte código:

{% highlight c++ %}
char valor;
int ledVerde = 8;
int ledAmarelo = 9;
int ledVermelho = 10;

void setup() {
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
  Serial.begin(9600);
}
{% endhighlight %}

Os pinos digitais escolhidos estão de acordo com o esquema elétrico apresentado no início deste tutorial.

O método `Serial.begin()` toma como argumento a *baud rate*, isto é, a rapidez da comunicação em bits por segundo. Por convenção, usa-se 9600.

Falta-nos agora escrever, no `void loop()` o que fazer quando o Arduino recebe informação. Quando queremos ler informação que está a ser enviada para o Arduino, temos sempre de usar o método `Serial.available()`. Ou seja,

{% highlight c++ %}
void loop() {
  // Se a comunicação série estiver disponível
  if (Serial.available())
  {
    // Guardar valor lido
    valor = Serial.read();
  }
}
{% endhighlight %}

Agora já temos o valor enviado pelo *Processing* guardado na variável valor. Como se trata de um variável do tipo `char`, só é possível guardar um caracter. Neste caso, usamos números como códigos para representar as 6 ações possíveis. Por exemplo, associamos o número 1 a ligar o LED Verde e o número 2 a desligar o LED Verde. A lista restante segue abaixo:

- [1] Ligar LED verde
- [2] Desligar LED verde
- [3] Ligar LED amarelo
- [4] Desligar LED amarelo
- [5] Ligar LED vermelho
- [6] Desligar LED vermelho

A primeira ação seria escrita como:

{% highlight c++ %}
if (valor == '1')
{
  digitalWrite(ledVerde, HIGH);
}
{% endhighlight %}

NOTA: O número 1 está entre aspas porque, se assim não fosse, o compilador iria assumir o valor ASCII do número 1.

Completa este bloco de código com os restantes cinco casos. Não te esqueças da diferença entre colocar um if seguido de else if's ou só if's. O código completo para o Arduino estará disponível no final deste tutorial.

## Enviar informação do Processing para o Arduino

Agora que já tratámos do nosso script para o Arduino, vamos voltar a abrir a nossa interface e adicionar os parâmetros para a comunicação.

Primeiro, vamos chamar a biblioteca que já tinha mencionado antes com a seguinte linha:

{% highlight c++ %}
import processing.serial.*;
{% endhighlight %}

Esta linha de código importa toda a biblioteca de comunicação série para o nosso sketch de *Processing*. Vês aquele asterisco no final da linha? Isso quer dizer que estamos a importar todos os ficheiros da biblioteca. Existem casos em que não queremos importar todos os ficheiros, então substituímos o asterisco pelo nome do ficheiro desejado.

Agora que já importámos a biblioteca, vamos criar um objeto do tipo Serial. Um objeto é uma espécie de variável cujo tipo nós personalizámos e ao qual damos certos atributos (propriedades). Esta é a base da chamada OOP (Object Oriented Programming) que é um modelo de programação muito comum hoje em dia. Basicamente tudo se faz em OOP. Mas isso fica para outro dia. Por agora, é só um tipo novo de variável que foi criado. Temos então:

{% highlight c++ %}
import processing.serial.*;

// Criar objeto do tipo Serial chamado myPort
Serial myPort;
{% endhighlight %}

Vamos agora definir o objeto myPort dentro do `void setup()`. Para isso adicionamos a seguinte linha:

{% highlight c++ %}
void setup() {
  // Abrir o porto de comunicação
  myPort = new Serial(this, Serial.list()[1], 9600);
}
{% endhighlight %}

O primeiro argumento refere-se ao objeto já criado daí a denominação `this`, o segundo argumento é a porta série que estamos a utilizar (geralmente é a 0 ou a 1). O último parâmetro diz respeito à *Baud Rate* que temos de fazer coincidir com aquela que estabelecemos para o Arduino, ou seja, 9600.

Agora, basta enviar os códigos que já definimos no sketch do Arduino quando clicamos em cada um dos botões. Fazemos isto no `void draw()`. Vamos usar o botão verde como exemplo. Existe uma variável chamada `mousePressed` que é um *boolean* que deteta se houve algum clique do rato.

Vamos ver então o exemplo para o botão verde.

{% highlight c++ %}
// Enviar informacao para o Arduino
  // Premir botao verde

  if (mousePressed && mouseX > 20 && mouseX < 160 && mouseY > 20 && mouseY < 160)
  {
    //Desligar LED verde
    if (verde == 255)
    {
      myPort.write("2");
    }
    //Ligar LED verde
    else
    {
      myPort.write("1");
    }
  }
}
{% endhighlight %}

O que este código faz é o seguinte: se o botão verde for premido e estiver a verde claro, o nosso sketch de *Processing* envia o código 2 para o Arduino que é traduzido como desligar o LED. Se o botão verde for premido mas estiver a verde escuro, então o Arduino recebe o código que corresponde a ligar o LED verde. Agora basta completar o código para os botões amarelo e vermelho.

## Finalmente, vamos testar a nossa interface!

Quando tiveres tanto o código em Processing como o código do Arduino pronto, estamos prontos a testar. Não te esqueças de montar os LEDs de acordo com o esquema elétrico no início deste tutorial! Para pormos a nossa interface a funcionar, começamos por ligar o nosso Arduino ao computador e fazer upload do sketch. Depois de fazermos isto, basta fazer "Play" do nosso sketch de *Processing* e a nossa interface irá aparecer. Se tudo correr bem, quando clicares num dos botões, o LED da cor corresponde irá acender! Se estiveres a obter um erro da parte do *Processing* tem provavelmente a haver com a porta que está definida. Se tiveres 1 coloca um 0 e vice-versa. Abaixo, algumas imagens da interface a funcionar!

![]({{ site.baseurl}}/img/20150912_145232.jpg)

![]({{ site.baseurl}}/img/20150912_145238.jpg)


## Código completo de Processing

{% highlight c++ %}
// Importar biblioteca comunicacao serie
import processing.serial.*;

// Criar objeto do tipo Serial chamado myPort
Serial myPort;

// Definir variaveis que mudam as cores dos botoes
int vermelho = 150;
int verde = 150;
int amarelo1 = 150;
int amarelo2 = 150;

void setup() {
  // Abrir o porto de comunicação
  myPort = new Serial(this, Serial.list()[0], 9600);

  // Criar uma janela com as dimensões 460x230 e preencher a preto
  size(460, 230);
  background(0);

  // Nome da interface a letras brancas no fundo da janela
  textSize(24);
  textAlign(CENTER);
  fill(255, 255, 255);
  text("Arduino LED Interface", 230, 210);
}

void draw() {
  // Criar um retângulo verde com texto a branco
  fill(0, verde, 0);
  rect(20, 20, 140, 140);
  fill(255, 255, 255);
  textSize(24);
  textAlign(CENTER);
  text("Verde", 90, 95);

  // Criar um retângulo amarelo com texto a preto
  fill(amarelo1, amarelo2, 0);
  rect(160, 20, 140, 140);
  fill(0);
  text("Amarelo", 230, 95);

  // Criar um retângulo vermelho com texto a branco
  fill(vermelho, 0, 0);
  rect(300, 20, 140, 140);
  fill(255, 255, 255);
  text("Vermelho", 370, 95);

  // Enviar informacao para o Arduino
  // Premir botao verde
  if (mousePressed && mouseX > 20 && mouseX < 160 && mouseY > 20 && mouseY < 160)
  {
    // Desligar LED verde
    if (verde == 255)
    {
      myPort.write("2");
    }
    // Ligar LED verde
    else
    {
      myPort.write("1");
    }
  }

  // Premir botao amarelo
  else if (mousePressed && mouseX > 160 && mouseX < 300 && mouseY > 20 && mouseY < 160)
  {
    // Desligar LED amarelo
    if (amarelo1 == 255 && amarelo2 == 255)
    {
      myPort.write("4");
    }
    // Ligar LED amarelo
    else
    {
      myPort.write("3");
    }
  }

  // Premir botao vermelho
  else if (mousePressed && mouseX > 300 && mouseX < 440 && mouseY > 20 && mouseY < 160)
  {
    // Desligar LED vermelho
    if (vermelho == 255)
    {
      myPort.write("6");
    }
    // Ligar LED vermelho
    else
    {
      myPort.write("5");
    }
  }
}

void mouseClicked() {
  // Botao verde
  if (mouseX > 20 && mouseX < 160 && mouseY > 20 && mouseY < 160)
  {
    if (verde == 150)
    {
      verde = 255;
    }

    else
    {
      verde = 150;
    }
  }

  // Botao amarelo
  if (mouseX > 160 && mouseX < 300 && mouseY > 20 && mouseY < 160)
  {
    if (amarelo1 == 150 && amarelo2 == 150)
    {
      amarelo1 = 255;
      amarelo2 = 255;
    }

    else
    {
      amarelo1 = 150;
      amarelo2 = 150;
    }
  }

  // Botao vermelho
  if (mouseX > 300 && mouseX < 440 && mouseY > 20 && mouseY < 160)
  {
    if (vermelho == 150)
    {
      vermelho = 255;
    }

    else
    {
      vermelho = 150;
    }
  }
}
{% endhighlight %}

## Código completo Arduino

{% highlight c++ %}
char valor;
int ledVerde = 8;
int ledAmarelo = 9;
int ledVermelho = 10;

void setup() {
  // Definir LEDs como OUTPUTs
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
  // Iniciar comunicacao serie
  Serial.begin(9600);
}

void loop() {
  if (Serial.available())
  {
    // Guardar valor lido e recebido do Processing
    valor = Serial.read();
  }

  // Se o valor recebido for igual a 1, acender o LED verde
  if (valor == '1')
  {
    digitalWrite(ledVerde, HIGH);
  }

  // Se o valor recebido for igual a 2, desligar o LED verde
  else if (valor == '2')
  {
    digitalWrite(ledVerde, LOW);
  }

  // Se o valor recebido for igual a 3, acender o LED amarelo
  else if (valor == '3')
  {
    digitalWrite(ledAmarelo, HIGH);
  }

  // Se o valor recebido for igual a 4, desligar o LED amarelo
  else if (valor == '4')
  {
    digitalWrite(ledAmarelo, LOW);
  }

  // Se o valor recebido for igual a 5, ligar o LED vermelho
  else if (valor == '5')
  {
    digitalWrite(ledVermelho, HIGH);
  }

  // Se o valor recebido for igual a 6, desligar o LED vermelho
  else if (valor == '6')
  {
    digitalWrite(ledVermelho, LOW);  
  }
}
{% endhighlight %}
