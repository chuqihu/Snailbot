# Week 7 (3/21 - 3/27)
## Completed Tasks
This week, we finalized the design of our robot and have begun 3D printing it. For the code, we've refined the dice detection function by incoprating a PD (proportion + derivative) controller.We also went through the codes and spotted and fixed an infinite loop in the correct path function.

## Problems and Solutions
It was difficult to find the ideal value for the propotion_gain and derivative_gain initially. We ended up first setting the derivative_gain to 0 and tuning the propotion_gain until the robot is heading towards the dice then tuning the derivative_gain to make the movement smoother. 

The robot was also not able to trace the dice at first, which was solved by flippingthe sign before proportion_gain and derivative_gain.
```
   int left_speed = BASE_VELOCITY + -PROPORTION_GAIN * error + -DERIVATIVE_GAIN * error_change;
    int right_speed = BASE_VELOCITY+ +PROPORTION_GAIN * error + DERIVATIVE_GAIN * error_change;
```


## Milestones

N/A

## Acknowledgements
Chaz worked on the 3D print modeling. Chuqi and Sudais worked on troubleshooting the codes and Chuqi also worked the PD controller. 
