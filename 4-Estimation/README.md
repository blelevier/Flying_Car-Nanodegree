# Building an Estimator

## Overview

Previously, we assume that the simulator was working with a perfect set of sensors. Here, we will develop the estimation portion of the controller. This project was based in this  [repo](https://github.com/udacity/FCND-Estimation-CPP) provided by Udacity. First, please read the previous repository as it contains important information about the environment setup, a simulator walkthrough, the tasks and evaluation.
The calculations were based on the paper [Estimation for Quadrotors](./resources/Estimation_for_Quadrotors.pdf).
The simulator covers six scenarios where we can test our functions.

### Scenario 6: Sensor Noise

Analyze the date and update the parameters ```MeasuredStdDev_GPSPosXY``` and ```MeasuredStdDev_AccelXY``` in ```./FCND-Estimation-CPP/config/6_Sensornoise.txt```.

![scenario6][scenario6]

```
Simulation #6 (../config/06_SensorNoise.txt)
PASS: ABS(Quad.GPS.X-Quad.Pos.X) was less than MeasuredStdDev_GPSPosXY for 71% of the time
PASS: ABS(Quad.IMU.AX-0.000000) was less than MeasuredStdDev_AccelXY for 69% of the time
```

### Scenario 7: Attitude Estimation 

Improve the complementary attitude filter with a better rate gyro attitude intehration scheme in the function ```UpdateFromIMU()```. The Euler angles were transformed to quaternions to get the representation in the inertial frame. After that, the body rate from the gyro was integrated to get the updated roll, pitch and yaw corresponding values.

![scenario7][scenario7]

```
Simulation #7 (../config/07_AttitudeEstimation.txt)
PASS: ABS(Quad.Est.E.MaxEuler) was less than 0.100000 for at least 3.000000 seconds
```

### Scenario 8: Prediction Step

This is the prediction step of the EKF filter, covered in ```PredictState()```. The transitional function calculates the first six states (pose and velocities from equation 36).
![equation36][equation36]
</br>
![scenario8][scenario8]

```
Simulation #8 (../config/08_PredictState.txt)
```

### Scenario 9: Prediction Covariance

We introduce a realistic IMU with noise in a fleet of 10 quadcopters. Equation 52 represents the partial derivative of the body-to-global rotation matrix in ```GetRbgPrime()```. We also tune the parameters ```QPosXYStd``` and ```QVelXYStd```.</br>

![equation52][equation52]
</br>
![scenario9][scenario9]

```
Simulation #9 (../config/09_PredictCovariance.txt)
```

### Scenario 10: Magnetometer Update

We'll add the information from the magnetometer  to improve the filer's performance by implementing the equations 56, 57 and 58. We improve the function ```UpdateFromMag()``` and tune the parameter ```QYawStd```.

![equation56][equation56]
</br>
![scenario10][scenario10]

```
Simulation #10 (../config/10_MagUpdate.txt)
PASS: ABS(Quad.Est.E.Yaw) was less than 0.120000 for at least 10.000000 seconds
PASS: ABS(Quad.Est.E.Yaw-0.000000) was less than Quad.Est.S.Yaw for 66% of the time
```

### Scenario 11: Closed Loop + GPS Update, Adding your Controller

Now we include the ```UpdateFromGPS()``` to our EKF estimator by using section 7.3.1 (equations 53, 54 and 55).

![scenario11a][scenario11a]

</br>Then, we'll be flying from an estimated state instead from flying with ideal pose. We introduce our controller from project 3. Parameters needed to be tune (aprox 30% reduction in position and velocity gains). We were able to satisfy the criteria of flying with an estimated position error of less than 1 meter, however, there is still a lot of room for improvement.

![scenario11b][scenario11b]

```
Simulation #6 (../config/11_GPSUpdate.txt)
PASS: ABS(Quad.Est.E.Pos) was less than 1.000000 for at least 20.000000 seconds
```

[//]: # (References)
[equation36]: ./img/equation36.PNG
[equation52]: ./img/equation52.PNG
[equation56]: ./img/equation56.PNG
[scenario6]: ./img/scenario6.gif
[scenario7]: ./img/scenario7.gif
[scenario8]: ./img/scenario8.gif
[scenario9]: ./img/scenario9.gif
[scenario10]: ./img/scenario10.gif
[scenario11a]: ./img/scenario11a.gif
[scenario11b]: ./img/scenario11b.gif
