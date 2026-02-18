## Lab 2

For this lab, we started working with the IMU sensor and the RC car. 

# Setup

![setup](./lab2/setup.png)

After installing the appropriate Arduino library for the IMU and connecting it to the Artemis Nano board using QWIIC connectors, I tested its functionality using the Example1_Basics file from the Arduino library. The AD0_VAL definition refers to the last bit of the address for the IMU, which corresponds to reading or writing data, so I left it at its default value of 1. 

Per the recommendation in the instructions, I also added a simple signal to indicate that the board is running. The onboard LED is set to flash 3 times at start of program execution. 

In the recording below, program execution works as intended. First I show gyroscope data by rotating the IMU across all three axes, with appropriate positive increase in positive rotation directions. Then I show accelerometer data using the serial plotter, with acceleration along the X, Y, and Z axes plotted on the pink, blue, and yellow lines, respectively. As I translated and rotated the IMU with each axis, aside from some noise in the sensor readings (and my shaky hand), the sensor readings adjusted appropriately for the accelerometer and gyroscope data. 

[![Example1_Basics](https://img.youtube.com/vi/-GwwxylbFAQ/0.jpg)](https://www.youtube.com/watch?v=-GwwxylbFAQ)


# Accelerometer

Working with the accelerometer now, my first task was to use the sensor data to find pitch and roll using the equations from class. Below I reproduce those equations:

![accelerometer equations](./lab2/acc_equations.png)

Followed by my implementation in Arduino, maintained as global variables: 

![accelerometer code](./lab2/acc_code.png)

And here is a short clip of my output with pitch of -90 degrees and roll of +90 degrees:

[![Pitch and roll test](https://img.youtube.com/vi/jJOOMa5cFIo/0.jpg)](https://www.youtube.com/watch?v=jJOOMa5cFIo)

To evalute the accuracy of the IMU accelerometer, I measured the output of the pitch and roll functions where outputs of -90, 0, and 90 degrees were expected, with results displayed in the graphs below. Based on this data, the accelerometer seems to calculate the pitch and roll at these extreme values pretty accurately. I had very small mean error of less than 1 degree, so there was little need for offset adjustments. There was notably much smaller amounts of noise for roll than pitch shown on the Fast Fourier transform plots. 

![pitch plus 90](./lab2/pitch_plus_90.png)
![pitch minus 90](./lab2/pitch_minus_90.png)
![pitch fft](./lab2/pitch_fft.png)

![roll plus 90](./lab2/roll_plus_90.png)
![roll minus 90](./lab2/roll_minus_90.png)
![roll fft](./lab2/roll_fft.png)

Based on this data, it seems like a good choice for cutoff frequency might be around 5 Hz because there doesn't seem to be useful data beyond this point for either axis. Implementing a simple low-pass filter using this cutoff frequency, I had a sampling frequency of about 350, so my alpha-value for my chosen cutoff frequency came to about 0.994.

![filtered acc](./lab2/acc_filter_code.png)

There was some small, but noticeable reduction in noise in the pitch readings. 

![filtered pitch plus 90](./lab2/filtered_pitch_plus_90.png)
![filtered pitch minus 90](./lab2/filtered_pitch_minus_90.png)
![filtered pitch fft](./lab2/filtered_pitch_fft.png)

There wasn't much noise in the roll measurements to begin with, so there was little change for that axis as anticipated. 

![filtered roll plus 90](./lab2/filtered_roll_plus_90.png)
![filtered roll minus 90](./lab2/filtered_roll_minus_90.png)
![filtered roll fft](./lab2/filtered_roll_fft.png)

# Gyroscope 

For the gyroscope data, I implemented the differential equation shown below in the subsequent code snippet: 

![gyroscope equation](./lab2/gyro_equation.png) 

![gyroscope code](./lab2/gyro_code.png)

I first compared the resting measurements with no IMU movement. The gyroscope data had significantly less measurement noise, but since the roll and pitch were continuously incremented by their previous values while no movement occurred, they had substantial drift. 

![gyroscope rest](./lab2/gyro_rest.png)

Delaying the sample frequency by ~100ms seemed to lower variance further to an extent, as this data was generally more linear. 

![gyroscope rest with +100ms delay](./lab2/gyro_rest_delay.png)

To counteract the drift, I implemented a complementary filter to combine the gyroscope and filtered accelerometer data. I retained the same low-pass parameter of 0.935, and eventually settled on a parameter of 0.4 for the complementary filter based on performance during long critical points in resting performance. Compared to the previous methods, this seemed to combine the solid baseline of the accelerometer accuracy with the gyroscope precision while minimizing drift.

![complementary filter](./lab2/complement_waves.png)

![commplementary filter](./lab2/complement_rest.png)

![commplementary filter](./lab2/complement_pitch_plus.png)

![commplementary filter](./lab2/complement_roll_plus.png)



# Sample Data

For my data collection in this lab, I used the second approach from last lab to collect and store all the data in global arrays before sending it to my laptop via Bluetooth. After removing all print statements and removing my while-wait for IMU updates, the fastest collection I measured was around 350 datapoints collected per second. It looks like my main loop outruns the IMU data updates, as my main delay was in accounting for that update speed. 

A major contributor is likely due to the fact that this was while I was only measuring one dimension at a time for the accelerometer. I maintained 2 global 1000-element array for time and sensor data, and the Artemis had more than enough space with 384kB to store 8,000 bytes, plus three more global floats used for updating variables, for a total of 8,012 bytes. 

For the gyroscope, I recorded everything at once, but maintained separate arrays for no particular reason. I think it would make more sense to store everything in one big array because keeping the entire array stored in one location avoids minor slow-downs from multiple searches for arrays that may be stored in multiple locations. Additionally, as these attributes would generally be accessed all at once for general locomotive purposes, it would be convenient for the data to be in the sam place. 

For the data type, floats made the most sense as accuracy of data is important in state estimation, and small error can propagate to large miscalculations depending on control setting. 

The accelerometer data, as well as most of the gyroscope data, was collected with a large sample size of 1000. Particularly, the gyroscope data before applying the complementary filter clearly demonstrates capturing for at least 5 seconds without the sampling rate delay, and for at least 10 seconds with the delay. 

# Record a Stunt

I tried out the RC car to finish out the lab. It was very difficult to control at first as it accelerated very quickly to full speed, which was convenient for long drives up and down the hallway, but not so much for the precision needed for tricks, especially with the basic controller sensitivity. 

It was suprrisingly easy to flip the robot over itself with forward momentum. I was able to get a spinning wheelie, but I was unfortunately not able to reproduce it on camera. 

[![Car stunt](https://img.youtube.com/vi/-nr-Zjzp4ws/0.jpg)](https://www.youtube.com/watch?v=-nr-Zjzp4ws)