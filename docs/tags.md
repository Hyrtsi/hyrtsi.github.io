---
layout: page
title: Tags
permalink: /tags/
---

Tips here: https://jekyllrb.com/docs/posts/

{% assign all_tags = '' | split: ',' %}

{% for post in site.posts %}
    {% for tags in post.tags %}
        {% for tag in tags %}
            {% assign all_tags = all_tags | push: tag %}
        {% endfor %}
    {% endfor %}
{% endfor %}

{% assign all_tags = all_tags | sort %}
{% assign all_tags = all_tags | uniq %}

---

<h1>Tags</h1>

<ul class="tag-list">
    {% for tag in all_tags %}
        <li><a href="{{ site.tag_dir | prepend: '/' }}/{{ tag | uri_escape }}">{{ tag }}</a></li>
        <ul class="posts-per-tag">
        {% for post in site.posts %}
            {% for tags in post.tags %}
                {% for taginner in tags %}
                  {% if tag == taginner %}
                        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
                  {% endif %}
                {% endfor %}
            {% endfor %}
        {% endfor %}
        </ul>
    {% endfor %}
</ul>

---

{% for tag in site.tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}

---

<ul class="tag-list">
    {% for tag in all_tags %}
        <li><a href="{{ site.tag_dir | prepend: '/' }}/{{ tag | uri_escape }}">{{ tag }}</a></li>
    {% endfor %}
</ul>