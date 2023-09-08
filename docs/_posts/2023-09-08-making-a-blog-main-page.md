---
layout: post
title:  "Making a list of blog posts automatically in Jekyll"
date:   2023-09-08 13:37:11 +0200
tags: jekyll blog automation
---

# Intro

I have been spending less and less time coding lately.
It's not that I don't like coding anymore.
It's that I like other things too much.
You see, the winter is really long, dark and cold in Finland.
And after the winter there is a short, sunny, warm and intensive summer.
The sun doesn't set in the north.
Like, I can't comprehend how up north we are here.

Anyways. Summer has meant for me time off the PC and time on the world.
I've been mostly bicycling and dancing.
So much that I will be blogging mainly on bicycling in the future.
Coding is a thing for me that I fund my adventures with.
Problem solving remains my passion but these are my priorities.

# The beef

I was thinking of making another blog for bicycling and leaving this entirely for coding.
However, I like this format. I like creating webpages using markdown, Jekyll and Github.
It's a good stack for a c++ programmer who doesn't know Wordpress or Javascript.
So here's the crux of this post: how to create a new main page like the one I have right now at https://hyrtsi.github.io that has a list of the recent posts?

## Creating a new html page

First we have to create a new page on the `github.io` page because I don't want to remove the coding blog posts or change the main page.
It's easy: just create a `.html` file inside `docs/`, commit it, push to remote, wait for the deployment and you're ready.

Here's an example:

`touch docs/test.html`

and add this

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Hello world!</title>
    </head>
    <body>
        <p>Just testing</p>
    </body>
</html>
```

then `git add -u && git commit -m "add test.html" && git push`

You can check the results in `https://hyrtsi.github.io/test`.
So the URL will be same as the filename.
Easy, eh?
Yes. But you don't necessarily want to start from raw html.
You can use the magic instead.
Create a markdown file in `docs/` instead and you'll be better off.

Let's do that next to get familiar with the system.

## Creating a markdown page

`touch docs/mdtest.md`

Then add the content to the file

{% raw %}
```markdown
---
layout: page
title: Mdtest
permalink: /mdtest/
---

# Hello

`this is markdown`
```
{% endraw %}

Again, commit, push and see the page.
Note that you can choose the permalink like I did above: https://hyrtsi.github.io/mdtest

Now you have a page with your layout instead of a "blank" html page.
Next: how do I go about creating a bicycling blog inside my coding blog?

