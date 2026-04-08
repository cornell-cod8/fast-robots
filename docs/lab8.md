# Lab 8

The objective of this lab is to put everything together from the last few labs to do cool stunts with our awesome robots!

## The Stunt

In the picture below, I have set up the lab scenario for the flip in Task A. I use the cardboard box as a barrier with foam padding to prevent damage to the robot. A labeled line of masking tape lies 3 meters from the box, and an adhesive pad lies in front of the box so that its center is 1 foot from the box. The car should approach the wall from past the line of tape, perform a flip around the center of the pad, then cross the line of tape again. 

<!-- TODO: add setup picture -->

## Methodology

Based on the details of the stunt, my control program for the stunt was fairly straightforward:

1. Drive motors forwards at max speed to accelerate at the wall.
2. Reverse motor inputs backwards around the center of the mat (~1 foot, or 304 mm) to perform the flip.
3. Back away from the wall until the car is on the other side of the 3-meter line.

I made some small adjustments to the code and to the robot during testing. I manually tuned the distance at which the robot attempts the flip slightly higher (~325 mm) to make sure it was attempted on the pad and to avoid hitting the wall. 

To aid in flipping over the robot, I taped a bag of nails under the front of the robot as shown below. I also had to tie down some of the wiring more tightly on the top so that nothing would drag while the robot runs upside-down. 

<!-- TODO: add weight picture -->

## Results

Below I present three successful attempts.

Attempt 1:

<!-- TODO: attempt 1 video -->
<!-- TODO: attempt 1 motor value graphs -->
<!-- TODO: attempt 1 sensor data graphs -->

Attempt 2:

<!-- TODO: attempt 1 video -->
<!-- TODO: attempt 1 motor value graphs -->
<!-- TODO: attempt 1 sensor data graphs -->

Attempt 3: 

<!-- TODO: attempt 1 video -->
<!-- TODO: attempt 1 motor value graphs -->
<!-- TODO: attempt 1 sensor data graphs -->