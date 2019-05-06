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

#### 2. BodyRateControl
To control the Roll of the quad is necessary to consider the non-linear transformation from local accelerations to body rates. In this way, this method provides acceleration and thrust commands applying a P controller and the moments of inertia.
Then the pqr_error values are obtained by desired body rates and estimated body rates. In way, it possible calculate the desired moment for each of the 3-axis and return that.

#### 3. RollPitchControl
It fundamental that the quad has the control itself in the down position and down velocity. This method has this propose providing the desired acceleration (in global coordinates and the estimated attitude). For that, it needs to calculate desired quad thrust based on altitude setpoint. It's applying the P controller on R13 and R23 elements of rotation.

#### 4. LateralPositionControl
In this method calculated the horizontal acceleration with based desired lateral position/velocity/acceleration and current pose, using a PID controller to X and Y acceleration.

#### 5. YawControl
This method is more easy to implement because use the P Controller to the yaw control providing the current and commanded yaws. To control the yaw a  linear/proportional controller is used in this method.

#### 6. AltitudeControl
The thrust and moments should be converted to the appropriate 4 different desired thrust forces for the moments. The PD controller provides desired and current vertical positions in NED coordinates. After calculated the collective thrust command return it.


### Flight Evaluation
#### 1. Your C++ controller is successfully able to fly the provided test trajectory and visually passes inspection of the scenarios leading up to the test trajectory.
Ensure that in each scenario the drone looks stable and performs the required task. Specifically check that the student's controller is able to handle the non-linearities of scenario 4 (all three drones in the scenario should be able to perform the required task with the same control gains used).


### Execute the flight
#### 1. Does it work?
It works!