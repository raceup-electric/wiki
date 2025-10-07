Simultaneous Localization and Mapping takes as input:
- the open loop [[Odometry]] 
- the cones positions relative to the car from [[Perception]]
and computes:
- the global map of all cones
- a "closed loop" position of the car, suffering less from drift.

[[Graph based SLAM]] was implemented from scratch by us.

[[MRTP SLAM]] uses an industry grade library; its more reliable but less customizable.