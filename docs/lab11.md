# Lab 11

The objective of this lab is to bring localization with Bayes filter from the simulation in Lab 10 to our physical car robots. 

## Lab Tasks

After setting up the base code, I tested the localization in simulation. After running the base code in the provided simulation notebook, the simualtor generated the following plot of ground truth position (green), sensor model data (red), and updated Bayes filter estimate (blue). The filter output seems like a reasonable estimate of the true path, so this is performing as expected.

![localization in simulation](./lab11/sim_test.png)

In order to perform the update step, I first had to implement two member functions in the RealRobot class. 

### get_pose

This method returns the current robot pose based on odometry. I adjusted the implementation a bit and hard-coded the four locations in the lab setup, converting units from feet to meters. 

![get_pose](./lab11/get_pose.png)

### perform_observation_loop

This method collects the distance data at different angles about the robot's center of rotation in 20-degree increments. Here I use the function from Lab 9 and a notification handler to populate and return an ordered list of distance measurements. The while-loop keeps the thread waiting until all data is sent from the robot. 

![perform_observation_loop](./lab11/perform_observation_loop.png)

## Results

<!-- TODO: put in some generalized overview of results -->

<!-- TODO: in addition to data analysis discussion, collect the following data at each point:
    -> polar plot of raw ToF data at each intended angle, ignoring rotation inaccuracy (for now)
    -> plot of Bayes filter returned position vs actual position on the grid
    -> video of data collection at that point
-->

My initial run bore pretty poor results... it seems like the same off-world point is being predicted every time. Below I show the polar plot of measured distance (in mm) at each angle (in degrees) and the resulting localization prediction from the Bayesian filter.

### Pose 1: (-3 ft, -2 ft, 0 deg)
![plot at pose 1](./lab11/plot0.png)
![prediction at pose 1](./lab11/point0.png)

### Pose 2: (0 ft, 3 ft, 0 deg)
![plot at pose 2](./lab11/plot1.png)
![prediction at pose 2](./lab11/point1.png)

### Pose 3: (5 ft, -3 ft, 0 deg)
![plot at pose 3](./lab11/plot2.png)
![prediction at pose 3](./lab11/point2.png)

### Pose 4: (5 ft, 3 ft, 0 deg)
![plot at pose 4](./lab11/plot3.png)
![prediction at pose 4](./lab11/point3.png)

