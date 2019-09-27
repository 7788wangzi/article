---
layout: default
title: "My blog"
---

<h2>{{ page.title }}</h2>


<nav class="myNav">
<ul>
  <li><a href="#home">My Site</a></li>
  
  
  <li class="dropdown right">
    <a href="javascript:void(0)" class="dropbtn">Categories</a>
    <div class="dropdown-content">
      <a href="#">Link 1</a>
      <a href="#">Link 2</a>
      <a href="#">Link 3</a>
    </div>
  </li>
  <li class="dropdown right">
    <a href="javascript:void(0)" class="dropbtn">Tags</a>
    <div class="dropdown-content">
      <a href="#">Link 1</a>
      <a href="#">Link 2</a>
      <a href="#">Link 3</a>
    </div>
  </li>
</ul>
</nav>

<ul>

{% for post in site.posts %}
<li>{{post.date | date_to_string}} <a href="{{site.baseurl}}{{post.url}}">{{ post.title }}</a></li>
{% endfor %}

</ul>

