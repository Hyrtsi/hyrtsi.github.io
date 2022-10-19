---
layout: page
title: Tags
permalink: /tags/
---

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

<ul class="tag-list">
    {% for tag in all_tags %}
        <li><a href="{{ site.tag_dir | prepend: '/' }}/{{ tag | uri_escape }}">{{ tag }}</a></li>
    {% endfor %}
</ul>