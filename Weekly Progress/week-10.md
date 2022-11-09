# Week 10 (4/11 - 4/17)
## Completed Tasks
Now that our chasis is together, we needed to rewire the buttons for the calibration mode, as well as think about how the roller would affect the weight of the chasis.
Instead of 3D printing, we elected to order a rolling brush online with our budget and modify it as we see fit. We may end up printing the modeled roller brush anyways, but I fear that the lack of support and tall size of the print will cause it to collapse on the build plate. Chaz designed a possible mount for the motor for this brush, but a mechanism for holding it to the motor will have to be created once we are in posession of it.

The camera mount STL file was found online as a case for a PixyCam. Luckily, the PixyCam is able to fit into this with minimal modifications. The case was designed that it could be fited to a go-pro mount, which we printed as well and plan to glue to the front of the robot. At first, we were concerned that the cover of the case covers the light from the Pixycam2, but turns out the light reflects and pases through the PLA and makes the color the camera is viewing even more visible than it was before!

Besides creatig the camera mount, we implemented the calibration mode. The calibration mode allowed us to collect the color info and recalibrate the color sensor's ability to recognize color. We also implemented the mechanism to let the robot recognize colored lines reliably.


## Problems and Solutions
#### 1

Once we went to rewire the robot, we had issues figuring out the calibration mode and the corresponding connections to the lights. In the end, after viewing past photos we had (thankfully) taken of our old set up, we were able to wire the button and the corresponding LED lights in the proper fashion. 

#### 2
We found out that the readings from the right and left sensors weren't consistent with each other so we decided to create seperate color-sensing standard for each of the sensor. We had two thoughts on how to implment the color sensoring:

![the strategies](https://user-images.githubusercontent.com/72710035/163728948-ab97e71e-8335-4229-b1a5-baf0cc333b15.png)



We ended up choosing to implement the second strategy at first since it was easier to code and seemed more efficient. After testing for different error range, we settled on 25% of error range and it gave us pretty reliable results.

This also means we do not have to raise the height of the color sensors, which we were concerned we would have had to do (in the situation where the light from the sensor made everything below it appear white).


## Milestones

Somehow from the move to the new chassis, we've seemingly fallen back from some of the milestones. Crucially, the LED lighting up when crossing the line will have to be fixed. 

## Acknowledgements
Sudais and Chuqi worked on the calibration mode and testings, Chaz worked on the camera mount construction.


