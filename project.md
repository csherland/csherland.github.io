---
layout: page
title: Projects
permalink: /projects/
---

Yo!
{{ site.collections }}

{{ site }}
{% for collection in site.collections %}
  {{ collection }}
{% endfor %}

{% for project in site.projects limit:20 %}
  <div class="project-card">
    <span class="project-meta">{{ project.date | date: "%b %-d, %Y" }}</span>
    <a class="project-link" href="{{ project.url | prepend: site.baseurl }}">{{ project.title }}</a>
    <p>{{ project.description }}</p>
  </div>
{% endfor %}
