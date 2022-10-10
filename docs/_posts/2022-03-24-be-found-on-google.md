---
layout: post
title:  "How to be found on Google"
date:   2022-03-24 20:47:25 +0200
categories: 
---

## How to make your `github.io` page visible on Google search:

- Go to [https://search.google.com](https://search.google.com)
- Add property
- Paste your github url there: `https://<username>.github.io`
  - Pay attention to the exact form that it's written in: `https://` is the oene that github pages use
- Download their html file. Let's call it `google-1234.html`.
- Add it to your website so that `https://<username>.github.io/google-1234.html` is reachable by their bot
  - This happens by merely adding the `.html` file provided to your project root
- Add your changes to git
  - `git add google-1234.html && git commit -m "foo" && git push`
- Open the html file using your browser in an incognito window
  - `https://<username>.github.io/google-1234.html`

These steps show google that you are administrating the website, not asking them to index someone elses website.

The next step is to add a `sitemap.xml`. It is a file showing all the pages on your website. It helps google's crawlers to reach all pages on your website. Similarly to the previous steps:

- Create the `sitemap.xml`
  - Use [this](https://www.xml-sitemaps.com/) or [this](https://www.mysitemapgenerator.com/) or [this](https://www.sureoak.com/seo-tools/google-xml-sitemap-generator) tool
  - You can also create the file manually. Just write all the links to all the pages you want to be indexed to a file. Check the format [here](https://www.google.com/gmail/sitemap.xml) if you need inspiration.
- Add it to your project and check that it works
  - Move the `sitemap.xml` to the folder where you build your website, for example `docs/`
  - `git add sitemap.xml && git commit -m "foo" && git push`
  - Test that you can open `https://<username>.github.io/sitemap.xml` with your browser
- Go to `Sitemaps` in [google search console](https://search.google.com/search-console/sitemaps)
  - Type the url to the `sitemap.xml` file, press `SEND`, hope for the best
  - It can take a few days for Google to tell you the results

That's it. Remember that you have to update the sitemaps when you add more pages.

## Further into SEO

This is just the basics of being found on Google search engine. You might want to study web crawling, search engines, SEO, web developing and more to further optimize your webpages. Google ranks the websites based on their content and popularity. If your page is reachable from the pages indexed (=known) by google they can find it after some time.

Make sure as many websites as possible link to your website and bring traffic there. I am thinking of creating posts of the topics I've discussed here on major Q&A websites such as Stackoverflow and Quora and linking this page there. I'm also going to start writing articles on Medium and linking back here.

I want to create short, simple and useful content and target especially on programming and tech newbies (I'm still one myself). Stay tuned for the next post!

Study more [here](https://developers.google.com/search/docs/fundamentals/seo-starter-guide).