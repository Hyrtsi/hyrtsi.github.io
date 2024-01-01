---
layout: post
title: "Getting into open source development as a beginner"
date: 2024-01-01 13:37:00 +0200
tags: opensource
author: Eljas Hyyrynen
---

# The importance of open source software

When I first got interested in open source it was mostly because I liked the philosophy, I felt like I wanted to make a contribution and learn to be a better programmer.
I didn't realize that the whole global software ecosystem relies on open source.
Not just part of it, like you could opt out from using open source.
But the entirety: Linux kernel, Mozilla Firefox, Python programming language, GNU Compiler Collection, Git, ...
I think it's impossible for a software developer to avoid using the byproducts of the open source movement.

![On the shoulders of the giants. Dependency by xkcd.](https://imgs.xkcd.com/comics/dependency.png)

Isn't it beautiful?
The global software community is building something big together.
That makes me want to contribute.
Be part of it actively instead of just passively consuming their efforts and giving back nothing.
This is a guide on how to start contributing to open source without any prior experience.

# The trivial way of becoming an open source developer

The most obvious way to become an open source developer is to publish your own code.
Even before starting my professional programming career I put all my projects to Github.
Just to build the habit of doing so and to learn using a version control system and an online platform for that.
I didn't make each and every repository public and maybe you shouldn't do that either.
But for the ones you do, consider these things before publishing your code.

- Have you written as good code as you possibly can?
- Does your repository serve the purpose that it was built for?
- Can someone else learn something from your code?
- Can someone build something better based on your code?
- Do you save hours (or more) of someone elses time if you publish your code?
- Does your repository use the code and other IP of other people in a proper way?
- Does your repository represent your personal image and reputation well?
- How do you want to license your code?

## About the last point: licensing.

Every software developer should know the basics of licensing options.
When I started developing they seemed like unnecessary or confusing.
But I'm going to say this one more time: you MUST always take stance on the licensing of your code and the licensing of the external code you use.
Please refer to [this](https://en.wikipedia.org/wiki/Software_license), [this](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository), [this](https://opensource.guide/legal/) and [this](https://choosealicense.com/licenses/) guide and ask your favorite search engine or LLM to clarify the points you didn't understand.
I personally recommend using ["copyleft"](https://en.wikipedia.org/wiki/Copyleft) or "viral" licenses as much as you can.
However, the license choice should always be considered based on the big picture.
The bigger your software project is and the more developers and users you have the more important the choice is.
For your own little hobby project that nobody ever discovers it doesn't really matter.
I used to always use MIT license but now I'm possessed of the idea of the license that spreads open sourceness like a virus.

# Studying repositories from Github

Github is my favorite platform for both storing and managing my repositories but also for "social" programming.
I am following other developers there and seeing their latest activity.
I'm checking interesting repositories by topic, browsing through their code, commits, issues, PRs and discussions.
The easiest way to learn coding and get into open source is to browse through lots and lots of repositories.
- See which languages are used for which tasks.
- Who are the contributors and how many of them there are.
- Which repositories are popular and which are unpopular.
- Also think the reason why this may be the case.
- Which repositories are easy to read, understand and get into, which are difficult?

A single repository is always answering to one question or one problem.
I mean, obviously they have broken down that into myriads of smaller parts.
But [tensorflow](https://github.com/NVIDIA/tensorflow) is "just" a machine learning framework.
It's not a game engine.
And [godot](https://github.com/godotengine/godot) is a game engine, not a graphics driver.
If you understand the scope of a repository well you're on your way to become a really good open source developer.
Because the next step is to identify your key skills, programming languages, frameworks and tools.

# Getting into a niche

Focus on a narrow skillset for a while.
Maybe as your new years resolution decide a framework, programming language or a piece of technology that you'll focus especially during the next year.
It may have surprising benefits to your skills, status on the job market and your future as a developer.
I'm focusing on c++, shaders and audio programming next year because I can't stick into one thing.
If you have a focus it's easier to search for projects to contribute to.

# Pick a project you like

I like [yolov5](https://github.com/ultralytics/yolov5).
I think it's a successful open source project:
- It's widely adopted
- It has an excellent README
- The author, Glenn Jocher, is a fantastic person
- It has lots of contributors
- It's focused on a narrow, well defined problem (object detection)
- It's been actively maintained and developed
- It has lots of questions, improvement ideas and PRs
- It has proven to be useful in many tasks

I did in fact study and use this project in my work for a while.
I learned a lot from reading the source code and getting deeper in the project.
I recommend the same for you: pick a project and dive deeper into it.
Take part in the discussion.
Read the source code.
Try to understand what it does.
Try to find out its limitations.
And finally, make a contribution to it.
Add a valuable piece of functionality or a fix to it.
I myself found out that when using segmentation and filtering classes in `yolov5` it doesn't work as expected.
I was playing with the repository, I had a clear idea in my mind how I wanted to use the project and found out a bug.
That's how I got my code into one of the most popular machine learning projects in the industry.

# Find a community

I used to be a lone wolf.
Just programming on my own, not interested in getting friends, contacts, communities or such around me.
It's good for a while but long term your progress will stall if you isolate yourself from the big picture.

You can find likeminded programmers in many places: Discord, Telegram, Twitter (X), various forums, IRC, real life events, webinars, LinkedIn, universities, volunteering programs, companies, gamejams, hackathons, via friends.
I suggest you to team up with some people.
The benefits are immense.
I learned programming myself via my best friend. 
He is an excellent c++ programmer, I was passionate to learn and he wanted to teach me.
I was so lucky - later he got me my first c++ job and that started my career quite well.

Making articles or blog posts online, emailing research paper authors about mistakes in their publications, posting on Stackoverflow, sharing cool projects in social media.
These all are ways to contribute to open source.
The main thing is to become interested and involved in these projects.

# The balance between two opposite forces

The most liked side of open source software is that it's free and accessible as opposed to closed-source software.
In addition to curious minds, students and non-profit organizations companies love this.
Let me put it this way: when working in a company, how many times did you purchase a commercial product instead of using an open source alternative?
In my case we almost never bought commercial products when there were good open source alternatives available.
It's just faster, cheaper and the way we used to do things.

So I'm raising a point where companies are using the efforts of open source developers to create a commercial product.
Is this right or wrong?
If the license terms are followed then it's totally legal and thus "right".
However, I think that there are a few ways in which we all could be more active and thus improve the ecosystem.

## Report all the bugs you find from each and every piece of 3rd party software you use.

If you paid for a piece of software and it has a bug what would you do?
Of course you would report the bug.
The same goes for open source.
You want to improve the experience for yourself and everyone else.
So the least you could do is to report the bugs to "pay" for usage of the product.

## Share the forks, improvements, modifications and derivatives of the product if it's possible

I don't think I need to describe this one.
Share all useful derivatives of the existing work if your intended licensing for your derivative work allows it.

## Share all the knowledge, improvement ideas and such you learned with the community

I've used my fair share of time to study pytorch, tensorflow, SFML, TensorRT and yolov5.
Thus I have gathered some amount of information about the source code, usecases, applications and common problems.
Since the aforementioned projects have been done cleverly there is a medium for discussing about them.
In this case, a forum, GitHub issue and PR trackers are the right places to ask and answer questions.

## Software is the best when used

Open sourcing a piece of software increases the amount of users and thus testers, potentially paying customers, reputation, visibility and people benefiting from it.
While companies may use open source to just "eat and run without paying", they will also contribute to the growth of the project.
If nobody is using the project how can it exist?
Software is always tied to the world surrounding it.
Some frameworks bloom, peak and then they're forgotten.
Some tend to stick forever since they're so fundamental and widely adopted.
This is the life cycle of software.

My dream is to one day make a significant contribution to the software ecosystem.
It would be glorious to have my code stay for a long period of time in the flow.

# Conclusion

Just spend a lot of time with projects you like and you'll end up making a change that matters.
One day the outcomes of your big and tiny actions will shape the software projects of the humankind towards something better.
I wish this motivated you to open that browser or IDE and start working.
