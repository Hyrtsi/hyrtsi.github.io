---
layout: page
title: Tags
permalink: /tags/
tags: tag
---

<h1>Tags of this page: {{ page.tags }}</h1>

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



<h1>Tags</h1>

<ul class="tag-list">
    {% for tag in all_tags %}
        <li><a href="{{ site.tag_dir | prepend: '/' }}/{{ tag | uri_escape }}">{{ tag }}</a></li>
        <ul class="posts-per-tag">
        {% for post in site.posts %}
            {% for tags in post.tags %}
                {% for taginner in tags %}
                  {% if tag == taginner %}
                    {% post_url {{post.title}} %}
                  <li><a href="https://github.io/hyrtsi">boo</a></li>
                  hello {{ post.title }}
                  {% endif %}
                {% endfor %}
            {% endfor %}
        {% endfor %}
        </ul>
    {% endfor %}
</ul>



<h3>The contents of all_tags variable:</h3>

{{ all_tags }}

<h3>Another try:</h3>

<ul class="tag-list">
    {% for tag in all_tags %}
        <li><a href="{{ site.tag_dir | prepend: '/' }}/{{ tag | uri_escape }}">{{ tag }}</a></li>
    {% endfor %}
</ul>

<h3>Good but why use bullet points?</h3>
{% for tag in all_tags %}
{{tag}}
{% endfor %}

<h3>asd</h3>
{% for tag in all_tags %}
{{ site.tag_dir | prepend: '/' }}/{{ tag | uri_escape }}
{% endfor %}

<h3>asd</h3>
{% for tag in all_tags %}
{{ tag | uri_escape }}
{% endfor %}

