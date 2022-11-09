# Week 3 (2/7 - 2/13)
## Completed Tasks

As of this week, our robot now has two color sensors connected that can accurately identify the orange and white lines and stop on the boundary of the field. For the time being, we've impelented a strategy of both sensors sensing the boundaries to control the angle at which the robot avoids crossing the line depending upon what sensors are simultaneously or nonsimultaneously triggered. This is to hopefully limit ballistic behavior of our robot and not run into a situation where the robot becomes lost due to crossing the center line while escaping from an edge.

This week, we've begun to talk about the bigger picture of our robot. The current concept is this: create a robot that has two primary states, the first collection state where it drives to the opposing side and picks up as many dice as possible. Then, in the second state, the robot focuses its attention on pushing as many dice as possible from the opponents side out of bound. Finally, once we discover what is the maximum time it needs to return back home, our robot rushes back to our side at full speed, carrying as much cargo as it manages to pick up. 

Our original idea was simply the "sabotage" method of pushing out all of the rival robot's dice, putting the stress on the opposing robot to try and collect as much dice as possible from our side. However, in anticipation of possible rival designs that focus on collection rather than retrieval, we decided it best to try and hoard some dice of our own at the start as a "backup" from our sabotaging method. The division of our allocated 90 seconds to each state is yet to be determined.

Addionally, we started working with the Pixy2 camera this week, which will be crucial in detection of both the dice and we believe the center line (especially now that it is dual colored). Thus far, we were sucessful on training the camera to recognize blue, green, and red colors. 

## Problems and Solutions
###### 
When testing the signal for crossing the center line, we found that the LED would occationally light up twice when crossing the line(indicating crossing the line twice) . After many trails, we realized that it might be caused by the shadows of the wires blocking the signal in the process. We eventually reorganized the wires and solved the problem.


## Milestones

2.) Place the robot in the center of the arena, facing any direction. The robot must detect the white line and avoid going out of bounds three times in a single run.
(These are all clips from a single attempt. The original video had to be split up to be included in this README. The full video is here: https://drive.google.com/file/d/1pj1mnYHfllKEz6Clt9xYgoD2WVtwiLNs/view?usp=sharing)


https://user-images.githubusercontent.com/72999867/153526097-d7070f06-dac1-4a59-bdcc-004735d88e5c.mp4

https://user-images.githubusercontent.com/72999867/153526082-1c2c89dc-ad26-4416-867d-7c4954b3883b.mp4

https://user-images.githubusercontent.com/72999867/153525852-3e6bf137-ba08-4ba2-a0c4-17c2858ade92.mp4


3.) Place the robot in the starting location. The robot must visibly or audibly signal (e.g., turn an LED on/off) when it has crossed the center line.

https://user-images.githubusercontent.com/72999867/153525518-0251226a-d370-4ad0-9279-b48b6fe1d3bb.MOV


## Acknowledgements
Chaz and Chuqi primarily worked on calibrating and coding for the Pixy2 camera. Sudais worked primarily on writing code for the robot's motors and timer.
