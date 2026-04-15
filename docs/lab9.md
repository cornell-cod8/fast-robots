# Lab 9

The objective of this lab is to begin implementing motion planning concepts by using our Time-of-Flight (ToF) sensors to produce a map of the robot's surroundings. 

## Methodology

At a high level, I will use an orientation control approach. The robot will make a full rotation in place in increments of roughly 20 degrees and take distance measurements using the front-facing ToF sensor at each increment. 

To check for sensor and robot position frames, I took some additional measurements using a tape measure and a flat-edge with measurements accurate to the nearest 1/16 inch. These were used to find the front-facing ToF sensor frame, visualized in the diagram below:

![robot sensor position](./lab9/robot_sensor_position.png)

Here I take the robot's center as the origin (0,0), with the x-axis running along the robot's long side and the y-dimension along the short side. The front-facing ToF sensor is well-centered in the y-dimension, so I took this coordinate to be 0. The robot's long side is 7 inches long, and the front-facing ToF sensor is roughly 0.75 inch from the front of the robot, so this sensor is at the point (2.75, 0) in the robot frame. This would result in a simple translation matrix, since the robot and sensor always have the same orientation. Distance measurements taken from this sensor will be adjusted by adding a 2.75 inch offset to my measeurements before plotting on my generated map.

## Procedure

After confirming that my Lab 6 code still works for PID orientation control, I built on top of that for this lab. Using multiple iterations for the PID control, the robot would visit multiple angles at increments of 15 degrees. At each point, the robot took a distance measurement with the front-facing ToF sensor and a gyroscope measurement with the IMU's digital motion processor (DMP). The sensor was left in long-distance mode to better fit the conditions of the lab environment setup. This data was sent to my laptop in polar coordinates, and I added the aforementioned 2.75-inch offset to the distance measurements to generate a graph. 

For example, consider the following setup in my room. Labeled in the diagram is an origin point (O) and three vantage points (A, B, and C) from which to take distance measurements. 

<!-- TODO: picture of experimental environment setup -->

<!-- TODO: digital diagram in ms paint or something -->


Here is a video of the data collection process at point A:
<!-- TODO: data collection video -->

This resulted in the following data:
<!-- TODO: input motor speeds over time -->
<!-- TODO: raw ToF scan of distance vs time -->
<!-- TODO: polar plot of ToF scan  -->

## Error Analysis

My primary sources of error came from the ToF sensor and the rotation of the robot itself. Per the VL53L1X datasheet from Lab 3, the sensor error is estimated at around 20 millimeters in this environment's lighting. A possible source of rotational error came from the margin of error permitted for the rotations, which was within 1 degree. I tried to minimize this error by remeasuring the angle from the DMP output instead of using the intended angle; for instance, if a rotation intended to terminate at 100 degrees from the setpoint instead terminated at 99.5 degrees, I would record the angle for that data point as 99.5 degrees instead of 100 degrees.

Additionally, the central axis of the robot adjusted with each measurement as the wheels were not rotating at exactly the same speed. To find this error, I pointed the robot reasonably perpendicular to a wall and took distance measurements before and after a full 360 degree rotation. After 5 trials, I found the average difference was an increase of _ millimeters. The start and end of the movement were fairly smooth, so it seems reasonable to interpret this as a gradual change over the full rotation. Working with the same example as before, I tried applying an adjustment of x*_/18 to the xth measurement after the initial measurement at 0 degrees (so the offset increased linearly with a constant increase in x). This yielded the following adjusted diagram, which may fit the environment more comfortably: 

<!-- TODO: adjusted diagram -->

I also tried making multiple rotations. Here I show overlapping data from three consecutive rotations at point A in three different colors, as well as an estimation of a "true" point by averaging the angle and distance data for the pair of points measured at each increment. The second diagram also adds the previously-mentioned gradual adjustment.

<!-- TODO: multiple rotations diagram -->

## Results

Below is all of my collected data, reproducing the data from Point A for convenience. I decided to adjust for the rotation error at all three locations. 

Point A: 

<!-- TODO: input motor speeds over time -->
<!-- TODO: raw ToF scan of distance vs time -->
<!-- TODO: polar plot of ToF scan  -->

Point B: 

<!-- TODO: input motor speeds over time -->
<!-- TODO: raw ToF scan of distance vs time -->
<!-- TODO: polar plot of ToF scan  -->

Point C: 

<!-- TODO: input motor speeds over time -->
<!-- TODO: raw ToF scan of distance vs time -->
<!-- TODO: polar plot of ToF scan  -->

I start the robot at the same orientation in all three positions, so the transformation matrices were fairly straightforward two-dimensional translations. 

<!-- TODO: show work for all three transformation matrices -->

Applying these gave me the following completed map. I drew the lines manually to estimate the locations based on the collected sensor data. 

<!-- TODO: show completed map -->