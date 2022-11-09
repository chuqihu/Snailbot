# Week 2 (1/31 - 2/6)
## Completed Tasks

We connected one color sensor to the robot and adjusted it to detect the white lines based off "c" values. A function of "back up and turn" was created to make sure the robot doesn't go out of the arena.

## Problems and Solutions
###### 
This week, we continued to have some consistent hardware problems: our only Windows laptop was completely unable to compile the code for some unknown reason. Later, the code did not upload correctly do to issues with an adaptor. Switching the adaptor solved this problem but it still presents an issue in that we have to rely on this adaptor.

Initially, the he light on the color sensor did not light up due to suspected broken bulbs. This was solved by changing to two different color sensor, but that presented new problems in terms of configuration due to the pins being in different positions. 

Most of these hardware issues could be solved by finishing the chassis of the robot to be better balanced and creating a structure for the color sensors to be drilled into. As for the uploading issues, the Windows laptop would only compile and upload after uninstalling and installing the 1.8.19 version of the Arduino IDE. We're hoping to sort out the compilation and uploading errors we keep running into once and for all this week.

In the end, while we have confirmation that the color sensors are picking up, but currently only one is connected and there seems to be a long delay between when it captures the color and executes the "back up and turn" function we wrote. Next on the agenda is to connect the other color sensor, get the robot to escape from the edge properly, and then add LED components to the robot to indicate when it has crossed the red centerline.   

## Completed Milestone(s)
No new milestone was completed. Up till now, we've completed the first milestone.

## Acknowledgments
Chaz worked mainly on creating a more stable chassis design that can accomodate two color sensors.
Sudais and Chuqi worked primarily on writing code for the robot's motors, color sensors, and a "back-up and turn" function.

