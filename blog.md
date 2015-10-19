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