I will edit `mdtest.md` to show you.
First you have to get familiar with Liquid.
RTFM [here](https://shopify.github.io/liquid/basics/introduction/).
Liquid is a language that is convenient for getting information from structured information such as blogs.
We can use it to loop over the pages, posts, tags and more.
Let me show an example.

If you have any posts this will loop over them:

{%raw%}
```liquid
{%for post in posts%}
    <!--... do something with the posts -->
{%endfor}
```
{%endraw%}

I don't want to list all posts there, just the bicycling ones.
So I add a conditional:

{%raw%}
```liquid
{%for post in posts%}
    {%if posts.tags contains 'bicycling'%}
        <!--... do something with the posts -->
    {%endif%}
{%endfor}
```
{%endraw%}

See? It's simple.
I actually copied the implementation from Jekyll Minima source code.
[<link>](https://github.com/jekyll/minima/blob/master/_layouts/home.html).

I copied that code and got a satisfying result.
You can add more rules for filtering such as tags, date, pagination.
You can also add excerpts (damn how difficult word that is to write) of your posts.
Here's the code:

{%raw%}
```liquid
---
layout: base
---

<div class="home">
  {% if site.paginate %}
    {% assign posts = paginator.posts %}
  {% else %}
    {% assign posts = site.posts %}
  {% endif %}

  {%- if posts.size > 0 -%}
    {%- if page.list_title -%}
      <h2 class="post-list-heading">{{ page.list_title }}</h2>
    {%- endif -%}
    <ul class="post-list">
      {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
      {%- for post in posts -%}
        {%- if post.tags contains 'bicycling' -%}
        <li>
            <span class="post-meta">{{ post.date | date: date_format }}</span>
            <h3>
            <a class="post-link" href="{{ post.url | relative_url }}">
                {{ post.title | escape }}
            </a>
            </h3>
            {%- if site.show_excerpts -%}
            {{ post.excerpt }}
            {%- endif -%}
        </li>
        {%- endif -%}
      {%- endfor -%}
    </ul>

    {% if site.paginate %}
      <div class="pager">
        <ul class="pagination">
        {%- if paginator.previous_page %}
          <li><a href="{{ paginator.previous_page_path | relative_url }}" class="previous-page">{{ paginator.previous_page }}</a></li>
        {%- else %}
          <li><div class="pager-edge">•</div></li>
        {%- endif %}
          <li><div class="current-page">{{ paginator.page }}</div></li>
        {%- if paginator.next_page %}
          <li><a href="{{ paginator.next_page_path | relative_url }}" class="next-page">{{ paginator.next_page }}</a></li>
        {%- else %}
          <li><div class="pager-edge">•</div></li>
        {%- endif %}
        </ul>
      </div>
    {%- endif %}

  {%- endif -%}

</div>
```
{%endraw%}

With this boilerplate code you can have as many blogs inside your blog as you want.
You can just filter the tags you want like shown above and create new pages where you list those posts.

## How to remove posts with a certain tag from the main page?

The main page of a Jekyll Minima -based page contains the list of posts.
If you want to change that, you have to first create a file `docs/_layouts/home.html`. 
You must copy the contents of Minima's implementation from their github repository or write it from scratch.
I did the former with this change:

{%raw%}
```liquid
      {%- for post in posts -%}
        {%- unless post.tags contains 'bicycling'-%}
    .... rest of the stuff
```
{%endraw%}

The whole file looks like this

{%raw%}
```liquid
---
layout: base
---

<div class="home">
  {%- if page.title -%}
    <h1 class="page-heading">{{ page.title }}</h1>
  {%- endif -%}

  {{ content }}


  {% if site.paginate %}
    {% assign posts = paginator.posts %}
  {% else %}
    {% assign posts = site.posts %}
  {% endif %}


  {%- if posts.size > 0 -%}
    {%- if page.list_title -%}
      <h2 class="post-list-heading">{{ page.list_title }}</h2>
    {%- endif -%}
    <ul class="post-list">
      {%- assign date_format = site.minima.date_format | default: "%b %-d, %Y" -%}
      {%- for post in posts -%}
        {%- unless post.tags contains 'bicycling'-%}
            <li>
                <span class="post-meta">{{ post.date | date: date_format }}</span>
                <h3>
                <a class="post-link" href="{{ post.url | relative_url }}">
                    {{ post.title | escape }}
                </a>
                </h3>
                {%- if site.show_excerpts -%}
                {{ post.excerpt }}
                {%- endif -%}
            </li>
        {%- endunless -%}
      {%- endfor -%}
    </ul>

    {% if site.paginate %}
      <div class="pager">
        <ul class="pagination">
        {%- if paginator.previous_page %}
          <li><a href="{{ paginator.previous_page_path | relative_url }}" class="previous-page">{{ paginator.previous_page }}</a></li>
        {%- else %}
          <li><div class="pager-edge">•</div></li>
        {%- endif %}
          <li><div class="current-page">{{ paginator.page }}</div></li>
        {%- if paginator.next_page %}
          <li><a href="{{ paginator.next_page_path | relative_url }}" class="next-page">{{ paginator.next_page }}</a></li>
        {%- else %}
          <li><div class="pager-edge">•</div></li>
        {%- endif %}
        </ul>
      </div>
    {%- endif %}

  {%- endif -%}

</div>
```
{%endraw%}

Remember to test that locally and add the file to version control!
If you followed the same steps you should have two different "feeds" of posts that "follow" different tags.

Happy blogging!