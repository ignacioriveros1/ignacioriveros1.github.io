---
layout: page
title: Papers and Drafts
permalink: /research
---

## Working Papers
<div class="research">
  <ul class="ul-research">
    {% for item in site.data.working_papers %}
      <li>
      <b>{{ item.title }}</b>
      {% if item.coauthors %}, 
        with 
        <br/>  <i>{{ item.coauthors }} </i>
      {% endif %}
      <br/> {{ item.abstract }}
      </li>
    {% endfor %}
  </ul>
</div>
