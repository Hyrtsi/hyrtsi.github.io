---
layout: post
title: "List of interesting personal projects for software developers"
date: 2022-10-17 09:47:25 +0200
tags: programming project studying
---

I have done personal projects since I learned programming.
I think you should do it too:
- You learn programming that way
- You get a nice portfolio
- It's motivating
- The output of your programs may benefit yourself or someone else
- You get to meet and collaborate with awesome people if you make your projects public

I have too many project ideas and too little time so I have to prioritize the projects and only do the most interesting and fulfilling projects.
Here are some of my ideas that I'll do when I have time.
Feel free to pick any of these and have fun!
These are in no particular order.
There are ideas for complete beginners and also for more seasoned programmers.
I will update this list every now and then.

If you need inspiration elsewhere I suggest you to check this:
- https://github.com/sindresorhus/awesome
- https://github.com/StanForever/awesome-websites
- 

## Robotics path planner

Implement path planning algorithm for a robot.
The robot may have any kind of kinematics and locomotion:
- Wheeled robot
  - Omnidirectional
  - Steer drive ("bicycle kinematics")
  - Differential drive (like a armored tank)
- Robot with two legs
- Flying robot

The enviroment may vary:
- 2D or 3D
- Maze
- A forest with hills, trees, lakes, rocks
- A city with buildings, streets, cars, bridges

The goals may vary
- As short path as possible
- Minimize energy consumption
- 

# Computer graphics and games

## Raytracer or pathtracer or raymarcher or marching cubes

## Physics engine

## Graphics engine

## Game engine

# Compute

It's fun to calculate things parallel really fast on your CPU or GPU.
Here are some ideas for using your GPU for computing things rather than rendering the screen.
I'm myself mostly focused on Nvidia stack but there is a big need for alternatives for that.
You could be a pioneer in that movement.

You can check [this](https://ppc.cs.aalto.fi/) free online course if you need more knowledge about parallel computing in general.

## CUDA compute library

Most of us don't know CUDA. So maybe it's time to study a little bit.
Read a few tutorials, update your drivers and try to get some crunching done on your GPU.
Ideas using CUDA:
- Calculate matrix products
- Implement a simple game such as chess or go
- Learn how to train or infer a neural network or similar
- Raytracing or path tracing
- Physics simulations

Tutorials:
- https://github.com/romain-jacotin/cuda
- https://github.com/mikeroyal/CUDA-Guide
- https://github.com/NVIDIA/cuda-samples


# Music

## A software synthetizer

I've managed to create sound in real time using Python and C++.
So there you go.
I have proven myself that it's possible to create a synth by programming.
You should know basic signal processing for this one but you'll learn most of it on the way.

Suggested readings: everything by Julius O. Smith.

Features in the synth:
- A CLI UI or a graphical one
  - You should multithread the sound engine and the UI so you can use both at the same time
  - Don't worry. Threading isn't as scary as it sounds
- Different waveforms
- Filters
- A keyboard
- Possibility to save and load configurations
- Play sounds from .wav or .mp3 or similar
- Some kind of sequencer
- Effects such as chorus, flanger, reverb, delay, distortion