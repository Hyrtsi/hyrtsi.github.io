---
layout: post
title: "Data bottleneck"
date: 2024-01-01 13:37:01 +0200
tags: ml datasets dataengineering
author: Eljas Hyyrynen
---

Data is 99% of machine learning.
There is discussion that we may run out of language data for LLMs as early as 2024 [`[source]`](https://epochai.org/blog/will-we-run-out-of-ml-data-evidence-from-projecting-dataset).
Thus, this poses a bottleneck for our neverending quest of training bigger and bigger ML models.

I got interested in data as I worked as a data engineer for automotive driving assistance system (ADAS) algorithms such as lane and pedestrian detection.
I learned how hard it is to find a good dataset to begin with!
The free datasets are kinda good but none of them is excellent.
[Open Images Dataset V7](https://storage.googleapis.com/openimages/web/index.html) is amazing but it's full of errors.
[COCO Dataset](https://cocodataset.org/) is the baseline for object detection but it has the same problem.
[Nuscenes and Nuimages](https://www.nuscenes.org/) has a wide variety of scenarios and labels but it has its problems, too.

I could go in depths listing what's wrong with each and every of these datasets but that's unnecessary now.
There are indeed solutions for the problem.
Cleanlab has released [a repository]([https://github.com/cleanlab/cleanlab]) that helps to clean labels automatically using existing state-of-the-art classifiers.
But that doesn't solve the problems of missing data.
[Augmentation](https://github.com/albumentations-team/albumentations) doesn't solve the entirety of the problem either but has earned its place in the ML engineers toolbox.
Nvidia offers [an interesting solution](https://arxiv.org/abs/2004.01294) for computer vision: to create new images of the old ones using novel view synthesis.
There are indeed numerous simulators that can be used for [gaze estimation](https://www.cl.cam.ac.uk/research/rainbow/projects/unityeyes/), [http://carla.org/](self-driving cars) and many more scenarios.

The root cause seems that machine learning is inherently data-hungry and creating high quality datasets is slow, time-consuming, expensive and somebody elses problem.
Or if we think outside the box the problem is that showing tens of millions of images doesn't guarantee that the model can generalize.

My own research question and contribution to this problem is: how do we identify which datasets are needed most urgently?
How do we make the data suppliers and users meet?
Where can you post requests for datasets?
Where can you advertise your dataset and possibly crowdsource its development?

For research groups of big corporations (Tesla, Meta, Google) the answer is clear: they create their own datasets.
But for smaller research institutes it's impossible to go to such heights.

I will be writing from the computer vision research point of view because that is the field that I have most experience and interest on.

I propose the following needs

# 1. Meta-need: a conclusive list of existing and lacking datasets

I have searched datasets myself using:
- [Kaggle](https://www.kaggle.com/)
- [Hugging Face](https://huggingface.co/)
- [Papers with code](https://paperswithcode.com/datasets)
- Github: [topics](https://github.com/topics/dataset), lists like [this](https://github.com/smuthubabu/awesome-public-datasets), [this](https://github.com/datasets) and [this](https://github.com/awesomedata/awesome-public-datasets)

and I can say that there are many.
But so many datasets are missing.
It's hard to find information of them.
I had to start by finding out what this problem is called.
Apparently it's the "data bottleneck in machine learning" but it may be called with many other names, too.

I tried:
- Asking ChatGPT about it
- Googling
- Searching on Reddit

There doesn't seem to be an official place for requesting publicly available datasets with different licensing options.
I'm seeing an opportunity here.
Mapping out the dataset needs would be interesting.
But how would I go about it?

## Manually

It goes like this.
Come up with a research field or topic:
- Detecting the bounding box and species of a bird from an image
- Medical imaginery such as detecting Alzheimers from a brain scan
- Detecting the time or geolocation from a scenery picture

Then search if there are datasets for those using popular search engines for web pages or datasets.
Both steps - listing the usecases and searching the datasets - are a lot of work!

## A crazy idea: automate it

What if: I compose a list of research and a list of datasets.
Then I compare these two somehow to see if there are certain keywords in research that don't occur in datasets.
It's not this simple.
Let's start with datasets.

Datasets can be divided int two parts: the annotations and the images.

### Images

Finding images is easy.
But finding images of specific topics?
Such as finding images of a specific species of bird, a PC with liquid cooling, a drone picture of Colorado River during solar eclipse.
Thus, we need a way to search through existing datasets and public images sources for these things and then to identify what is missing.
Since only imagination is a limitation there must be tons of possibilities for such scenes.
And here the image generation models such as DALL-E and Stable Diffusion come into play.
They can be employed in generating such datasets.
The fact that I like about Stable Diffusion is that it runs on consumer-grade PCs with decent GPUs.
I recently tested it on my RTX3080 and it worked extremely well!

### Annotations

Annotations 




Most commonly datasets have images (or videos) and labels.
I think we have enough images and videos on internet to take many leaps forward in computer vision research.
However, we easily think that just creating new datasets and collecting more data somehow magically solves the problems.
[Chani et al. (2022)](https://arxiv.org/abs/2211.07959) state that most commonly when faced a novel problem the solution is to create a new dataset for that problem specifically.
It means we end up duplicating some of the work.
It the case of computer vision it means that we are not only duplicating the effort of collecting and storing the images but some of the labels.





# Missing: fixing datasets


--

It is possible to train a neural network using not only existing datasets but also other neural networks:

- transfer learning
- creating labels using another NN model
- creating annotations or images using generative models


----

https://datasetninja.com/


---

References and to read

https://en.wikipedia.org/wiki/List_of_datasets_for_machine-learning_research
