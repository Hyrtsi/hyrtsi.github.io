---
layout: post
title:  "Learning program optimization as a total noob to the subject"
date:   2022-10-09 13:37:11 +0200
---

Optimization. What a cool word.

I have studied optimization as a mathematical phenomenon and that is interesting indeed.
But today we will talk about optimizing code with respect to speed, CPU/GPU utilization, memory and power consumption.
Moreover, we will lay the fundamentals for someone who doesn't know about the topic at all.
I will try to find the common ground between parallel processing, compute, the basics how CPUs, GPUs, SoCs work to begin with and how to get into the mindset of a high-performance programmer.

# The problem statement

Let's start with a simple one.
This is how I got into optimization.

You have a piece of code.
It is doing what you want but it's really slow and you want to make it faster.
How to do it?
The `why` is probably quite trivial.

## A practical example

Let's create an environment with circles and implement collision physics.
Each circle starts with the same radius and a random linear speed.
They collide without losing energy and thus the simulation can run arbitrarily long.
We use SFML to create a window and for rendering for simplicity.

