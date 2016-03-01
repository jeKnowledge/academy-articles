---
layout: post
title:  "Tenho um Arduino. E agora?! <br> (Parte 1)"
date:   2016-03-01 08:32:13
categories: getting started arduino beginners microcontroller
---

Este é o primeiro de uma série de três artigos que explica os princípios básicos do Arduino. Uma das coisas que mais ouço quando se fala de Arduino é "Ah, por acaso até tenho um, mas não sei trabalhar com ele...". Vamos então começar pelo mais básico. Afinal, o que é um Arduino?

## Arduino 101

O Arduino é uma plataforma de prototipagem baseada num microcontrolador. Com um Arduino é possível fazer inúmeros projetos de eletrónica de uma forma muito barata e rápida. Por esta razão, esta é a plataforma de eleição tanto para engenheiros, como para designers, estudantes ou meros entusiastas. A filosofia do Arduino baseia-se numa plataforma *open source* (o que significa que toda a gente pode ter acesso aos esquemáticos e ajudar no seu desenvolvimento), baseada em software e hardware simples, que toda a gente pode usar.

É por esta razão que o Arduino é, a meu ver, ideal para quem se está a iniciar na programação. Isto deve-se, essencialmente, a duas vertentes. Primeiro, os alunos interagem com o *hardware* e montam os seus próprios circuitos. Isto é muito importante porque estamos a dar um correspondente fisico a uma coisa puramente virtual (programação) para além de que os alunos começam a familiarizar-se com conceitos de eletrónica e circuitos elétricos. De certa forma, o Arduino aproxima *hardware* e *software* de uma forma quase simbiótica (um não funciona sem o outro, embora haja a tendência de nos esquecermos disso).

Em segundo lugar, há o facto de a *computação física* (sistemas que interagem com o mundo exterior através de sensores), permitir observar de uma forma muito simples, o que está a ser programado por software. O aluno consegue perceber que o que programou está efetivamente a acontecer. Com duas linhas de código, consegue perceber que um LED está acesso porque foi isso que programou.

## Que Arduino deve utilizar?

Existem imensos tipos diferentes de Arduino, para as mais variadas funções e necessidades. O que costumamos usar no workshop é o Arduino Uno e esta é a minha recomendação para todos aqueles que se estão a iniciar nos sistemas embebidos. Também começam a aparecer mais Arduinos Leonardo que também servem perfeitamente mas possuem algumas configurações diferentes das do Uno.

![]({{ site.baseurl}}/img/tenhoArduino/arduino.jpg)

## O que preciso para começar a programar?

Ao nível mais básico, precisas de um computador (claro!), o IDE do Arduino (onde vais programar), um Arduino e um cabo USB A to B (se estiveres a usar um Arduino Uno) ou um cabo micro USB (se estiveres a usar um Arduino Leonardo). 

> PRO TIP: Os Arduinos nunca vêm com cabo (a não ser que tenhas comprado um kit). Se não tiveres um cabo USB A to B, fica a saber que este é o tipo de cabo que a grande maioria das impressoras usa.

![]({{ site.baseurl}}/img/tenhoArduino/cabo.jpg)

Mas com isto não consegues fazer muita coisa... É aqui que entra a eletrónica. O kit que os tutoriais têm por base contêm o seguinte material:

- 1 Breadboard
- 1 LED verde
- 1 LED amarelo
- 1 LED vermelho
- 3 resistências de 150 Ohm
- 3 resistências de 390 Ohm
- 4 resistências de 820 Ohm
- 1 resistência de 10K Ohm
- 1 Botão
- 1 Potenciómetro (Resistência Variável)
- 1 Display de 7 segmentos
- 1 Fotoresistência
- Fios de ligação

Entrarei em detalhes sobre cada um dos componentes no próximo artigo. Por agora, queremos apenas fazer o nosso primeiro projeto em Arduino, o "Blink" onde colocamos um LED a piscar com a frequência desejada. Se não tiveres um LED, não te preocupes! O Arduino tem um soldado na própria placa, ligado ao Pin 13.

> FUN FACT: Em programação, o primeiro projeto de uma linguagem costuma-se designar por "Hello, World!". Neste programa, o código escreve a frase "Hello, World!" na consola. O "Blink" é como se fosse o "Hello, World!" do Arduino!

## O teu primeiro projeto em Arduino

