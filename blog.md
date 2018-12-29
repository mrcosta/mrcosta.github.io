---
layout: default
title: Blog archive
---
<div class="page-content wc-container">
  <h1>Hi, my name is Mateus and I like to code =)
      I have passion for building software with quality and creative solutions for who will use and maintain.
      TDD and Clean Code as essential skills in my day to day. I’m also playing a little with blockchains and advocating for privacy on the internet.
  </h1>  
  {% for post in site.posts %}
  	{% capture currentyear %}{{post.date | date: "%Y"}}{% endcapture %}
  	{% if currentyear != year %}
    	{% unless forloop.first %}</ul>{% endunless %}
    		<h5>{{ currentyear }}</h5>
    		<ul class="posts">
    		{% capture year %}{{currentyear}}{% endcapture %}
  		{% endif %}
		<li>
			<a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
		</li>
    {% if forloop.last %}</ul>{% endif %}
{% endfor %}
</div>
