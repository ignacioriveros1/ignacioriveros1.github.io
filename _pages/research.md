---
layout: page
title: Research
subtitle: Here you will find my published articles and the current papers I'm working on
permalink: /research
---

<div>
{% assign researchsCategory = site.research | group_by_exp:"research", "research.category"  %}
{% for category in researchsCategory %}
<h4 class="research-teaser__month">
<strong>
{% if category.name %} 
<center>
- - - - - - - - - - - - - - - - - - - -  {{ category.name }} - - - - - - - - - - - - - - - - - - - -
</center>
{% else %} 
{{ Print }} 
{% endif %}
</strong>
</h4>
<ul class="list-researchs">
{% for research in category.items %}
<li class="research-teaser">
<a href="{{ research.url | prepend: site.baseurl }}">
<span class="research-teaser__title"> 
  {{ research.title }}
</span>
</a>
{% if research.journal %} 
- <i> {{ research.journal }}  </i>
{% endif %}
{% if research.pub_year %}
- {{ research.pub_year }}
{% endif %}
{% if research.coauthors %} 
 <br> <i> with {{ research.coauthors }} </i>
{% endif %}
</li>
{% endfor %}
</ul>
{% endfor %}
</div>
