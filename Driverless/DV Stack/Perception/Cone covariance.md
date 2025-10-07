Cone measurement covariance is derived by propagating sensor noise from spherical coordinates into Cartesian space. Covariance data is crucial for [[SLAM]].

For each detected cone at Cartesian coordinates (x,y,z), we first convert to spherical. Sensor noise is assumed independent in this spherical frame, with variances $\sigma_r^2$ (range), $\sigma_h^2$​ (horizontal angle), and ​$\sigma_v^2$ (vertical angle). This defines a diagonal covariance matrix in spherical coordinates.

To express the uncertainty in Cartesian space, we compute the Jacobian of the spherical-to-Cartesian transformation. The Jacobian captures how small perturbations in $r,h,v$ affect $x,y,z$. Multiplying the spherical covariance by the Jacobian and its transpose yields the Cartesian covariance of a single point:
$$ \Sigma_{xyz} = J \Sigma_{spherical} J^T $$

The covariance of the centroid is then the covariance of the mean of all the points:

$$\mathbf{\Sigma}_{\bar{X}} = \frac{1}{n^2} \sum_{i=1}^{n} \mathbf{\Sigma}_{X_i}$$

The result is a full 3×3 covariance matrix for each cone position. This matrix reflects anisotropic uncertainty: low radial noise but higher angular uncertainty at long range, leading to elongated error ellipsoids along tangential directions.