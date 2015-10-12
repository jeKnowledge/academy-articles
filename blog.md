---
layout: post
title: blog
permalink: /blog/
---

<div class="home">

  <h1 class="page-heading">Posts</h1>

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
