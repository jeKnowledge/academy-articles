---
layout: academy2016
title: jeKnowledge Academy 2016
permalink: /Academy2016.html
---

<style type="text/css">
  #top{
    min-width: initial;
    display:flex;
   /*Ocupa o ecran todo*/
    min-height:100vh; /*min-height para nao sobrepor o conteudo original quando a janela é muito pequena*/
    flex-wrap: wrap; /*faz com que o footer não esteja paralelo com o conteudo*/

    background: url('/academy-articles/img/stars.png'), radial-gradient(20rem circle at 50% 40%, rgb(22, 47, 117), rgba(255, 255, 255, 0)), linear-gradient(#162141, #1D2C4E);
    background-color: #061841;

}
  h1{
    text-align: center;
    width:100%;
    padding: 1em;
  }


  .container{
    display: flex;
    min-height:100vh;
    flex-wrap: wrap;
    justify-content:center;
    /*padding: 20px 30px;*/
    justify-content: space-around;
  }

  .col{
    display:flex; flex-wrap: wrap;    
    min-width: 18em;   
    padding: 1em 2em;
  }

  .col-faq{
    max-width: 20em;    
    padding: 1em 2em;
  }

  .cont{
    width:50%;
    padding: 1em 2em;
  }
  .map{
    width:100%;
  }

  #box{
    display: flex;
    flex-wrap: wrap;
    max-width: 40em;
    justify-content:center;
  }

  footer{
    width: 100%;/*expande horizontalmente footer*/
    align-self: flex-end;/*footer no fim da pagina*/
  }

  .content{
        margin: auto; /*centra conteudo*/


  }

</style>
<body>

<section id="top" class="container" data-section-menu="Home">
  <img src="http://jeknowledge.github.io/academy-articles/img/jkacademylogo_mobile.svg" width="230em">
  <h3> Dias 12, 13 e 14 de Julho</h3>
  <nav>
  <p>
    <a href="/academy-articles/" class="selected">Tutoriais</a>
    <a href="http://jeknowledge.pt/"  class="applink appstore-link" title="jeKnowledge">jeKnowledge</a>
    <a href="https://github.com/jeKnowledge" class="applink playstore-link" title="Github">Github</a>
  </p>
  <p>
    <a href="#maininfo" class="selected">Academy 2016</a>
    <a href="#faq" class="selected">FAQ</a>
    <a href="/academy-articles/" class="selected">Inscrições</a>
    <a href="#contacts" class="selected">Contactos</a>

  </p>
  </nav>

</section>

<section id="maininfo" class="container" data-section-menu="section 1">
  <h1>jeKnowledge Academy 2016</h1>
</section>

<section id="faq" class="container" data-section-menu="section 2">

  <h1>FAQ - Respondemos algumas perguntas por ti</h1>
  <div id="box">
  <div class="col col-faq">
      <h2>Para quem?</h2>
      <p>
        Para todos os alunos do 11º 12º ano com vontade de aprender e de se desafiar.
      </p>

      <h2>Como participo?</h2>
      <p>
        As inscrições estão abertas até dia 3 de julho às 23h59. Para participares basta preencheres o formulário de inscrição. Depois de submeteres o formulário receberás um e-mail nosso no endereço que indicaste com mais informações.
      </p>

      <h2>Preciso de levar computador?</h2>
      <p>
        Sim. O teu computador vai ser a tua ferramenta principal para estes dias espetaculares.
      </p>

      <h2>Preciso de ter conhecimentos de programação ou robótica?</h2>
      <p>
        Não precisas de qualquer conhecimento de programação ou robótica, só precisamos que tragas boa disposição e interesse em aprender.
      </p>
  </div>
  <div class="col col-faq">

      <h2>E o ambiente?</h2>
      <p>
        Entra em contacto com um ambiente empresarial, aprende connosco e faz-nos aprender contigo também. Não penses que um ambiente empresarial e sinónimo de aborrecimento, vem e verifica que pode ser exatamente o contrário.
      </p>

      <h2>As refeições?</h2>
      <p>
        As refeições durante o evento (3 almoços, bebidas e snacks durante o dia) vão ser asseguradas pela jeKnowledge e estão incluídas no custo de participação.
      </p>

      <h2>Quando custa?</h2>
      <p>
        O custo de participação será de 40 euros, com o objetivo de te assegurar a melhor experiência possível na jeKnowledge Academy (inclui um kit de iniciação contendo um Arduino e refeições).
      </p>

      <h2>Onde é?</h2>
      <p>
        A jeKnowledge Academy vai ser na nossa sede, no Departamento de Física, no Pólo 1 da Universidade de Coimbra. O evento começa às 9h00 do dia 12 de Julho.
      </p>
  </div>
  </div>
</section>


<section id="contacts" class="container" data-section-menu="section 1">
  <h1>Description 3</h1>
  <!--metade mapa metade info -->
  <div class="col map" style="background-color:blue;"></div>
  <div class="col cont" style="background-color:pink;"></div>
  <div class="col cont" style="background-color:green;"></div>
</section>

<section id="apoios" class="container" data-section-menu="section 1">
  <!--metade mapa metade info -->
  <div class="col map" style="background-color:blue;">
<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3047.0558726846093!2d-8.42665604944977!3d40.20782007614189!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0xd22f9098f98004b%3A0xf74b13602c7f2de3!2sDepartamento+de+F%C3%ADsica+da+Universidade+de+Coimbra!5e0!3m2!1spt-PT!2spt!4v1464274013492" frameborder="0" style="border:0; pointer-events:none; width:100%;" allowfullscreen></iframe>
  </div>
  <div class="col cont" style="background-color:pink;">
  jeKnowledge

  Departamento de Física sala B3
  Rua Larga
  P-3004-516
  </div>
  <div class="col cont" style="background-color:green;"></div>
</section>

</body>
