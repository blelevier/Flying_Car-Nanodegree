# Control of a 3D Quadrotor


## Overview

In this project we will be building and implementing a controller in C++. This project was based in this  [repo](https://github.com/udacity/FCND-Controls-CPP) provided by Udacity. First, please read the previous repository as it contains important information about the environment setup, a simulator walkthrough, the tasks and evaluation.

The 3D control architecture was:

![Control Architecture][architecture]

The simulator covers six scenarios where we can test our functions.

### Scenario 1

The ```Mass``` parameter was tuned until hover was achieved.

![scenario1][scenario1]

```
Simulation #1 (../config/1_Intro.txt)
PASS: ABS(Quad.PosFollowErr) was less than 0.500000 for at least 0.800000 seconds
```

### Scenario 2

First we implemented the body rate control and tuned ```kpPQR```. After that we proceeded with the implementation of the roll/pitch control and the tuning of ```kpBank```.
A reminder of how the quadcopter looks like:<br/>
![drone][drone]
<br/>Now based on the input from the controller we can set the angular velocities of the propellers. For this, we would like to solve the next linear equation. The first row represents the vertical acceleration, the second represents the equation for roll, the third row is the pitch equation, and the last one is derived from the yaw equation:<br/>

![equation1][formula2]
<br/>
![scenario2][scenario2]

```
Simulation #2 (../config/2_AttitudeControl.txt)
PASS: ABS(Quad.Roll) was less than 0.025000 for at least 0.750000 seconds
PASS: ABS(Quad.Omega.X) was less than 2.500000 for at least 0.750000 seconds
```


### Scenario 3
For this task we implement the position, altitude and yaw controls and start tuning other parameters as ```kpPosZ```, ```kpVelXY```, ```kpVelZ```, ```kpPQR```, ```kpYaw```.

![equation2][formula3]

We based this approach on the formula (see reference 1).


![scenario3][scenario3]

```
Simulation #3 (../config/3_PositionControl.txt)
PASS: ABS(Quad1.Pos.X) was less than 0.100000 for at least 1.250000 seconds
PASS: ABS(Quad2.Pos.X) was less than 0.100000 for at least 1.250000 seconds
PASS: ABS(Quad2.Yaw) was less than 0.100000 for at least 1.000000 seconds
```

### Scenario 4

Gains were tuned again.

![scenario4][scenario4]

```
Simulation #4 (../config/4_Nonidealities.txt)
PASS: ABS(Quad1.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
PASS: ABS(Quad2.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
PASS: ABS(Quad3.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
```


### Scenario 5

More tuning. Need to work on the trajectory of the second quad.


![scenario5][scenario5]
```
Simulation #714 (../config/X_TestMavlink.txt)
PASS: ABS(Quad2.PosFollowErr) was less than 0.250000 for at least 3.000000 seconds
```

### Scenario 6


![scenario6][scenario6]


#### References

1. [Feed-Forward Parameter Identification for Precise Periodic
Quadrocopter Motions](./resources/Feed-ForwardParameter_paper.pdf)
2. [Double Integrator Control: Cascaded P Controller Gains vs Damping
Ratio](./resources/Double_Integrator_Control_paper.pdf)
3. [Guidelines on PID Tuning](https://docs.px4.io/master/en/config_mc/pid_tuning_guide_multicopter.html)


[//]: # (References)
[architecture]: ./img/architecture.PNG
[drone]: ./img/drone.PNG
[formula2]: ./img/formula2.PNG
[formula3]: ./img/formula3.PNG
[scenario1]: ./img/scenario1.gif
[scenario2]: ./img/scenario2.gif
[scenario3]: ./img/scenario3.gif
[scenario4]: ./img/scenario4.gif
[scenario5]: ./img/scenario5.gif
[scenario6]: ./img/scenario6.gif
