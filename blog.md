---
layout: default
title: Academy Blog
permalink: /blog/
---
{% assign m = page.date | date: "%-m" %}
{{ page.date | date: "%-d" }}
{% case m %}
  {% when '1' %}Jan
  {% when '2' %}Fev
  {% when '3' %}Mar
  {% when '4' %}Abr
  {% when '5' %}Maio
  {% when '6' %}Jun
  {% when '7' %}Jul
  {% when '8' %}Ago
  {% when '9' %}Set
  {% when '10' %}Out
  {% when '11' %}Nov
  {% when '12' %}Dez
{% endcase %}
{{ page.date | date: "%Y" }}


  <h1 class="page-heading" style="display:inline">Posts</h1><div class="site-title"></div>
<div>
  <ul class="entries">
    {% for post in site.posts %}
    	<li class="entry group">
    		<div class="entry-date">
    			<time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d, %Y" }}</time>
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