A primeira coisa a fazer é instalar o IDE do Arduino. IDE significa *Integrated Development Environment* e é o programa onde vais escrever o teu código. O IDE está disponível para Windows, OSX e Linux. Podes fazer o download [aqui](https://www.arduino.cc/en/Main/Software).

Depois de teres feito o download e instalado o programa, podemos começar. Mas ainda não temos nada montado! Se não tiveres um LED, ignora esta secção  (mas não a explicação abaixo!). Começa por pegar na tua *breadboard*. Esta placa é o suporte físico de todas as ligações elétricas. Como podes ver existem quatro corredores horizontais e 63 ligações na vertical. Os quatro corredores destinam-se às alimentações e é recomendável seguires a convenção de azul para o GRD (terra) e vermelho para +5V. O Arduino tanto pode fornecer +5V como +3.3V. O Ground (GRD) é a referência da alimentação. Se seguires sempre a regra das cores, é muito menos provável enganares-te a montar o teu circuito. 

![]({{ site.baseurl}}/img/tenhoArduino/breadboard.jpg)

Na imagem, podes ver representadas as ligações interiores da breadboard. A separação que existe entre as ligações verticais serve para ligar certos componentes sem curto-circuitar as suas entradas. Antes de montarmos o circuito, vamos olhar para o nosso Arduino com mais atenção. Do lado esquerdo, no primeiro conjunto de pinos a contar de cima tens as ligações das alimentações.

Temos então, e por ordem:

- **IOREF** Pin que distingue se se estão a usar +5V ou +3.3V (só necessário para usos mais avançados)
- **3.3V** Pin que fornece +3.3V
- **5V** Pin que fornece +5V
- **GND** Pin que estabelece o Ground
- **GND** Pin que estabelece o Ground
- **Vin** Tensão de input que está a ser fornecida pelo *Power Jack* (apenas necessário se estivermos a usar o *Power Jack* para alimentar o Arduino em vez do cabo USB). O *Power Jack* pode ser útil quando já fizeste upload do teu programa para o Arduino e queres que ele esteja a correr sem estar ligado ao computador. Por exemplo, um relógio, um termómetro ou um calendário. O *Power Jack* suporta tensões de 7V a 12V.

Abaixo dos pins de alimentação, temos os pins analógicos. A distinção entre analógico e digital é muito importante. Uma grandeza analógica é algo que pode tomar vários valores. Por exemplo, a temperatura de uma sala. Já uma grandeza digital é binária, ou seja, só pode tomar dois valores: 0 ou 1, *true* ou *false*. Existem 6 pins analógicos à tua disposição no UNO. Do lado direito, podes ver os pins digitais, 14 no UNO. Se prestares atenção, consegues ver que ao lado dos pinos 0 e 1 existem duas inscrições, RX e TX. RX significa "Receber" e TX "Transmitir". Estes pins são usados na Comunicação Série que iremos discutir no próximo artigo. Outra marca, na qual poderás ter eventualmente reparado, são uns pequenos tils (~) ao lado de alguns pins. Estes pins têm capacidade PWM, que significa *Pulse Width Modulation*, e que se irá revelar muito útil e da qual iremos falar em artigos futuros.

> FUN FACT: Reparaste na forma como os pins estão numerados? A numeração começa no zero pois, em programação, começamos sempre a contar a partir do zero e não do um! 

No entanto, ainda não falámos da parte mais importante do nosso Arduino! Aquele chip com 28 ligações que imediatamente à direita dos pins analógicos é aquilo a que chamamos um microcontrolador. Um microcontrolador é um computador muito pequenino, que processa, possui memória e entradas e saídas. Tudo no mesmo chip e, por isso, se diz que um microcontrolador é um sistema embebido. Provavelmente nunca ouviste este termo, mas fica a saber que se trata de um dos avanços tecnológicos mais importantes do último século. Os sistemas embebidos estão por todo o lado: desde o teu telemóvel aos sensores de um carro!

Na verdade, programar um microcontrolador é uma tarefa algo complicada que exige uma compreensão profunda do modo com este funciona. Esta é uma das razões pela qual o Arduino é a plataforma de eleição: a tarefa de o programar é extremamente facilitada por uma série de funções já fornecidas.

Antes de começarmos a montar o circuito, é importante que tenhas algumas noções de Eletrónica. Um LED é um Díodo Emissor de Luz. Um Díodo é um componente eletrónico no qual a corrente flui apenas num sentido. O sentido da corrente é do Ânodo (elétrodo positivo) para o Cátodo (elétrodo negativo). Uma boa forma de te lembrares desta convenção é pensares numa cascata: a água passa da parte mais alta para a mais baixa. Aqui é precisamente o mesmo caso. O mais interessante desta analogia é que o princípio de funcionamento de um LED baseia-se precisamente numa diferença de potencial. 

Mas como sabemos qual é o Ânodo e qual é o Cátodo? Pela diferença do comprimento das ligações do LED! Se reparares, um dos encaixes é mais longo: esse é o Ânodo. Podes ver na imagem abaixo, com mais atenção, a diferença. 

![]({{ site.baseurl}}/img/tenhoArduino/led.jpg)

Mas ainda falta adicionarmos uma resistência! Quando juntamos um elemento que consome corrente como é o caso do nosso LED, esta deve ser limitada com uma resistência. Isto porque uma variação mínima na tensão do mesmo irá causar uma grande variação na corrente fornecida ao LED. Mas, e como saber o valor da resistência a usar? Recorrendo a algo que, de certeza, já conheces: a Lei de Ohm. Ora a Lei de Ohm, uma relação de natureza experimental, diz-nos que a tensão é igual à resistência vezes a corrente elétrica, ou seja, U = RI. Como o nosso Arduino funciona a 5V, temos então que U = 5V. Embora varie com a cor do LED, um valor de corrente seguro para o funcionamento do mesmo é 20 mA. Temos assim que I = 20 mA.

Aplicando a fórmula temos,

<img src="http://www.sciweavers.org/tex2img.php?eq=U%20%3D%20RI%20%20%20%5CLeftrightarrow%205V%20%3D%20R%20%20%5Ctimes%2020mA%20%20%5CLeftrightarrow%20R%20%3D%20250%20%20%5COmega%20&bc=White&fc=Black&im=png&fs=12&ff=arev&edit=0" align="center" border="0" alt="U = RI   \Leftrightarrow 5V = R  \times 20mA  \Leftrightarrow R = 250  \Omega " width="319" height="17" />

Logo, concluimos que o ideal é usar uma resistência de 250 Ohm ou ligeiramente superior. Como 20 mA é um valor de certa forma conservativo, temos margem para poder usar resistências mais pequenas como é o caso das resistências de 150 Ohms.

Feito o esclarecimento relativamente a que resistência utilizar, vamos então passar ao circuito. Podes ver o esquema das ligações no diagrama abaixo, criado com o programa *Fritzing*. Começas por ligar a ligação mais curta do LED ao GND. De seguida, ligas a resistência ao Ânodo (ligação mais comprida) e, de seguida, ligas a resistência ao pino 13 por intermédio de um fio de ligação.

![]({{ site.baseurl}}/img/tenhoArduino/circuito.png)

## Programar o *Blink*

Agora que já temos o circuito montado, só nos resta começar a programar. Começa por abrir o Arduino IDE. Quando o fizeres, repara que há logo duas funções pré-definidas: o `void setup()` e o `void loop()`. Estas duas estruturas são muito importantes e são as bases de um programa de Arduino. O `void setup()` é uma estrutura de inicialização. É aqui que vais inicializar as tuas variáveis, atribuir funções aos teus pinos e outro tipo de instruções que só queiras que corram uma vez no início do programa. Já o `void loop()` corre o código dentro das chavetas num *loop* infinito.

Vamos agora pensar no nosso problema: queremos acender e apagar um LED com uma determinada periodicidade de modo a que este pisque. A primeira coisa que devemos fazer é dizer ao Arduino que o pino 13 é um OUTPUT, ou seja, estamos a enviar informação para lá. Se estivéssemos a receber dados, por exemplo de um sensor, então deveríamos configurar esse pino como INPUT. A maneira como fazemos esta atribuição é com uma função já pré-definida chamada `pinMode()`. Esta função tem dois argumentos, ou seja, elementos adicionais que lhe passamos. Neste caso, os argumentos são o número do pino, neste caso, 13, e se este está a funcionar como INPUT ou OUTPUT.

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

Se tudo tiver corrido bem, tens agora o teu LED a piscar com a frequência que definiste! Parabéns por teres concluído o primeiro tutorial de Arduino. Na continuação deste tutorial, iremos ter vários pequenos projetos que fazem uso de outros componentes do nosso kit. Até à próxima :smile:
