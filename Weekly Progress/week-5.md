# Week 5 (2/21 - 2/27)
## Completed Tasks

This week was dedicated mainly to continue "beautifying" the code, modeling the robot, and coding the collection behavior. By the end of the week, we had a relatively reliable collecting behavior where the robot will approach the nearst dice. The next step would be dealing with the blind spots and integrating this behavior with the line detections and scheduler.

We printed out a motor mount to test and started remodeling that in Fusion360 in order to add it to a larger print. The main issue with modeling at the moment is figuring out measurements.

## Problems and Solutions
###### 
When testing the collecting behavior, we found that the robot will always arrive somewhere to the left of the dice even though the code should enable the robot reach the dice. After some trouble shooting, we realized that it was a hardware problem and solved it by setting the motor to different speed for the "go straight" function.

Besides this, we are also working on a behavior where the robot would ignore the dices on "our side" of the arena, and only go for dices on the opponent's side. It is relatively easy to achieve when the camera is pointing straight towards the middle line(by comparing the y coordinate), but in other circumstances is might be tricky. After consulting with Professor De Leeuw, we had the idea of computing the equation for the line and compare the x coordinate of the dice to the line. We are in the process of implementing the function.


## Milestones

N/A

## Acknowledgements
Chaz worked on the 3D print modeling. Chuqi worked on the collecting behavior code. Sudais worked on the scheduler. 
