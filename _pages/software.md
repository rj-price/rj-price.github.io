---
layout: page
title: software
permalink: /software/
description: Pipelines and AI tools I build and maintain.
nav: true
nav_order: 3
---

<!-- pages/software.md -->
<div class="projects">
  {% assign software_projects = site.projects | where: "category", "software" | sort: "importance" %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in software_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
</div>

{% if site.data.repositories.github_repos %}

## repositories

<div class="repositories d-flex flex-wrap flex-md-row flex-column justify-content-between align-items-center">
  {% for repo in site.data.repositories.github_repos %}
    {% include repository/repo.liquid repository=repo %}
  {% endfor %}
</div>
{% endif %}
