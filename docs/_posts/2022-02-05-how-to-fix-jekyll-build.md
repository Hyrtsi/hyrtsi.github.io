---
layout: post
title:  "Fix Jekyll build: Error: Invalid syntax for include tag in social-icons/.svg"
date:   2023-01-15 13:37:00 +0200
tags: jekyll
author: Eljas Hyyrynen (hyrtsi)
---

My jekyll build is broken and my site does not update anymore.

It seems that when I test locally (or check Github actions) I see this:

{% raw %}
```
my@pc:~/source/hyrtsi.github.io/docs$ bundle exec jekyll serve
Configuration file: source/hyrtsi.github.io/docs/_config.yml
      Remote Theme: Using theme jekyll/minima
            Source: source/hyrtsi.github.io/docs
       Destination: source/hyrtsi.github.io/docs/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
      Remote Theme: Using theme jekyll/minima
       Jekyll Feed: Generating feed for posts
  Liquid Exception: Invalid syntax for include tag. File contains invalid characters or sequences: social-icons/.svg Valid syntax: {% include file.ext param='value' param2='value' %} in assets/minima-social-icons.liquid
jekyll 3.9.0 | Error:  Invalid syntax for include tag. File contains invalid characters or sequences:

  social-icons/.svg

Valid syntax:

  {% include file.ext param='value' param2='value' %}


Traceback (most recent call last):
        78: from gems/bin/bundle:23:in `<main>'
        77: from gems/bin/bundle:23:in `load'
        76: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/exe/bundle:36:in `<top (required)>'
        75: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/friendly_errors.rb:103:in `with_friendly_errors'
        74: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/exe/bundle:48:in `block in <top (required)>'
        73: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/cli.rb:25:in `start'
        72: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/vendor/thor/lib/thor/base.rb:485:in `start'
        71: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/cli.rb:31:in `dispatch'
        70: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/vendor/thor/lib/thor.rb:392:in `dispatch'
        69: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/vendor/thor/lib/thor/invocation.rb:127:in `invoke_command'
        68: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/vendor/thor/lib/thor/command.rb:27:in `run'
        67: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/cli.rb:483:in `exec'
        66: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/cli/exec.rb:23:in `run'
        65: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/cli/exec.rb:58:in `kernel_load'
        64: from /var/lib/gems/2.7.0/gems/bundler-2.3.10/lib/bundler/cli/exec.rb:58:in `load'
        63: from gems/bin/jekyll:23:in `<top (required)>'
        62: from gems/bin/jekyll:23:in `load'
        61: from gems/gems/jekyll-3.9.0/exe/jekyll:15:in `<top (required)>'
        60: from gems/gems/mercenary-0.3.6/lib/mercenary.rb:19:in `program'
        59: from gems/gems/mercenary-0.3.6/lib/mercenary/program.rb:42:in `go'
        58: from gems/gems/mercenary-0.3.6/lib/mercenary/command.rb:220:in `execute'
        57: from gems/gems/mercenary-0.3.6/lib/mercenary/command.rb:220:in `each'
        56: from gems/gems/mercenary-0.3.6/lib/mercenary/command.rb:220:in `block in execute'
        55: from gems/gems/jekyll-3.9.0/lib/jekyll/commands/serve.rb:75:in `block (2 levels) in init_with_program'
        54: from gems/gems/jekyll-3.9.0/lib/jekyll/commands/serve.rb:93:in `start'
        53: from gems/gems/jekyll-3.9.0/lib/jekyll/commands/serve.rb:93:in `each'
        52: from gems/gems/jekyll-3.9.0/lib/jekyll/commands/serve.rb:93:in `block in start'
        51: from gems/gems/jekyll-3.9.0/lib/jekyll/commands/build.rb:36:in `process'
        50: from gems/gems/jekyll-3.9.0/lib/jekyll/commands/build.rb:65:in `build'
        49: from gems/gems/jekyll-3.9.0/lib/jekyll/command.rb:28:in `process_site'
        48: from gems/gems/jekyll-3.9.0/lib/jekyll/site.rb:71:in `process'
        47: from gems/gems/jekyll-3.9.0/lib/jekyll/site.rb:192:in `render'
        46: from gems/gems/jekyll-3.9.0/lib/jekyll/site.rb:471:in `render_pages'
        45: from gems/gems/jekyll-3.9.0/lib/jekyll/site.rb:471:in `each'
        44: from gems/gems/jekyll-3.9.0/lib/jekyll/site.rb:472:in `block in render_pages'
        43: from gems/gems/jekyll-3.9.0/lib/jekyll/site.rb:479:in `render_regenerated'
        42: from gems/gems/jekyll-3.9.0/lib/jekyll/renderer.rb:62:in `run'
        41: from gems/gems/jekyll-3.9.0/lib/jekyll/renderer.rb:79:in `render_document'
        40: from gems/gems/jekyll-3.9.0/lib/jekyll/renderer.rb:126:in `render_liquid'
        39: from gems/gems/jekyll-3.9.0/lib/jekyll/liquid_renderer/file.rb:28:in `render!'
        38: from gems/gems/jekyll-3.9.0/lib/jekyll/liquid_renderer/file.rb:49:in `measure_time'
        37: from gems/gems/jekyll-3.9.0/lib/jekyll/liquid_renderer/file.rb:29:in `block in render!'
        36: from gems/gems/jekyll-3.9.0/lib/jekyll/liquid_renderer/file.rb:42:in `measure_bytes'
        35: from gems/gems/jekyll-3.9.0/lib/jekyll/liquid_renderer/file.rb:30:in `block (2 levels) in render!'
        34: from gems/gems/liquid-4.0.3/lib/liquid/template.rb:220:in `render!'
        33: from gems/gems/liquid-4.0.3/lib/liquid/template.rb:207:in `render'
        32: from gems/gems/liquid-4.0.3/lib/liquid/template.rb:242:in `with_profiling'
        31: from gems/gems/liquid-4.0.3/lib/liquid/template.rb:208:in `block in render'
        30: from gems/gems/liquid-4.0.3/lib/liquid/block_body.rb:82:in `render'
        29: from gems/gems/liquid-4.0.3/lib/liquid/block_body.rb:103:in `render_node_to_output'
        28: from gems/gems/liquid-4.0.3/lib/liquid/tags/for.rb:79:in `render'
        27: from gems/gems/liquid-4.0.3/lib/liquid/tags/for.rb:150:in `render_segment'
        26: from gems/gems/liquid-4.0.3/lib/liquid/context.rb:123:in `stack'
        25: from gems/gems/liquid-4.0.3/lib/liquid/tags/for.rb:158:in `block in render_segment'
        24: from gems/gems/liquid-4.0.3/lib/liquid/tags/for.rb:158:in `each'
        23: from gems/gems/liquid-4.0.3/lib/liquid/tags/for.rb:160:in `block (2 levels) in render_segment'
        22: from gems/gems/liquid-4.0.3/lib/liquid/block_body.rb:82:in `render'
        21: from gems/gems/liquid-4.0.3/lib/liquid/block_body.rb:103:in `render_node_to_output'
        20: from gems/gems/liquid-4.0.3/lib/liquid/tags/unless.rb:10:in `render'
        19: from gems/gems/liquid-4.0.3/lib/liquid/context.rb:123:in `stack'
        18: from gems/gems/liquid-4.0.3/lib/liquid/tags/unless.rb:14:in `block in render'
        17: from gems/gems/liquid-4.0.3/lib/liquid/block_body.rb:91:in `render'
        16: from gems/gems/liquid-4.0.3/lib/liquid/block_body.rb:103:in `render_node_to_output'
        15: from gems/gems/jekyll-3.9.0/lib/jekyll/tags/include.rb:137:in `render'
        14: from gems/gems/liquid-4.0.3/lib/liquid/context.rb:123:in `stack'
        13: from gems/gems/jekyll-3.9.0/lib/jekyll/tags/include.rb:140:in `block in render'
        12: from gems/gems/jekyll-3.9.0/lib/jekyll/liquid_renderer/file.rb:28:in `render!'
        11: from gems/gems/jekyll-3.9.0/lib/jekyll/liquid_renderer/file.rb:49:in `measure_time'
        10: from gems/gems/jekyll-3.9.0/lib/jekyll/liquid_renderer/file.rb:29:in `block in render!'
         9: from gems/gems/jekyll-3.9.0/lib/jekyll/liquid_renderer/file.rb:42:in `measure_bytes'
         8: from gems/gems/jekyll-3.9.0/lib/jekyll/liquid_renderer/file.rb:30:in `block (2 levels) in render!'
         7: from gems/gems/liquid-4.0.3/lib/liquid/template.rb:220:in `render!'
         6: from gems/gems/liquid-4.0.3/lib/liquid/template.rb:207:in `render'
         5: from gems/gems/liquid-4.0.3/lib/liquid/template.rb:242:in `with_profiling'
         4: from gems/gems/liquid-4.0.3/lib/liquid/template.rb:208:in `block in render'
         3: from gems/gems/liquid-4.0.3/lib/liquid/block_body.rb:91:in `render'
         2: from gems/gems/liquid-4.0.3/lib/liquid/block_body.rb:103:in `render_node_to_output'
         1: from gems/gems/jekyll-3.9.0/lib/jekyll/tags/include.rb:128:in `render'
gems/gems/jekyll-3.9.0/lib/jekyll/tags/include.rb:67:in `validate_file_name': Invalid syntax for include tag. File contains invalid characters or sequences: (ArgumentError)

  social-icons/.svg

Valid syntax:

  {% include file.ext param='value' param2='value' %}
```
{% endraw %}


Wtf? What did I do?

I check what happened between the commits and found out that it's not what I did. It is actually Minima, my Jekyll theme.

A closer look to the issue shows that my social icons are broken.

If you check [here](https://github.com/jekyll/minima#social-networks) you can see that Minima has changed the social media API.

It used to be like this:

```
social_links:
  github: hyrtsi
```

and nowadays it is

```
social_links:
    - { platform: github, user_url: "https://github.com/hyrtsi" }
```

After the changes try to make a local build:
```
bundle exec jekyll serve
```

then open it in localhost: `http://127.0.0.1:4000`.

I found out some discussion [here](https://github.com/jekyll/minima/issues/687) and [here](https://talk.jekyllrb.com/t/jekyll-fails-to-build-site-i-dont-think-i-changed-anything/7795).

I hope this helps. Always remember to check after each commit on your Github pages repo that it builds.