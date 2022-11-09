# Week 13 (5/1 - 5/9)
## Completed Tasks
For this week, we first fixed the color detection with Professor deLeeuw's method. Then, we did extensive testings and made small code changes based on the testing results. We also finished all of the milestones.

## Problems and Solutions
### 1
During one point before the competition, our robot began messing up consistently. However, once we changed the battery there was no more problem. Due to this, we tested several batteries and chose the two with the most consistent runs to use for the actual competition.

### 2
After changing our base velocity we noticed that sometimes the robot wouldn't back up enough and would need to back up a second time to "escape" the boundaries so we had to do some last minute adjusting of the speed and duration of the backup function.

### 3 
We fixed the color calibration after consulting with Professor deLeeuw. We calculated the r,g,b, ratio of each color and used the Euclidian distance to see what color the sensor is detecting. Changing the type to "float" and fixing a numeric typo ended up solving the problem of not detecting color correctly.

### 4 
Since we've testing our robot at the lab, and our robot detects the lab floor as white, we were worried that the brown floor at villard will mess up the behaviors. So, we brought the robot to villard and got readings for the brown floor. We set it so that the robot will hehave the same when it detects brown as it detects white. However, for some reason, the robot would also see the black mat as brown. So we ended up changing it back. We taped blue tape around the boundary lines and tested the robot in the lab and it turned out behaving alright and not going off of bound. So, we stayed with the previous settings. We also later realized it didn't matter all that much as the robot would treat brown as any color but black so it should never cross it.

### 5
The button that is supposed to start the motors is a little finicky such that sometimes it will start before we press the button (specifically, when we put our fingers close to the button but don’t actually touch it, like it’s reading our intentions). It turned out our hand might be creating a small electrical field.

We made sure the button was connected to the ground correctly and set the pin mode to INPUT_PULLUP. That solved our problem.

## Milestones

We knocked out the last several milestones this week, now that we finally have a consistently working robot! This includes:

-milestone 5-8: Achieve a net expected score* of at least +4 to +80 points in a solo run (milestone 5-8). We ended up achieving a net expected score of 204.5 in the solo run.

link to the video: https://drive.google.com/file/d/1yyNW1XBaTx3e5j_8FlBkEFE9EMrLOQ8c/view?usp=sharing
and dices we collected:
<img width="359" alt="204.5 expected score" src="https://user-images.githubusercontent.com/72710035/167765838-26e93707-edb0-4087-a769-1c26fc6b0397.png">

-milestone 9: Compete in a head-to-head competition. The robot must remain functional (not break down or get stuck) and stay in bounds for the whole run.

-milestone 10: Compete in five consecutive head-to-head competitions, with no break other than to reset the board. The robot must remain functional (not break down or get stuck) and stay in bounds for the whole run.

-milestone 11: Achieve a net expected score* of at least +20 points in a head-to-head competition against the demo robot. 

-milestone 12: Achieve a net expected score* of at least +40 points in a head-to-head competition against the demo robot.

videos for milestone 9-12: 

https://drive.google.com/file/d/1aZJfGxHLkB0AXx0TnObk2yTCjhDvrOvy/view?usp=sharing
https://drive.google.com/file/d/1YsGJTLaGJWS7hXU1f-oEnDfAcFUNl07x/view?usp=sharing
https://drive.google.com/file/d/1iJhU9nKngFmojKsvaqKMLW7KJRtqrDO3/view?usp=sharing
https://drive.google.com/file/d/1o6eA4AXsqMP55-tl8-SDtzWCQzklLoUY/view?usp=sharing

