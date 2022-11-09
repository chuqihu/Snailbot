# Week 4 (2/14 - 2/20)
## Completed Tasks

This week was dedicated mainly to "beutifying" code and implementing different reactions depending upon how it approaches the white, yellow, and orange lines in order to avoid ballistic behavior. When only one sensor detects the white line, the robot only turns away from the line (as in, only the wheel on that side reacts). When both sensors sense white, the robot performs a 180 degree turn and continues on its path.

By calibrating our code to the color values picked up by the sensors, our robot can also correctly identify yellow and orange tape, and can differentiate when one side is on the yellow and the other the orange (same with combinations of yellow/orange/white tape).

We are sticking with the method we've dubbed "sabotage," but would like to try and implement both collection and pushing off the board simultaneously, without the need for the robot to switch between a "collecting" and "pushing" state. by implementing a collection device, brainstormed as some kind of cylyndrical brush or flapping contraption that is constantly rotating and brushing the dice the robot runs into into a collection chamber within the robot. If this brush is constantly rotating (such as on a vacuum), the robot's behavior could be skewed towards approaching dice and pushing them off the field. The spinning mechanism would allow our robot to collect dice as it's simultaneously getting them off the field: any dice that isn't collected in our robot will be out of the game. We are essentially separting our robot into two "behaviors" in one without the need to fracture our time between two totally seperate behaviors.

This leads us to looking at the structure of our chasis going forward. After some brainstorming with Professor deLeeuw, we came up with a robot design that seperates the arduino, battery, and breadboards from the collection chamber and the spinning mechanism's motor. Here is a quick mock-up in TinkerCAD:

![tinkercadimage4](https://user-images.githubusercontent.com/72999867/154890388-3cbd4ca5-a041-44c8-a248-2abadcc61980.png)
![tinkercadimage3](https://user-images.githubusercontent.com/72999867/154890390-e9c87e4d-01c9-41dd-a3be-ae6244391d26.png)
![tinkercadimage2](https://user-images.githubusercontent.com/72999867/154890391-c4398676-7ec3-4a4b-a82e-1eb1f4dce8ef.png)
![tinkercadimage1](https://user-images.githubusercontent.com/72999867/154890394-491be846-224d-44bb-a962-3e143c953a33.png)

The idea is to stack the boards in "shelves" in the back red box and have the battery rest above a ball caster at the back of the robot. Connections to the motors can be run from wires along the inside walls of the robot. Ideally, we would be able to create the robot (motors + arduino) as one piece and then be able to connect and disconnect a "collection chamber" piece (with the brush built in).


## Problems and Solutions
###### 


## Milestones

The next milestone is to have our robot stop after 90 seconds and be able to signal which half of the board its on. We've accomplished the 90 second round part but have not coded the robot such that the light stays on on the opponenets side at all times (though we expect this to be an easy add).


## Acknowledgements
Sudais is working on a priority queue for the behavior of the robot and writing a lot of the code (with help from Chuqi). Chuqi worked on the PixyCam code and setup as we begin to work that into our design. Chaz worked with Prof. de Leeuw to come up with the new chassis design and is starting the process of building the parts to 3D print. 
