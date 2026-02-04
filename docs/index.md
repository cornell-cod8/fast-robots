# ECE 4160 Student Page -- Clarence Dagins (cod8)

## About me

![Me](/docs/headshot.jpg)

My name is Clarence Dagins. I'm a senior engineering student studying computer science. I'm passionate about robotics, especially in its intersections with formal verification methods and CS theory. Outside of academics, I enjoy personal electronics projects, automotive work, outdoor activities, old-school arcade fighting games, and cats. 

It's been a long time since I've done any web design, so I'm not too proud of this technological terror I've constructed... this will hopefully look nicer as the year progresses.

## Lab 1A

The first half of this lab was just getting familiar with using the Artemis board. For this part, I did not collaborate with anyone. 

Before lab, I installed ArduinoIDE and the Sparkfun Apollo3 boards manager on my computer to send programs to the board. The setup itself was fairly simple: I had the board connected to my laptop with ArduinoIDE open with a USB-C cable. The rest of lab involved running simple tests and sample programs to confirm functionality of communication with my laptop and of onboard sensors. Notably, I had to lower the baud rate to 9600 for the second and third tests in lab, but I did not have this issue at home despite being on the same machine... 

The first program in 01.Basics/Blink toggled an LED on/off once per second. Setup and execution was fairly straightforward. 

