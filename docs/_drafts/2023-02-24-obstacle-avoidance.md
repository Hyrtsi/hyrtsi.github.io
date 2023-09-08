---
layout: post
title: "Path planning and obstacle avoidance"
date: 2023-02-24 13:37:00 +0200
tags: robotics path_planning algorithms
author: Eljas Hyyrynen
---

This is my favorite topic in robotics and autonomous driving.
I even did [my master's thesis](https://aaltodoc.aalto.fi/handle/123456789/33679) on it.
The punchline is: no vehicle can move autonomously without:
- Sensing the environment
- Classifying the measurements to drivable area, non-drivable area, other moving obstacles and static obstacles
- Updating the path according to above to achieve the target pose

I'm assuming the readers don't know much about the topic so I'm introducing some basic concepts.

The goal of this post is to present you on some hands-on c++ (and possibly ROS 2) examples on obstacle avoidance and path planning.

## Introduction

I will be referring to vehicles as `robot` whether it is an autonomous guided vehicle (AGV), self-driving vehicle, autonomous mobile robot (AMR) or even an aerial or naval unit.

The robot's location and orientation in the space is called `pose`.
Pose refers to 

$$ X = \begin{bmatrix} x \\ y \\ \theta \end{bmatrix} $$

in 2D space which we're focusing on in this article.

A trajectory is an ordered set of poses.
We are limited to trajectories that are attainable with the kinematic model of the robot.

`Kinematic model` means the mathematical description of the robot: its functional dimensions and degrees of freedom (DOF).
It describes the constraints under which the robot is operating in.
Examples of kinematic models are:
- Differential drive
- Steer drive (bicycle model)
- Omnidirectional

As we are only simulating robots we can choose which kinematic model we want or abstract it away totally.

## Dynamics

Control theory describes how the system is changing.
Change in dynamics means the velocity and the angular velocity of the robot:

$$U = (v, \Delta \theta)$$

<a name="dynamics"></a> Given a current pose and a control signal, a new pose can be calculated with this simple dynamics model:

$$X_{new} = \begin{bmatrix} x + v * \cos(\theta + \Delta \theta) \\ y + v * \sin(\theta + \Delta \theta) \\ \theta + \Delta \theta \end{bmatrix}$$

The velocity and turn rate can be limited to simulate a real robot.
We can get even more realistic by applying some of the kinematic models as well as using acceleration instead of velocity.

## Environment representation

Environments are usually represented by point clouds, a set of 2D or 3D points.
LIDARs capture the closest environment points for a given set of angles depending on their angular resolution.
The output is a histogram: set of distances for each angle.
The points can be transformed into `(x,y)` or `(x,y,z)` points by a coordinate space conversion (polar to cartesian).

We represent the environment with a set of points with a given radius for simplicity.
The environment is always fully known and observable and does not change.

In real world techniques such as Kalman filter, particle filter and probabilistic point clouds can be employed for this problem.

## Finding the shortest path using control and differentiable cost functions

Consider a robot in 2D space.
Its starting pose is $$X_{start}$$ and target $$X_{target}$$.
Assume that the target pose is reachable from the starting pose with the robot kinematics in finite amount of steps.

We can come up with a simple heuristic to reach the target by assigning `cost` for each point in space.
Since it's a continuous and differentiable function it is usually called the `cost function`.

In the first example we choose the function to be:

$$f(X, \tilde{X}) = || X - \tilde{X} ||$$ where $$||\cdot||$$ denotes the l2 loss.

Our goal is to find the optimal control signal for every given state (=pose). 
We use the following algorithm:
- Start with a control signal (random or zero)
- Calculate the gradient of the cost function with respect to the control
- Use gradient descent to minimize the cost
- After running the gradient descent steps clamp the control signal using maximum velocity and turn rate
- Use this control signal and dynamics to move the robot to the new state

(this will be elaborated in detail later)

This is a clever way to turn the path planning problem into an optimization problem.
Please don't continue further until you've understood at least [gradient descent](https://en.wikipedia.org/wiki/Gradient_descent) and [numerical derivatives](https://en.wikipedia.org/wiki/Numerical_differentiation).
The one I use for derivatives is

![This](https://wikimedia.org/api/rest_v1/media/math/render/svg/8c47753768b4e184080c6782a3ee8ca9d8174b0a)

Gradient descent for finding the minumum for a function is simply:
- Start somewhere
- Check the local neighbourhood to find which direction the function decreases the fastest
- Go a short step into that direction
- Continue from the second step until you've found a good enough solution or you don't get any better solutions anymore

or mathematically:

$$X_{n+1} = X_{n} - \alpha \nabla f(X_{n}), n \geq 0$$

We can apply this to calculate separately the derivatives for velocity and turn rate.
First we check what kind of effect a small change to either direction in the velocity has to the cost.
If we add a small number, let's say $$h=0.0001$$ to the current velocity and move one [dynamics step](#dynamics) forward using that control signal, we get $$X_{new}$$ as our state.
Thus, the cost in this state is $$f(X_{new})$$.

Similarly, we can decrease the velocity by $$h$$ and apply the dynamics to the original state to get $$X_{new, 2}$$ and cost $$f(X_{new, 2})$$

Then, using the equation for derivatives, we can approximate the derivative of the cost function at $$X$$ by $$\frac{f(X_{new}) - f(X_{new, 2})}{2h}$$

We can do the same thing by changing only the angular velocity $$\Delta \theta$$ and attain the partial derivative with respect to that varible.

I think this result is somehow magical.
We don't know the analytical solution to the equations and we don't have to.
The numerical derivative is in this case more than enough to move along the function to the minimae.
What is more convenient is that it's trivial to add more variables to the control signal, change the dynamics or cost function.
The baseline algorithm does not change - we simply update the equations.

We will begin by changing the cost function to take obstacles into consideration.
The cost must be somehow larger for obstacles to make the algorithm plan routes that go around the obstacles.