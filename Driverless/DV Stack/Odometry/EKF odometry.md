Uses an Extended Kalman Filter based on the [[Bicycle model]], integrating IMU data.


Designing the models for our EKF was not a trivial task. Our final design uses:  
State vector:
$$X = [x,y,ψ,v]$$
where x,y are global coordinates, ψ is yaw, and v is forward speed at the vehicle center.
Control vector:
$$U = [v_c, δ]$$
where ​ $v_c$ is the speed reported by the engine/encoders and δ is the wheel steer angle (after column→wheel conversion).

We use the discrete-time bicycle kinematic, call it $X_{k+1} = f(X_k, U_k)$.
It integrates the kinematic equations for position and yaw and propagates speed. The control ​is treated as a commanded/measured forward speed subject to control noise. We model process uncertainty coming from unmodelled slip, actuator errors and control noise.
  
Measurements $z = [a_x, a_y, ψ']$ come from the IMU, ​ containing longitudinal/lateral accelerations ax​,ay​, yaw rate ψ˙​.
The map $h(⋅)$ predicts those sensor outputs from the current state and control. IMU accelerations are predicted from kinematics: centripetal and longitudinal acceleration terms. Yaw rate is predicted from the kinematic yaw derivative.

Evaluating the EKF requires computation of the following Jacobian matrices:
- F: Jacobian of the state transition with respect to the state. $F=∂f/∂x$. It linearises how perturbations in the state map into the next state.
- G: Jacobian of the state transition with respect to control (or control noise). $G=∂f/∂u$. It maps control uncertainty into state uncertainty.
- H: Jacobian of the measurement map with respect to the state. $H=∂h/∂x$. It linearises how state errors affect predicted measurements.
- M: Jacobian of the measurement map with respect to control (or control-dependent measurement terms). $M=∂h/∂u$. It maps control uncertainty into measurement space when measurements depend on control.

Covariances for control noise U, measurement noise R, and residual process noise $Q_x$ are currently estimated from sensor data sheets when available or fitted on a insufficiently small number of runs. These estimates work but are approximate. Improving them is a priority next year and could lead to major improvements in the accuracy of the model.