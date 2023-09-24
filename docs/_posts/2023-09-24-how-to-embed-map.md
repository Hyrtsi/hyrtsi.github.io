---
layout: post
title: "How to embed Google My Maps to your Jekyll + Github Pages blog"
date: 2023-09-24 13:37:00 +0200
tags: googlemaps maps blogging
author: Eljas Hyyrynen
---

Maps are fun.
Sharing maps is sharing fun.

You have probably used Google's map services quite a lot.
They're the de facto of everday navigation.

Next I will show you how to share a map in your blog.
You could, of course, just render it to a image and share that.
But it doesn't have the following features:
- updates on your blog always when you update the map in [Google My Maps](https://www.google.com/maps/d/)
- supports panning and zooming
- possible to view in full screen
- supports toggling between satellite and map
- supports sharing the map link

Let's get to it.

1. Create a new map in Google My Maps or open an existing map.
2. Make the map public in the sharing menu
3. Click the three dots next to your map's name and choose "Embed"
4. Copy the HTML code to your page. It will look like this


{% raw %}
```
<iframe src="https://www.google.com/maps/d/u/0/embed?mid=12-1jR9eg9WrSYcYUdut1llUKNJB6q4U&ehbc=2E312F" width="640" height="480"></iframe>
```
{% endraw %}

5. Paste it to your page and you're done!

<iframe src="https://www.google.com/maps/d/u/0/embed?mid=12-1jR9eg9WrSYcYUdut1llUKNJB6q4U&ehbc=2E312F" width="640" height="480"></iframe>

# Bonus: how to embed Google Maps?

1. Search something on Google Maps.
2. Choose "Share"
3. Choose "Embed a map"
4. Copy the HTML. It will look like this

{% raw %}
```
<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3721.6970549469356!2d-11.404745524050453!3d21.124640684477146!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0xe811f822dce98ed%3A0x2165fb02a4108c6f!2sEye%20Of%20The%20Sahara!5e0!3m2!1sfi!2sfi!4v1695557376858!5m2!1sfi!2sfi" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>
```
{% endraw %}

5. Paste it to your page and you're done!

<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3721.6970549469356!2d-11.404745524050453!3d21.124640684477146!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0xe811f822dce98ed%3A0x2165fb02a4108c6f!2sEye%20Of%20The%20Sahara!5e0!3m2!1sfi!2sfi!4v1695557376858!5m2!1sfi!2sfi" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="no-referrer-when-downgrade"></iframe>

In this article you learned how to embed maps on your blog.
Note that even though you will mostly write your blog posts in Markdown, you will need to use HTML to embed maps.
You don't need to do anything special to mix HTML and Markdown in your blog posts if you're using Jekyll and Github pages.
Just write your Markdown and HTML as you would normally do.
This was confusing to me at first because I didn't know how the system works under the hood but it became super powerful after I got used to it.