# Week 1 (1/24 - 1/30)
## Completed Tasks
We got the motors to work based off simple code for arduino and built a rudimentary chasis for our robot. This allowed us to complete the first milestone of a robot capable of driving from the middle of the board to off the board. Then, we decided to build a more stable chasis. At the end of the week, this was the state of our robot:

<p align="middle">
    <img src="https://user-images.githubusercontent.com/77326346/151726755-fb20a1ed-4742-41bf-a89d-a0290aa8378b.JPG" height="300" width="300" />
    <img src="https://user-images.githubusercontent.com/77326346/151726762-eb82c996-5adf-440f-9400-55b2ccd42863.JPG" height="300" width="300" />
</p>

## Problems and Solutions
###### 1
The first thing we decided to work on was simpling seeing if we could get the motors to work. 

We started out by working with an adafruit arduino shield due to the lack of a certain connector cable. There was difficulty in connecting to the Windows computer and we had to get assistance from Professor deLeeuw's laptop in order to get the motors spinning but we finally did by the end of the class. In the next class we were provided with a laptop capable of compiling our code. 

Next class, we will bring an adaptor and further look into solutions to compiling from our own devices. 
###### 2
When creating holders for the motors, we initially made the mistake of making them out of styrofoam. They were replaced with regular black-board when we realized hot glue melts styrofoam. 

###### 3
In our design, we had the motors attached to the robot in opposite directions so the wheels would spin in the opposite direction to eachother (since we placed the wheels that the wires are at the same direction). To combat this, we programmed each of them to run at frequencies mirrored around the baseline of 1500 µs (i.e. one of them was at 1300 µs and the other was at 1700 µs).
```
void loop() {
  LEFT.writeMicroseconds(1700);
  RIGHT.writeMicroseconds(1300);
  delay(20);
}
```


## Completed Milestone(s)
Place the robot in the center of the arena, facing any direction. The robot must move to contact the white line.


https://user-images.githubusercontent.com/72710035/151727746-b6051c19-8f2c-4f3e-b45d-9a4d68fb2988.MOV



## Acknowledgments
Since this is only the first week of the class, we worked on many aspects collectively. 

Chaz - came up with the schematics for the rudimentary chassis design and helped put it together

Chuqi - helped put the chassis together and code the robot to drive straight 

Sudais - coded the robot and helped put the chassis together
