---
layout: page
title: Tags
permalink: /tags/
---

<!-- {% assign all_tags = '' | split: ',' %}

{% for post in site.posts %}
    {% for tags in post.tags %}
        {% for tag in tags %}
            {% assign all_tags = all_tags | push: tag %}
        {% endfor %}
    {% endfor %}
{% endfor %}

{% assign all_tags = all_tags | sort %}
{% assign all_tags = all_tags | uniq %} -->


{%- for tag in site.tags -%}
  <a style="font-size: {{ tag | last | size  |  times: 40 | plus: 60  }}%">{{ tag[0] | append: " "}}</a>
{%- endfor -%}

<br/>

---

{% for tag in site.tags %}
  <h3 style="font-size: {{ tag | last | size  |  times: 15 | plus: 80  }}%">{{ tag[0] }} ({{ tag[1] | size }} posts)</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}

---