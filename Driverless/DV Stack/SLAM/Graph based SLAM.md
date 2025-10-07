Graph-based SLAM implemented with g2o.
The graph contains two optimizable vertex types:
- vehicle poses (position and heading)
- landmarks (cone positions).
Edges encode measurements with covariances:
- odometry edges represent measured relative travel between poses, computed as differences between poses from the [[Odometry]] node.
- vision edges represent relative pose-to-cone observations from the last [[Perception]] frame.

Optimization is a nonlinear least-squares problem. Levenberg–Marquardt in g2o finds the maximum a posteriori explanation of percepts by minimizing covariance-weighted residuals from odometry and vision edges. The optimizer outputs a corrected trajectory and updated cone map.

### Color voting
We maintain a per-landmark vote history and resolve color by plurality over recent observations.

Each detected cone is associated with a persistent landmark by SLAM data association. For every new observation we increment that landmark’s vote counts for the reported color and ignore unknown/invalid classifications. We do not overwrite previous votes. Once a landmark accumulates at least three non-unknown votes we take the plurality color as the landmark’s label. Ties are broken deterministically by selecting the color with the highest cumulative confidence or by a fixed color priority if confidences are equal.

This procedure reduces perceptual flicker because transient misclassifications are outvoted by consistent observations tied to the same map landmark. Storage per landmark is a small fixed-size integer histogram of color votes. Votes may be decayed or capped in future work to adapt to long-term changes or reclassifications.

Measured effect on accuracy. The raw perception classifier yields 82.19% color accuracy on single observations. After applying SLAM-based majority voting on three observations per landmark the effective color accuracy rises to 98.36%.

### Real time performance
The optimizer is the SLAM runtime bottleneck. Full-graph optimization is infeasible within 50 ms.

We limit work by optimizing a sliding, pose-centered neighborhood that grows progressively. Data association runs in a few milliseconds. The remaining budget is spent on repeated local optimizations over neighborhoods built from the newest poses and all landmarks connected to them. Neighborhoods are constructed by taking the most recent _n_ pose vertices and including every vertex that shares an edge with those poses. This yields a subgraph containing both recent vehicle poses and their observed cones.

Optimization is staged. We run a sequence of local optimizations with increasing spatial scope and iteration counts so early cheap refinements converge quickly and more expensive refinements run only if time allows. Example schedule used in practice: neighborhood sizes _n_={1,5,10,20,30} with corresponding iteration budgets {2,3,3,5,50}. Each stage optimizes only the selected subgraph while other vertices remain fixed. After every stage we check a hard time budget and abort further stages if exceeded.

A watchdog enforces the real-time constraint. A stop flag is set if the optimizer exceeds a short timeout (30 ms in our setup). The optimizer polls that flag and terminates gracefully mid-stage if required. Each optimization run increments an id so stale watchdog signals cannot stop newer runs.

This progressive, time-aware strategy concentrates computation on the most relevant recent poses and observations, yields fast incremental corrections compatible with a 20 Hz SLAM loop, and provides graceful degradation: small, fast updates always complete; larger, higher-quality refinements run opportunistically when time permits.