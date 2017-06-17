### PID algorithm
PID algorithm is used to calculate the steering angle of the car, such that the car can running on desired line.

The equation of the algorithm is that: `cte * P_parameter + delta_cte * D_parameter + sum_of_cte * I_parameter`

#### P component
The P term in the algorithm is used to ensure that the car's overall direction can be same as the line.

If the line is on the left the line, then the car should turn left; else if the car is on the right on the line, then the car should turn right; else if the car is on the line, then the car will have steering angle zero. If the P value is large, then the car will have a large steering angle.

However, the car can never run on the line exactly, because the direction of the can will never be the same as the line, while it is reaching the line. Let say the line is on the left of the car. The car will turn left to reach the line. When the car is reaching the line, the car will have zero steering angle, and keep moving forward. The car is overshot. Now the line is on the right of the car. The car will turn right and reaching the line again. After it is reaching the line, the car will be overshot again. As a result, the car can never running on the line.

The D component is calculated by `cte * P_parameter`

#### D component
The D term in the algorithm is used to reduce the steering angle of the car while the car is approaching the line.

The D term is calculated from the difference between the distances of the car and the line in two time frame. If the difference is large, then it means the car is far away from the line and it is safe to use a larger steering angle; else of the difference is small, then it means the car are approaching the line and we should reduce the steering angle to minimize the overshot.

The D component is calculated by `delta_cte * D_parameter`

#### I component
The I term in the algorithm is used to reduce the error between the actual steering angle and our expect steering angle.

The car may not be perfect, the actual steering angle of the car may not be the same as we set. For example, the wheel of the car is not installed correctly, when we set the steeling angle to zero, the actual steeling angle can be some non-zero value. As a result, the car will never go alone the line as we want.

In order to solve this issue, we keep track of the distance between the car and the line. We add extra value to the steering angle.

The I component is calculated by `sum_of_cte * I_parameter`

### Parameter tuning
I started with the parameter shown in the Q&A, `{P=-0.5, I=0, D=-0.5}`.

I noticed that the car is running with oscillation. I believed that the D parameter is too small, so I increase the D parameter to see if the oscillation can be reduced. Finally, I find that **-20** as D parameter, which results in a acceptable oscillation.

Then I start a trial with `{P=0, I=0, D=0}`, according to the algorithm, the steering angle should be zero, always. However, I find that the steering angle is non-zero, and the car is turning right.

![steering angle if {P=0, I=0, D=0}](/img/Screen Shot 2017-06-17 at 12.25.40 PM.png)

I believe that there is an error in the wheels of the car, such that the steering angle is different than I expected.

In order to reduce this error, I introduce a small value, **-0.0001**, to I parameter. Such that the car will have a extra steering angle to left, when the car noticed that it is always on the right side of the line.

Finally, I use the following parameter in the project: `{P=-0.5, I=-0.0001, D=-20}`.

### Note
I started the project after I watched the Q&A video. As a result, the code in my project is similar to what we had in that video.

I want to have a video recording in each trial, however, the car behaves badly if video recording is on. I think it is because my computer has less CPU power to calculate the steering angle when video recording is on. As a result, there is larger delay between the simulator and the pid program. The larger delay makes the car out of the program control.
