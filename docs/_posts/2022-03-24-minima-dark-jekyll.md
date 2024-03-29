---
layout: post
title:  "How to use minima dark theme on Jekyll + Github pages"
date:   2022-03-24 19:47:25 +0200
tags: jekyll githubpages
author: "Eljas Hyyrynen"
---

This guide assumes that you have already set up your Jekyll pages using Github pages or similar and you want to change your theme.

1. Change the `gem "minima"` line in the `Gemfile` as follows so it contains the github repo link

```
gem "minima", "~> 2.5", git: "https://github.com/jekyll/minima"
```

2. `bundle install && bundle update` to load whatever there is in the github repo

3. Change the build settings in `_config.yml`

In the place where you earlier had

```
theme: minima
```

just add this line after `theme: minima`:

```
minima:
  skin: dark
```

You may add other options for `minima` as well such as `header_pages`, `date_format`, `disqus`, `author`, `social_links` and more. Check [the readme](https://github.com/jekyll/minima) of the official `minima` repo.

4. Update your page and check the results!

```
bundle exec jekyll serve
```

then check `http://127.0.0.1:4000/` with your browser.

Credit for [Slowbro](https://blog.slowb.ro/dark-theme-for-minima-jekyll/). I found the technique from their blog post and wrote a dumbified-down tutorial here for my own fun.

