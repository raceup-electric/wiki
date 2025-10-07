It treats the vehicle as a two-wheel surrogate with a single front steering wheel and a rear driving wheel. Let x,y be position in a world frame and ψ the yaw. Let v be forward speed at the vehicle center and δ the steer angle at the wheels. The continuous kinematic equations are
$$ \displaylines{
	x' = v \cos ψ \\
	y' ​= v \sin ψ  \\ 
	ψ' = L v \tan δ
} $$
where L is the wheelbase. 

Inputs are wheel encoder speed and steering column angle. Convert encoder readings to linear speed by $v=rω$ where r is effective wheel radius and ω is wheel angular rate obtained by converting engine ticks and considering the reductors at the wheels. Convert steering column angle α to wheel steer angle δ with a calibrated cubic polynomial with coefficients ​ found from calibration against measured wheel angles.

The model outputs integrated $x,y,ψ$ given an initial pose. It assumes no lateral slip and flat ground. Errors accumulate over time due to slip, encoder bias, steering nonlinearity and integration error.