# Week 11 (4/18 - 4/24)
## Completed Tasks
We created the mounts for the brushes.We tried lifting up the chassis by switching to another version of the wheel mounts that we have - beacuse the wheel will get stuck at the edge of the arena sometime. However, it created other problems such as the robot would not go straight anymore. After a lot of trouble shooting we decided to switch back to the original wheel mounts and approached the stucked wheel problem by changing our code.




## Problems and Solutions
#### 1

The wheels sometimes get stuck at the edge of the arena after it turns. We tried lifting up the chasis, but then the robot was not able to go straight after the change. After switching out the wheels, motors and ballbearing one by one, we came to the conclusion that there were a mix of causes with the ball bearing being the main cause. Since we only have one ball bearing at the back of the chasis, the back of the robot tended to drag on the ground and little uneven weight distribution will result in large changes in the robot's behaviors. We thought comparing to that, the stucked wheel was relatively easier to solve by changing the code. So we ended up switching back to the original mounts.

We coded the robot so that it will back up before turning. This way, the wheels are less likely to fall off the arena.
```
//an example of the backup and rotate. When the right sensor sees white and the left sensor sees black, the robot will back up for 0.1 seconds then turn left for 0.6 seconds

(r.rw && r.lb) r.backUpAndRotate(100, LEFT, 600);
```

#### 2

Sometimes when corssing the middle line, the two color sensors would not detect the line at the same time because the robot was approching at an angle. So we created probablyswitching. The thinking behind this is that if the left sensor crossed the middle line first, it will remember that it detected the middle line by switching the probablySwiching state to LEFT. Then within a time range, if the right sensor also detects the middle line, the LED light signaling line corssing will be turned on.
```
   else if (r.ry && r.lb)  {
      if (probablySwitching == LEFT) {
        toggleHomePin();
      } else probablySwitching = RIGHT;
    }
```
The premise for this to work is that there are only 3 kinds of interactions with the middle line- two sensors detecting it at the same time, the left sensor detecting it first, the right sensor detecting it first. So we are still working to ensure that other situations -like the robot doing crazy turns when crossing the line would not happen: one possible senario is tracking down the dice on our side and suddenly turn when about to cross to the opponent's side.




## Milestones
Milestone 4 completed. The blue light was signaling the middle line crossing.
https://drive.google.com/file/d/1nSAo_nqosvLGrPVNPxciAutdfTULQucy/view?usp=sharing


## Acknowledgements
Chaz, Chuqi, and Sudais worked on constructions and trouble shooting together.Chaz mainly focused on creating the brush mounts while Chuqi and Sudais focused on creating and testing different versions of the code.

