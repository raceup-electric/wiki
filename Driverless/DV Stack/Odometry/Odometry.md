The odometry node reads data from the sensors of the car:
- wheel speed from the four encoders
- steering angle
- IMU
and computes the current global speed, position and orientation of the car.

This estimate is "open-loop" and subject to drift over time.
The odometry node also receives from [[SLAM]] corrections on its position to correct this drift.
This correction is done in the odometry node as opposed to directly in the SLAM node since odometry runs at an higer frequecy (currnetly 100 vs 20 Hz).

[[Bicycle model]] is a simple kinematic model.
Currently using the [[EKF odometry|EKF]] implementation.