For the very last run in the video, we achieved a net expected score of 101.
![IMG_2166](https://user-images.githubusercontent.com/72710035/167768904-38eee9e4-30dc-4d76-b7d6-127e00834cdf.jpg)

 

## Competition Reflection
From Chaz:

For most of the class, we were behind the other teams in terms of run-testing. However, by the end of the class I believe we had a highly functional design and a collection method that worked much better than we had initially thought it would. 

During the competition, the main issue we ran into was, frustratingly, collisions and interference of the other robot (which accounted for 4 of our 6 losses). Our robot getting stuck in these scenerios was a disadvantage to us particulalry beacuse our robot operates by collecting and holding dice on the oponents side, so if it gets pinned on the opponents side, those dice go to them. There is little we could have done to combat this besides simply making our robot capable of avoiding the other robot, making it larger such that it could not be pushed so easily, or modifiying the brush in some way so that it would stop when near another robot. 

We also ran into an issue that we had never run into before during our final run of the competition, that being our robot failing to cross onto the opponents side at the beginning and subsequently searching for dice only on our side. This was also particulalry frustrating due to this never happening before, and I'm still unsure how that happened. 

Overall, while the competition itself could have gone better, I believe we are all pleased with how our final robot itself turned out. Our collection method, the scheduler, callibration mode, and our reactions to the colored lines were all very efficient and under other circumstances and with less bad luck (and maybe an extra week of head-to-head practice with the other robots), I'm sure we could have won. Regardless, this was overall a great experience working with robotics and we are proud of what we accomplished.

From Chuqi:

I am also very proud of what our robot turned out to be. There were some unexpected collisions and unfortunate glitches during the competitions, but that's not something we can controlled for. For the glitch at the final run, my hypothsis is that the robot got caught by the tape just a bit, so it thought it already crossed and started to act like it's on the opponent'side, but we need to look at the recording to determine the real cause.

I think some possible changes we could make is lifting up the robot a bit so that it will not get stuck at the boundary line. Instead, it will go out of bound when collision happen, which I think will be beneficial for us, since we have all the dices on our robot. It's better that none of the team gets them rather than giving them to the other team. However, lifting the robot up may interfere with the collecting machanism, so without testing we could never know. Also, collecting dice at the opponent's side increased the probablity of our robot to be disfunctioned at their side. We might be able to account for it by doing more head-to-head tests with the other team's robots. 

Overall, I enjoyed working with my groupmates. Also, I might be biased, but I think our robot has the coolest collecting mechanism and the best solo run performance! Very proud of what we achieved together!

Sudais:

Our robot had a simple but effective design and was only doing a few things but was doing them all really well. It detected the center line correctly, had proper behaviors at boundary lines and wasn't getting stuck there, it wasn't going off the board and was collecting dice much better than we ever expected. We also had as few delays as possible by leveraging the scheduler for the detection of the center line to minimize the "blind" periods (where we couldn't detect colors).

We had a single point of failure which was the robot not ending up back in our half at the end of the game and we took a number of measures to mitigate against that (including giving it almost one-third of the time of the game to get back, speeding it up, using Pixy cam to detect the middle line, etc.) and it was doing really well in solo runs with a >95%+ clearance in them. 

In the competition, however, these measures were not enough as the other robots had more complex mechanisms than the only one we were able to test it against (Josh's). Our brush was the same color as one of the dice which could have made it a target for the other robots and it also got caught in the wires of another robot ending the run short (which is bad for us since we only get back to our side at the very end). 

In retrospect, given how well out collection mechanism was working, on the day of the competition a better strategy could have been to just go to the opponents side for a few seconds (~15?) to collect as many dice as possible and then just come back to our side and collecting all of ours so the opponent couldn't get them.

Overall, I am happy with how the robot turned out and learnt a fair bit about robots (might chuckle whenever I see a Roomba now).

## Acknowledgements

Thanks to Sudais for coming up with the code structure to implment complex behaviors, writing the majority of the behavioral code from the ground up and help fixing different code problems.

Thanks to Chuqi for creating the tracking code, modifying the behavior code and doing most of the testing and trials for the robot at every stage of development, and for coming up with solutions to hardware and software issues.

Thanks to Chaz for designing the chassis and modeling/3D printing the hardware for the robot.

