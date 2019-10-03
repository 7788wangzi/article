---
layout: main
title: "My blog"
---

<ul class="post-list">
<h1>Latest Post</h1>
{% for post in site.posts %}
<li>
<div class="date">
<h3>{{ post.date | date:"%Y"}}<br><span>{{ post.date | date:"%m"}}</span></h3>
</div>
<a href="{{site.baseurl}}{{post.url}}"><p>{{ post.title }}</p></a>
</li>
{% endfor %}

</ul>

