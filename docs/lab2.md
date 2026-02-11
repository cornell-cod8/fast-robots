## Lab 2

For this lab, we started working with the IMU sensor and the RC car. 

# Setup

![setup](/docs/lab2/setup.png)

After installing the appropriate Arduino library for the IMU and connecting it to the Artemis Nano board using QWIIC connectors, I tested its functionality using the Example1_Basics file from the Arduino library. The AD0_VAL definition refers to the last bit of the address for the IMU, which corresponds to reading or writing data, so I left it at its default value of 1. 

Per the recommendation in the instructions, I also added a simple signal to indicate that the board is running. The onboard LED is set to flash 3 times at start of program execution. 

In the recording below, program execution works as intended. First I show gyroscope data by rotating the IMU across all three axes, with appropriate positive increase in positive rotation directions. Then I show accelerometer data using the serial plotter, with acceleration along the X, Y, and Z axes plotted on the pink, blue, and yellow lines, respectively. As I translated and rotated the IMU with each axis, aside from some noise in the sensor readings (and my shaky hand), the sensor readings adjusted appropriately for the accelerometer and gyroscope data. 

<!-- TODO: discuss acceleration/gyroscope data more...? -->

[![Example1_Basics](https://img.youtube.com/vi/-GwwxylbFAQ/0.jpg)](https://www.youtube.com/watch?v=-GwwxylbFAQ)


# Accelerometer

Working with the accelerometer now, my first task was to use the sensor data to find pitch and roll using the equations from class. Below I reproduce those equations:

![accelerometer equations](/docs/lab2/acc_equations.png)

Followed by my implementation in Arduino, maintained as global variables: 

![accelerometer code](/docs/lab2/acc_code.png)

And here is a short clip of my output with pitch of -90 degrees and roll of +90 degrees:

https://youtu.be/jJOOMa5cFIo?si=iSStjftr6LQj-K4M

[![Pitch and roll test](https://img.youtube.com/vi/jJOOMa5cFIo/0.jpg)](https://www.youtube.com/watch?v=jJOOMa5cFIo)

To evalute the accuracy of the IMU accelerometer, I measured the output of the pitch and roll functions where outputs of -90, 0, and 90 
degrees were expected, with results displayed in the graphs below. Based on this data, the accelerometer seems to calculate the pitch and roll at these extreme values pretty accurately. I had very small mean error of less than 1 degree, so there was little need for offset adjustments. There was notably much smaller amounts of noise for roll than pitch shown on the Fast Fourier transform plots. 

![pitch plus 90](/docs/lab2/pitch_plus_90.png)
![pitch minus 90](/docs/lab2/pitch_minus_90.png)
![pitch fft](/docs/lab2/pitch_fft.png)

![roll plus 90](/docs/lab2/roll_plus_90.png)
![roll minus 90](/docs/lab2/roll_minus_90.png)
![roll fft](/docs/lab2/roll_fft.png)

Based on this data, it seems like a good choice for cutoff frequency might be around 5 Hz because there doesn't seem to be useful data beyond this point for either axis. Implementing a simple low-pass filter using this cutoff frequency, I had a sampling frequency of about 350, so my alpha-value for my chosen cutoff frequency came to about 0.994. There wasn't much noise in the roll measurements to begin with, so there was little change for that axis as anticipated. There was some noticeable small change in noise in the pitch readings. 

![filtered pitch plus 90](/docs/lab2/filtered_pitch_plus_90.png)
![filtered pitch minus 90](/docs/lab2/filtered_pitch_minus_90.png)
![filtered pitch fft](/docs/lab2/filtered_pitch_fft.png)

![filtered roll plus 90](/docs/lab2/filtered_roll_plus_90.png)
![filtered roll minus 90](/docs/lab2/filtered_roll_minus_90.png)
![filtered roll fft](/docs/lab2/filtered_roll_fft.png)

# Gyroscope 



# Sample Data

For my data collection in this lab, I used the second approach from last lab to collect and store all the data in global arrays before sending it to my laptop via Bluetooth. After removing all print statements and removing my while-wait for IMU updates, the fastest collection I measured was around 350 datapoints collected per second. It looks like my main loop outruns the IMU data updates, as my main delay was in accounting for that update speed. 

A major contributor is likely due to the fact that this was while I was only measuring one dimension at a time for the accelerometer. I maintained 2 global 1000-element array for time and sensor data, and the Artemis had more than enough space with 384kB to store 8,000 bytes, plus three more global floats used for updating variables, for a total of 8,012 bytes. 

For the gyroscope, I recorded everything at once, but maintained three separate arrays for minor convenience. I think it would make more sense to store everything in one big array because keeping the entire array stored in one location avoids minor slow-downs from multiple searches for arrays that may be stored in multiple locations. 

For the data type, floats made the most sense as accuracy of data is important in state estimation, and small error can propagate to large miscalculations depending on control setting. 

Below I demonstrate 5 seconds of IMU data collected at once:

<!-- put 5 sec video here -->

# Record a Stunt

I tried out the RC car to finish out the lab. It was suprrisingly easy to flip the robot over itself with forward momentum. I tried to do a wheelie, but I didn't have the delicate touch for it. I was able to get it to roll onto its side and get back up, though. 

<!-- put stunt video here -->

