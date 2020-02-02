---
title: algorithms
layout: collection
permalink: /algorithms/
collection: algorithms
entries_layout: grid
classes: wide
---
{% capture written_label %}'None'{% endcapture %}

{% for algorithms in site.algorithms %}
  {% unless algorithms.output == false or algorithms.label == "posts" %}
    {% capture label %}{{ algorithms.label }}{% endcapture %}
    {% if label != written_label %}
      {% capture written_label %}{{ label }}{% endcapture %}
    {% endif %}
  {% endunless %}
  {% for post in algorithms.docs %}
    {% unless algorithms.output == false or algorithms.label == "posts" %}
      {% include archive-single.html %}
    {% endunless %}
  {% endfor %}
{% endfor %}
