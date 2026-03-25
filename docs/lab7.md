# Lab 7

The objective of this lab is to implement a Kalman filter algorithm to improve performance for Lab 5's task of approaching a particular distance from a wall (1 meter) as quickly as possible from various distances (2-4 m) by refining distance data from the front-facing Time-of-Flight (ToF) sensor. 

## Setup

<!-- TODO: (optional) I may put something about troubleshooting sensor distance modes -->

Aside from rewriting some old functions to use less data and save space, I have made no meaningful changes to my robot, or regarding sending/receiving messages via BLE between my laptop and my car robot, since the previous lab. 

## Estimating drag and momentum

The first step was to estimate drag and momentum terms for the A and B matrices. I wrote a program that measures motor PWM inputs and ToF distance measurements over time. During data collection, the car is set to run at a wall at a constant motor input. I used a 200 PWM input to be consistent with Lab 5 conditions. To avoid damaging the car, I put up a foam sheet and added active breaking in the code when the car gets within ~1 meter of the wall.

<!-- TODO: insert code snippet here -->
<!-- I will probably just put pseudocode here instead of the actual loop -->

Below are the results of my procedure and a brief video demonstration:

<!-- TODO: insert car ramming video -->
<!-- TODO: insert PWM input vs time -->
<!-- TODO: insert distance vs. time -->

I then converted distance measurements to velocity values in Python by taking differences between adjacent data points at each measurement time.

<!-- TODO: insert Jupyter code of converting distance data to velocity data -->
<!-- TODO: insert calculated car velocity vs. time -->

<!-- TOOD: insert data analysis -->

Testing LaTeX in Markdown...?
$a+b=c$
$\forall n\in \mathbb N, \; \sum_{x=0}^n i = \frac{n(n+1)}{2}$