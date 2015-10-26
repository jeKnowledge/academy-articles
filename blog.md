---
layout: default
title: Academy Blog
permalink: /blog/
---
  <h1 class="page-heading" style="display:inline">Posts</h1><div class="site-title"></div>
<div>
  <ul class="entries">
    {% for post in site.posts %}
    	<li class="entry group">
    		<div class="entry-date">
    			<!--<time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d, %Y" }}</time>-->
    			<time datetime="{{ post.date | date_to_xmlschema }}">		{% assign m = page.date | date: "%-m" %}
{% case m %}
  {% when '1' %}Janeiro
  {% when '2' %}Fevereiro
  {% when '3' %}Março
  {% when '4' %}Abril
  {% when '5' %}Maio
  {% when '6' %}Junho
  {% when '7' %}Julho
  {% when '8' %}Agosto
  {% when '9' %}Setembro
  {% when '10' %}Outubro
  {% when '11' %}Novembro
  {% when '12' %}Dezembro
  {% endcase %}
{{ page.date | date: "%Y" }}</time>
    		</div>
    		<div class="entry-title">
    			<h2>
    			<a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
    			</h2>
    		</div>
    	</li>
    {% endfor %}
  </ul>
  <br>
</div>
