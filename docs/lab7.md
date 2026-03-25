# Lab 7

The objective of this lab is to implement a Kalman filter algorithm to improve performance for Lab 5's task of approaching a particular distance from a wall (1 meter) as quickly as possible from various distances (2-4 m) by refining distance data from the front-facing Time-of-Flight (ToF) sensor. 

## Setup

<!-- TODO: (optional) I may put something about troubleshooting sensor distance modes -->

Aside from rewriting some old functions to use less data and save space, I have made no meaningful changes to my robot, or regarding sending/receiving messages via BLE between my laptop and my car robot, since the previous lab. 

## Estimating drag and momentum

The first step was to estimate drag and momentum terms for the A and B matrices. I wrote a program that measures motor PWM inputs and ToF distance measurements over time. During data collection, the car is set to run at a wall at a constant motor input. I used a 200 PWM input to be consistent with Lab 5 conditions. To avoid damaging the car, I put up a foam sheet and added active breaking in the code when the car gets within ~1 meter of the wall. Below is a brief video demonstration:

<!-- URL: https://youtu.be/V1TT1EpXGjo -->
[![car running into wall](https://img.youtube.com/vi/V1TT1EpXGjo/0.jpg)](https://www.youtube.com/watch?v=V1TT1EpXGjo)

As shown in the video, the active braking and foam were very effective in minimizing damage to my car. Below is my collected motor PWM input and measured TOF distance data: 

<!-- TODO: insert PWM input vs time -->
![PWM values](./docs/lab7/T1_input_speed_vs_time.png)
<!-- TODO: insert distance vs. time -->
![distance values](./docs/lab7/T1_distance_vs_time.png)

I then converted distance measurements to velocity values in Python by taking differences between adjacent data points at each measurement time. Below is the aforementioned Python code and the resulting graph, annotated to highlight target values:

<!-- TODO: insert Jupyter code of converting distance data to velocity data -->
![velocity code](./docs/lab7/T1_velocity_code.png)
<!-- TODO: insert calculated car velocity vs. time -->
![velocity values](./docs/lab7/T1_velocity_vs_time.png)

Based on this data, my steady-state velocity seems to be approximately 2.610 meters per second. At 90% rise time around ~1.05 seconds, the speed was around 2.349 meters per second. It follows that the drag and momentum terms are found as follows:

<!-- TODO: insert calculation of d and m -->
![calculations](./docs/lab7/T1_dm_calcs.png)