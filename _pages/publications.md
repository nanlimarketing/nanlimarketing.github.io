---
layout: page
permalink: /research/
title: Research
description: 
nav: true
nav_order: 1
---
<!-- _pages/publications.md -->
<!-- <div class="publications">

{% bibliography -f {{ site.scholar.bibliography }} %}

</div> -->

<div class="publications">

<h1>Working papers</h1>

{% bibliography -f preprints %}

<h1>Work-in-progress</h1>

{% bibliography -f reports %}

<h1>Publications</h1>

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}



</div>