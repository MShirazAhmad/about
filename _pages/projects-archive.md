---
title: Projects
layout: collection
permalink: /projects/
collection: projects
entries_layout: grid
classes: wide
---
{% capture written_label %}'None'{% endcapture %}

{% for projects in site.projects %}
  {% unless projects.output == false or projects.label == "projects" %}
    {% capture label %}{{ projects.label }}{% endcapture %}
    {% if label != written_label %}
      {% capture written_label %}{{ label }}{% endcapture %}
    {% endif %}
  {% endunless %}
  {% for post in projects.docs %}
    {% unless projects.output == false or projects.label == "projects" %}
      {% include archive-single.html %}
    {% endunless %}
  {% endfor %}
{% endfor %}
