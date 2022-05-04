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

|name|val|
|----|----|
{% assign beatles = "John, Paul, George, Ringo, xxx, vvv" | split: ", " -%}
{% for member in beatles -%}
|  {{ member }} | |
{% endfor -%}


