---
layout: post
title:  "Why are my changes on Github Jekyll site not visible?"
date:   2022-06-20 17:47:25 +0200
---

# Why are my changes on Github Jekyll site/blog not visible?

You have done a lot of changes and now it's time to check the results online. But there are no changes visible.


## Check the status of the pipeline

Github creates the `docs/_site` of your site automatically. Thus, you need to check the status of the pipeline:

- Go to the repository of your `<username>.github.io` pages
- Check `Environments` at the right and press `github-pages`

![environments]({{ site.baseurl}}/assets/git-pipeline-2.png)


There you can see the deployment status and the corresponding code hash. Make sure the status is `Active` and the code hash corresponds to the latest commit in the branch from which you deploy.

You can also check the commits of the branch from which you deploy. The latest commit should have a small green tick after it. If you hover your mouse on top of it this kind of information should appear.

![environments2]({{ site.baseurl}}/assets/git-pipeline-1.png)


## Check the Github Pages -related settings of your repository 

You can check the Github Pages -related settings by choosing `Settings -> Pages` in your repository or going to `https://github.com/<Your-username>/<your-username>.github.io/settings/pages`

Make sure that the branch and the folder are what you expect them to be. For me the branch is `master` and the folder is `/docs`.


## Check your changes locally

```
cd docs
bundle install
bundle exec jekyll serve
```

Then open `http://127.0.0.1:4000` on your browser. You may get a warning like this to your console

```
jekyll favicon.ico not found
```

but it's most likely a false positive. Your changes should be visible locally. If they are not then you have to modify your post until it works locally. This is the fastest way to test since you can refresh your page and see your changes immediately in your browser.


## Make sure your posts are in the correct place

Your posts should be in the folder from where you deploy. In my case the deploy folder is `/docs` and posts are in `docs/_posts`. You should not add `docs/_site` to source control or edit it.

## Make sure your posts are of correct file format

I sometimes forget to make my posts `.md` files. Remember that the filename must be `timestamp in YYYY-MM-DD-title.md` format. For example

```
2022-01-01-foo.md
```

## Make sure your posts have the correct header

The correct header looks like this

```
---
layout: post
title:  "How to train your dragon"
date:   2022-01-01 13:37:40 +0500
categories: foo
---
```


## Make sure you remembered to all add assets to source control

If you forgot to do that there will be no errors necessarily but the pages cannot be created.

```
git add docs/assets/foo.png
git commit -m "add foo.png"
git push
```

Remember that there is a size limit for a single file and there is a size limit for your whole repository!

# Make sure the times is correct in the header

I have once put mistakenly a time in the future

```
date: 2023-06-21 14:43:00 +0200
```

Make sure the time you put is now or in the past. Otherwise the post will show up later. If you can't find the error just pick a time in the past and check if that works.

## That's all

Thanks for reading till the end! I hope this article helped you deploying your Github pages!