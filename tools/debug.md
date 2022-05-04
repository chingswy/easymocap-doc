---
layout: default
title: debug
parent: 实用工具
---

# debug
{: .no_toc }

1. TOC
{:toc}
---

{% assign beatles = "John, Paul, George, Ringo" | split: ", " %}
|name|val|
|----|----|
{% for member in beatles %}
|  {{ member }} | |
{% endfor %}


