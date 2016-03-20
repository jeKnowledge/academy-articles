# Academy Labs

Este é um blog onde é disponibilizado periodicamente conteúdo em português para quem quer dar os primeiros passos em programação de hardware de uma forma fácil.


## Informação de utilização para autores

### Permalinks
Usar **permalink:** tag para cescolher o link dos artigos.
```
---
layout: post
title:  "Jogo da Memória"
permalink: <o-teu-link>
---
```

#### OUTPUT
```
www.jeKnowledge.github.io/academy-articles/o-teu-link
```

### Tags
 Usar **series:** tag para criar uma coleção de artigos.

  *Baseado no blogpost:
 http://digitaldrummerj.me//blogging-on-github-part-13-creating-an-article-series/*

  **Exemplo:**
```
---
layout: post
title: "You're up and running!"
published: true
series: "<nome da coleção>"
---
```

#### OUTPUT
```
Continua a tua jornada!

Este artigo pertence à coleção " <nome da coleção> ".
Acabaste de ler a parte 1/1.

Parte 1 - Este artigo
...
```
  **Nota:** Os artigos são ordenados por ordem cronológica.
  Futuramente será implementada ordem por tag
