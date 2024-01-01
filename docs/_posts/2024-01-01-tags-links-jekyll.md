---
layout: post
title: ""
date: 2024-01-01 13:37:01 +0200
tags: tags jekyll
author: Eljas Hyyrynen
---

I was reading my own posts and noticing that even though I list the tags in the header of every post they don't have a link to all posts with that tag.
That would be an useful feature.
This post shows you how to do it.

## Recap: making tags

I have this kind of header in the beginning of my post

{% raw %}
```markdown
---
layout: post
title: "foo"
date: 2024-01-01 13:73:00 +0200
tags: foo bar
author: Eljas Hyyrynen
---
```
{% endraw %}

So far so good?
You can see the resulting header in any of my posts.
But what does this actually do? 
Where is the exact piece of code that processes these posts and the tags in them?

I'm using a Jekyll theme called [minima](https://github.com/jekyll/minima).
I have added the following line to my `_config.yml` as instructed by minima.
Note that I'm using `remote_theme` instead of `theme` in order to specify the Github repository of the theme instead of downloading it.

{% raw %}
```yml
remote_theme: jekyll/minima
```
{% endraw %}

I need to read the associated code in `docs/_layouts/post.html`.
I'm not using [the default one from minima](https://github.com/jekyll/minima/blob/master/_layouts/post.html) but my own instead.
It looks like this:

{% raw %}
```html
    <p class="post-meta">
        {% if page.tags.size > 0 %}
        Tag{% if page.tags.size > 1 %}s{% endif %}:
        {{ page.tags | sort | join: ", " }}
        {% endif %}
    </p>
```
{% endraw %}

Are you still with me?
Please refer to [my post.html file](https://github.com/Hyrtsi/hyrtsi.github.io/blob/master/docs/_layouts/post.html) if you need inspiration.
I might write a tutorial on creating and modifying your own layouts in the future.

# The beef

We need to modify this piece of Liquid code to actually create links for the tags.
But where do the links lead to?
There needs to be a page that has all posts for every tag.
How to do that?
There are many ways.

1. Generate manually an unique `.html` page for every tag

You can probably understand how bad idea this is since it needs to be maintained manually.

2. Automatically generate an unique `.html` page for every tag

Better, but you need to use Github actions to make sure your script does the work.
Not bad, but we can do better.
I don't like the idea of separate pages either.

3. Automatically generate a single page containing all the tags

We are going with this one.
Please read [my tutorial](https://hyrtsi.github.io/2022/10/20/tag-cloud.html) on how to generate a tag cloud.
Now, in my case, the target page is [https://hyrtsi.github.io/tags](https://hyrtsi.github.io/tags).
I need each and every link to point to the header associated with the tag name.
How to do that?

## 1. Create the anchors in the tag page

This will be the target link for the tag.
For a tag `T` there needs to be a link to it.
The link will be in my case of form `https://hyrtsi.github.io/tags/#T`.

If you followed my guide, go to the file `docs/tags.md`.
If you didn't follow my guide go to your associated page with the tag cloud.
If you embedded the tag cloud to each and every page sidebar, header of footer for example, go to your layout code that generates the tag cloud.

Now, for each tag, create an anchor (jump link) to it by using the format `id=anchor_name` inside the `<h3>` tag (or whatever tag you use):

{% raw %}
```liquid
{% for tag in site.tags %}
  <h3 style="font-size: {{ tag | last | size  |  times: 15 | plus: 80  }}%" id={{tag[0]}}>{{ tag[0] }} ({{ tag[1] | size }} posts)</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}
```
{% endraw %}

You can validate that this step is success by trying for a few tag names to use the anchor links.
For example:
- [https://hyrtsi.github.io/tags/#ml](https://hyrtsi.github.io/tags/#ml)
- [https://hyrtsi.github.io/tags/#opensource](https://hyrtsi.github.io/tags/#opensource)
- [https://hyrtsi.github.io/tags/#c++](https://hyrtsi.github.io/tags/#c++)

Awesome.
Then we need to make the header of each post to use these links instead of the plain text for the tag names.

## 2. Add the links to the header tag list

Now we're editing `docs/_layouts/post.html`.

This is the original code that creates the tags in the post header:

Inside

{%raw%}
```html
<article class="post h-entry" itemscope itemtype="http://schema.org/BlogPosting">
  <header class="post-header">
```
{%endraw}

we change this:

{%raw%}
```liquid
    <p class="post-meta">
        {% if page.tags.size > 0 %}
        Tag{% if page.tags.size > 1 %}s{% endif %}:
        {{ page.tags | sort | join: ", " }}
        {% endif %}
    </p>
```
{%endraw%}

to this:

{%raw%}
```liquid
    <p class="post-meta">
        {% if page.tags.size > 0 %}
        Tag{% if page.tags.size > 1 %}s{% endif %}:
        {% for tag in page.tags %}
          <a href="{{ '/tags/' | relative_url }}#{{ tag | slugify }}" class="p-category">{{ tag }}</a>{% if forloop.last == false %}, {% endif %}
        {% endfor %}
        {% endif %}
    </p>
```
{%endraw%}

Compile your page locally and see if there are any errors.
For me after this change it started to magically work!
Explanation for the spell:

- `<a href=...` means a hyperlink.
- `{{ '/tags/' | relative_url}}` resolves into `/tags/` that comes after the base url of your blog (`https://yourblogname.github.io`)
- Then comes the `#` for the anchor that we want after `/tags/` 
- Finally, the tag name that you have named the anchors after

It was actually Github Copilot that suggested me this change. 
Cool.

That's it!
Now you know how to create links to the collection of posts by tag to the header of your post.
Liquid is powerful.
