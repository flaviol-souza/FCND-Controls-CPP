## Project: Control of a 3D Quadrotor
---

# Required Steps for a Passing Submission:
1. Implemented body rate control in C++:
2. Implement roll pitch control in C++.
3. Implement altitude controller in C++.
4. Implement lateral position control in C++.
5. Implement yaw control in C++.
6. Implement calculating the motor commands given commanded thrust and moments in C++.
7. Write it up.
8. Congratulations!  Your Done!

## [Rubric](https://review.udacity.com/#!/rubrics/1643/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
## Implemented Controller
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. 

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. GenerateMotorCommands
The goals this method is provided and keep the stabilize of the quad when there is rotation. So it, the controller should be a proportional controller to the inertia of the quad. The GenerateMotorCommands method provides the thrust based on the total thrust and the moment.
For this, it's necessary to calculate the rotor-to-rotor length to get the moment by axis.
Lastly, the total momentum must then be balanced between the axis (X, Y and Z). The equilibrium the forces are applicated to each rotor can have negative or positive values. So that, the Clockwise rotation generates a counterclockwise direction with a positive value. If it a negative value the Counter-clockwise rotation produces clockwise moment.
In this intro step, just the mass of the quad adjust:
```
PASS: ABS(Quad.PosFollowErr) was less than 0.500000 for at least 0.800000 seconds
```

#### 2. BodyRateControl
To control the Roll of the quad is necessary to consider the non-linear transformation from local accelerations to body rates. In this way, this method provides acceleration and thrust commands applying a P controller and the moments of inertia. In this way, the kpPQR parameter should be adjusted to be the quad isn't from flipping.
Then the pqr_error values are obtained by desired body rates and estimated body rates. In a way, it possible calculate the desired moment for each of the 3-axis and return that.

#### 3. RollPitchControl
It fundamental that the quad has the control itself in the down position and down velocity. This method has this propose the desired acceleration (in global coordinates and the estimated attitude). For that, it needs to calculate desired quad thrust based on altitude setpoint. It's applying the P controller on R13 and R23 elements of rotation. To that is a need to find an equilibrium the kpBank and kpPQR until that the quad gains stable.
The implementation can be checked in lines [135 to 151](https://github.com/flaviol-souza/FCND-Controls-CPP/blob/master/src/QuadControl.cpp)

#### 4. LateralPositionControl
In this method calculated the horizontal acceleration with based desired lateral position/velocity/acceleration and current pose, using a PID controller to X and Y acceleration by changing the body orientation (non-zero thrust) to hope direction. This function is implemented on the `LateralPositionControl` method in lines [227 to 236](https://github.com/flaviol-souza/FCND-Controls-CPP/blob/master/src/QuadControl.cpp) on `QuadControl.cpp`.

#### 5. YawControl
This method is more easy to implement because use the P Controller to the yaw control providing the current and commanded yaws. To control the yaw a  linear/proportional controller is used on YawControl in lines [257 to 270](https://github.com/flaviol-souza/FCND-Controls-CPP/blob/master/src/QuadControl.cpp) of the `QuadControl.cpp` file.

#### 6. AltitudeControl
The thrust and moments should be converted to the appropriate 4 different desired thrust forces for the moments that also includes the non-linear effects on Roll and Pitch. The PD controller provides a resource to desired and current vertical positions in NED coordinates. After calculated the collective thrust command return it.
To scenario 4 is needs includes an integrator (i) to compensate for the shift of weight presented in one of the quads. This implementation can be see on lines [181 to 191](https://github.com/flaviol-souza/FCND-Controls-CPP/blob/master/src/QuadControl.cpp) of the `QuadControl.cpp` file

### Flight Evaluation
#### 1. Your C++ controller is successfully able to fly the provided test trajectory and visually passes inspection of the scenarios leading up to the test trajectory.
Ensure that in each scenario the drone looks stable and performs the required task. Specifically check that the student's controller is able to handle the non-linearities of scenario 4 (all three drones in the scenario should be able to perform the required task with the same control gains used).


### Execute the flight
#### 1. Does it work?
It works!