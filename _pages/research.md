---
layout: page
title: Research
subtitle: Here you will find my current and past research projects.
permalink: /research
---

<div id="research_list"> 
{% assign sortedResearch = site.research | sort: 'category' %}
{% assign researchsCategory = sortedResearch | group_by_exp:"research", "research.category" %}
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
<span class="research-teaser__title"> 
  {{ research.title }}
</span>

{% if research.coauthors %} 
 <br> <i> with {{ research.coauthors }}</i>
{% endif %}

{% if research.journal %} 
, <i> {{ research.journal }} </i>
{% endif %}

{% if research.pub_year %}
, {{ research.pub_year }}
{% endif %}

{% if research.files.manuscript %}
<br>
<a href="{{ research.files.manuscript }}" >Manuscript</a>
{% endif %}

{% if research.files.repfiles %}
<a href="{{ research.files.repfiles }}" >- Replication Files</a> 
{% endif %}

{% if research.files.oa %}
<a href="{{ research.files.oa }}" >- Online Appendix</a>
{% endif %}

{% if research.abstract %}
<br>
<a onclick="showHide( '{{ research.title }}' )">Show/Hide Abstract </a>
  <div id= "{{ research.title }}" style="display:none">
    {{ research.abstract }}
  </div>
{% endif %}


</li>
{% endfor %}
</ul>
{% endfor %}

</div>



<!-- Adding the showHide() function -->
<script>
  function showHide(id) {
    var abstract = document.getElementById(id);
    if (abstract.style.display === "none") {
      abstract.style.display = "block";
    } else {
      abstract.style.display = "none";
    }
  }
</script>