![01.Basics/Blink](https://youtube.com/shorts/8DN68hM2iHc?feature=share) 

The second program in Apollo3/Example4_Serial tested user interaction with the serial monitor with a simple echo program. Everything the robot received as input was returned through the serial monitor output. 

![Serial](https://youtu.be/ly9EcbCKW0U)

The third program in Apollo3/Example2_analogRead tested the onboard temperature sensor. Once this was done, the room temperature measurement was around . After holding the chip with my hands for a little while, I was able to get the reading to raise to around ____. 

![Temp1](/docs/lab1/temp1.png)

![Temp2](/docs/lab1/temp2.png)

The fourth program in PDM/Example1_MicrophoneOutput tested the onboard microphone. As shown in the video, the resting frequency in a quiet environment was at 0, raised to a steady 351 while I was humming, and fell again afterwards. 

![MicrophoneOutput](https://youtu.be/PfDuNnPpgM8)

## Lab 1B

Lab 1B

This part of the lab familiarized us with wireless communication with the Artemis board via Bluetooth stack, and sending Python commands to the board to execute Arduino code. For this section, I briefly discussed coding strategies during lab section with Rajarshi Das and Rushika Prasad, but did not collaborate with anyone otherwise. 

Before lab, I downgraded my installation of Python to 3.13 and set up a virtual environment to use for the course with the following packages: numpy, pyyaml, colorama, nest_asyncio, bleak, and jupyterlab. After downloading the codebase into my project directory, I installed ArduinoBLE in the ArduinoIDE, loaded and burned the provided ble_arduino.ino sketch from the codebase, and started the Jupyter server to send Python commands to the board during the lab. 

To connect the board to my laptop via Bluetooth, I updated the configuration file to match the board's MAC address and generated a new BLEService UUID to use so that I didn't mistakenly connect to anyone else's board during lab. After all of this, I reuploaded ble_arduino.ino to the board and used the provided demo.ipynb notebook to test basic BLE functionality. This was the combined list of command types I had, with the last 6 being added as I went along (described as they appear).

![command types](/docs/lab1/cmdtypes.png)

To complete the following tasks, I edited the provided codebase (particularly ble_arduino.ino, updating codes in cmd_types.py as necessary) and send Python commands by adding more cells to the provided demo.ipynb Jupyter notebook. For each command sent from my laptop to the board, I had the result sent back to my laptop by updating the tx_characteristic_string in Arduino and reading the tx_esting_value field in Python. I also had the same result printed to the serial output console for debugging purposes. 

The first task was to implement and test the [ECHO] command. My code updated the tx_characteristic_string object with the necessary code to return, which was simply the input code with a brief string prefix. The string was then sent back to the Python code in the Jupyter notebook.

![ECHO](/docs/lab1/echo.png)

![1b task 1](/docs/lab1/1btask1.png)

The second task was to implement and test the [SEND_THREE_FLOATS] command. Three floats are sent as input from my laptop, which are received by the board to be processed as float values, simply returning those values as output. 

![sendthreefloats](/docs/lab1/sendthreefloats.png)

![1b task 2](/docs/lab1/1btask2.png)

The third task was to implement and test [GET_TIME_MILLIS], which takes no input parameters and returns the amount of time in milliseconds for which the board has been operating. In Arduino, this can be found using [millis()], so the robot command queries that function and returns the result. 

![gettimemillis](/docs/lab1/gettimemillis.png)

![1b task 3](/docs/lab1/1btask3.png)

The fourth task was to set up a notification handler in Python to receive and process queries to [GET_TIME_MILLIS] by extracting the time from the return string. My Python function used the [start_notify] function to start running a thread to execute a custom notification handler. The handler took in the uuid string object and the bytearray of the characteristic value, converted that bytearray to a string representation, and printed it to the notebook. I wanted the handler to only run for one command, but [start_notify] executes on any command which changes the UUID string object, so my handler only tries to extract time when the updated string starts with the "Time elapsed: " prefix exclusively generated from [GET_TIME_MILLIS]. 

![1b task 4](/docs/lab1/1btask4.png)

For the fifth task, I set up a loop to repeatedly query for the current time in milliseconds and send the data to my laptop. I added a new command to execute this, which I simply called [FIFTH_TASK]. I wrote a for loop in Arduino to run the code for GET_TIME_MILLIS 100 times, and the time data was collected in Python with the notification handler, editing to collect the output in a list. Averaging the time differences between consecutive queries yielded an average of around 15 messages per second. 

![fifthtask](/docs/lab1/fifthtask.png)

![1b task 5](/docs/lab1/1btask5.png)

For the sixth task, I globally defined an array of size 20 called [stamps] to store time stamps. I made a copy of the [FIFTH_TASK] loop called [SIXTH_TASK] that stores the data in the array instead of sending it directly to my laptop, and double-checked that the loop count matched the array size. I added the command [SEND_TIME_DATA] which sends each data point individually to my laptop, and I verified that everything was sent by printing all the data on the serial monitor and in Python upon receiving the data and made sure both lists were matching sets of 20 elements. 

![sixthtask](/docs/lab1/sixthtask.png)

![sendtimedata](/docs/lab1/sendtimedata.png)

![1b task 6](/docs/lab1/1btask6.png)

For the seventh task, I added another global array called [temps] to store temperature data, and made another copy of the same loop called [SEVENTH_TASK], which performs the same actions as [SIXTH_TASK], but every update to [stamps] also checks the measured temperature and updates [temps]. I added another command called [GET_TEMP_READINGS] that sends the [stamps] data followed by the [temps] data, prepended each data point with either "Stamp: " or "Temp: ", and edited the handler to maintain two separate lists and add each data point to the right list in the right order. 

![seventhtask](/docs/lab1/seventhtask.png)

![gettempreadings](/docs/lab1/gettempreadings.png)

![1b task 7](/docs/lab1/1btask7.png)

Regarding the eighth task of comparing the results of tasks five and seven, filling the array and then sending everything at once was significantly more time efficient, sending all of the data in about 33 milliseconds. If each time and temperature is stored as a 4-byte float, the board can store up to 96,000 values to fill 384 kilobytes. The method from task 5 may be more time-efficient for very small numbers of measurements due to the time taken to fill the array, but this could be considered unrealistic for real-time measurements that requires high amounts of measurements for accuracy and consistency. The first method, then, seems most valuable for quick troubleshooting, while the second seems more valuable for regular data collection.
