---
layout: post
title:  "Jekyll + Github pages: Why isn't your theme working on remote even though it works locally?"
date:   2022-03-24 21:27:25 +0200
---

Your Jekyll page looks amazing. It's time to deploy. But one problem: after you deploy the page doesn't look anything like it was earlier.

Maybe your theme isn't loading or the page is totally broken.

Here are my tips for fixing the issue.

# 1. Check the baseurl in _config.yml

The fields `url` and `baseurl` look this for me:

```
baseurl: ""
url: "https://hyrtsi.github.io"
```

Make sure that you have defined them correctly.

# 2. Use remote_theme instead of theme

This finally fixed it for me.

In `Gemfile`, add `gem "jekyll-remote-theme"` to the place where you list your plugins.

Tip: After adding a new plugin remember to run `bundle install` locally if you want to double check that it works locally after your changes.

Then, in `_config.yml` replace `theme: minima` (or whatever theme you had) with `remote_theme: jekyll/minima` (or whatever theme you want).

The form is `github-user/repo-name`. You can also  use the link to the github repo. I found the tutorial [here](https://github.com/benbalter/jekyll-remote-theme) on the official page of `jekyll-remote-theme` plugin.