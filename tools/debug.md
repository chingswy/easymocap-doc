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


{% highlight javascript %}
{% include alert_msg.js %}
{% endhighlight %}



<button type="button" name="submit" class="btn">Button element</button>

<script type="text/javascript" charset="utf-8">
$(document).ready(function(){
    $("#submit").click(function(e){
    {% include alert_msg.js %}
    return false;
    })
});
</script>