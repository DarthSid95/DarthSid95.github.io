---
layout: page
permalink: /publications/
title: Publications
description:
nav: true
nav_order: 3
---

<!-- _pages/publications.md -->
<div class="publications">

<h2 style="text-align: center;">Ph.D. Thesis</h2>

{% bibliography --query @*[grp=1] %}

<h2 style="text-align: center;">Conference and Journal Publications</h2>

{% bibliography --query @*[grp=2] %}

<h2 style="text-align: center;">Workshop Publications and Prepints</h2>

{% bibliography --query @*[grp=3] %}

<h2 style="text-align: center;">Bachelor's Thesis</h2>

{% bibliography --query @*[grp=4] %}

</div>
