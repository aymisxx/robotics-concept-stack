# **Space Robotics & AI**

### **From Planetary Autonomy to Field Robots, Digital Twins, and Information-Guided Exploration**

**Focus:** autonomous exploration systems for Earth and space: modeling, estimation, control, perception, 3D reconstruction, semantic mapping, active sampling, and multi-robot decision-making under uncertainty.

---

# Why this course matters in robotics

Space Robotics & AI is where the whole robotics stack is forced to grow up.

A classroom robot can assume good lighting, flat floors, cheap failures, nearby humans, and convenient debugging.

A rover, drone, spacecraft, lander, or environmental field robot does not get that luxury.

It must operate with:

- limited communication,
- uncertain terrain,
- noisy sensors,
- strict energy limits,
- unknown environments,
- partial observability,
- expensive failure,
- and mission objectives that are usually scientific, not just navigational.

That is why this course is a synthesis course.

It pulls together:

- **linear algebra** for least squares, SVD, factor graphs, projections, and reconstruction,
- **state-space control** for LQR, LQG, MPC, controllability, and observability,
- **probability** for Bayes filters, Kalman filters, Gaussian processes, and sampling,
- **computer vision** for cameras, stereo, epipolar geometry, SfM, MVS, and visual servoing,
- **SLAM** for long-horizon localization and mapping,
- **semantic perception** for rocks, craters, trees, fruits, landmarks, and science targets,
- **generative and neural scene models** for NeRF, Gaussian splatting, DUSt3R, MASt3R, and digital twins,
- **information theory** for entropy, mutual information, active learning, and sample return,
- **multi-robot systems** for graph coordination, consensus, decentralized bandits, and communication-aware autonomy.

The core idea is simple but sharp:

> A space robot is not just a robot in space. It is an autonomous information-gathering machine operating under harsh physics, sparse knowledge, and mission-level constraints.

That is the course.

# How to use this document

This file is meant to work at three speeds.

## Mode 1: 5-minute refresh

Read:

- Big Picture Map.
- Core Formula Sheet.
- Robotics Connection Sheet.
- What to Never Forget.

## Mode 2: 1-hour revision

Read module summaries in order:

1. course framing and exploration autonomy.
2. least squares, MLE, and probabilistic foundations.
3. state-space models, LQR, LQG, MPC, controllability, and observability.
4. Bayes filters, Kalman filters, EKF, UKF, particles, and importance sampling.
5. platform stacks, GNC, PX4, rovers, drones, and lunar autonomy.
6. camera models, calibration, features, epipolar geometry, and stereo.
7. SfM, MVS, bundle adjustment, visual servoing, VIO, and SLAM.
8. semantic SLAM, tracking, data association, and mission-relevant landmarks.
9. scene representations, digital twins, NeRF, Gaussian splatting, DUSt3R, and MASt3R.
10. information theory, Gaussian processes, active learning, bandits, and optimal sampling.
11. multi-robot coordination, graphs, consensus, decentralized bandits, and adversarial uncertainty.

## Mode 3: Deep rebuild

Read everything and mentally reconnect:

- mission objective,
- robot state,
- sensor stream,
- uncertainty model,
- estimator,
- controller,
- map representation,
- semantic understanding,
- information objective,
- sampling strategy,
- team coordination,
- and field deployment workflow.

---

# Big Picture Map

At the highest level, this course answers eleven giant questions.

## 1. What is an exploration robot actually optimizing?

Not only distance or path length.

It may optimize:

- map accuracy,
- science value,
- energy usage,
- risk,
- sample return,
- localization confidence,
- coverage,
- communication robustness,
- or mission utility.

A planetary robot is often a moving sensor whose job is to collect the right information before time, power, terrain, or uncertainty closes the window.

## 2. How do we estimate structure from noisy data?

> Through least squares, maximum likelihood, Bayes filtering, Kalman filtering, and nonlinear estimation.

The course starts with the basic estimation spine because every later system depends on it.

## 3. How do we control systems whose full state is only partially measured?

> Use state-space models, controllability, observability, LQR, LQG, and MPC.

This is the GNC skeleton hiding underneath rovers, drones, landers, spacecraft, and autonomous survey robots.

## 4. How does a robot maintain belief instead of pretending it knows the truth?

> Use probability distributions over possible states.

That is the Probabilistic Robotics doctrine: uncertainty is not an annoyance, it is the representation.

## 5. How does 3D geometry emerge from 2D images?

> Through camera models, calibration, epipolar geometry, stereo, triangulation, SfM, and MVS.

This is the vision-to-terrain pipeline.

## 6. How does a robot localize and map over time?

> Through SLAM, factor graphs, pose graph optimization, landmark maps, loop closure, visual-inertial odometry, and bundle adjustment.

If localization lies, the mission becomes cartographic fan fiction.

## 7. How does perception become semantic enough for science?

> Replace anonymous features with meaningful landmarks: rocks, craters, trees, fruits, trunks, boulders, hazards, sample targets.

Semantic SLAM turns maps into decision-support systems.

## 8. How should a robot represent a scene?

As a point cloud, mesh, voxel grid, octree, surfel map, implicit field, radiance field, Gaussian splat set, neural pointmap, or hybrid digital twin.

Representation choice decides what the robot can plan, render, analyze, compress, and communicate.

## 9. How do digital twins support space robotics?

They let teams reconstruct, simulate, visualize, test, brief, and rehearse missions before hardware pays the tuition bill.

A good digital twin is not a pretty model. It is a mission-planning substrate.

## 10. Where should a robot sample next?

> Use entropy, mutual information, Gaussian processes, active learning, upper confidence bounds, bandits, and regret-aware decision-making.

The scientific question becomes a control question over information.

## 11. What changes when exploration becomes multi-robot?

Communication graph, local knowledge, decentralized decision-making, gossip, consensus, spectral gaps, adversarial/non-adversarial rewards, and network-wide regret all enter the room wearing steel-toed boots.

That is the course arc.

---

# Notation and foundational objects

We work with states, controls, measurements, maps, cameras, graphs, and distributions.

**State:**

$$
x_t \in \mathbb{R}^n
$$

**Control input:**

$$
u_t \in \mathbb{R}^m
$$

**Measurement:**

$$
z_t \in \mathbb{R}^p
$$

**Continuous-time state-space model:**

$$
\dot{x} = Ax + Bu, \qquad y = Cx + Du
$$

**Discrete-time nonlinear model:**

$$
x_{t+1} = f(x_t,u_t) + w_t
$$

$$
z_t = h(x_t) + v_t
$$

**Belief:**

$$
bel(x_t) = p(x_t \mid z_{1:t}, u_{1:t})
$$

**Linear regression model:**

$$
y = w^T x + \epsilon
$$

**Camera projection:**

$$
\lambda \tilde{p} = K[R \mid t]\tilde{P}
$$

**Stereo depth:**

$$
Z = \frac{fB}{d}
$$

**Feature track / semantic landmark:**

$$
\ell_j \in \mathbb{R}^3
$$

**Pose graph variables:**

$$
X = \{x_1, x_2, \dots, x_T\}
$$

**Map:**

$$
M = \{\ell_1, \ell_2, \dots, \ell_N\}
$$

**Graph:**

$$
\mathcal{G} = (\mathcal{V},\mathcal{E})
$$

**Adjacency matrix:**

$$
A_G = [a_{ij}]
$$

**Degree matrix:**

$$
D = \mathrm{diag}(d_1, \dots, d_N)
$$

**Graph Laplacian:**

$$
L = D - A_G
$$

**Gaussian process latent field:**

$$
f(x) \sim \mathcal{GP}(m(x), k(x,x'))
$$

**Information objective:**

$$
I(X;Y) = H(X) - H(X \mid Y)
$$

---

# Core Formula Sheet

## Least squares

For a dataset with design matrix $X$ and observations $Y$:

$$
\hat{w} = \arg\min_w \|Y - Xw\|^2
$$

Normal equations:

$$
X^T X \hat{w} = X^T Y
$$

Closed-form solution when $X^TX$ is invertible:

$$
\hat{w} = (X^T X)^{-1}X^T Y
$$

Pseudoinverse form:

$$
\hat{w} = X^+Y
$$

## Gaussian noise likelihood

$$
y_i \sim \mathcal{N}(w^Tx_i, \sigma^2)
$$

Likelihood:

$$
L(w) = \prod_{i=1}^N p(y_i \mid x_i, w, \sigma^2)
$$

For i.i.d. Gaussian noise, maximizing log-likelihood is equivalent to minimizing squared error.

## Bayes rule

$$
p(x \mid z) = \frac{p(z \mid x)p(x)}{p(z)}
$$

## Bayes filter

Prediction:

$$
\bar{bel}(x_t) = \int p(x_t \mid x_{t-1},u_t)bel(x_{t-1})dx_{t-1}
$$

Correction:

$$
bel(x_t) = \eta p(z_t \mid x_t)\bar{bel}(x_t)
$$

## Kalman filter

Prediction:

$$
\hat{x}_{k|k-1}=A\hat{x}_{k-1|k-1}+Bu_k
$$

$$
P_{k|k-1}=AP_{k-1|k-1}A^T+Q
$$

Kalman gain:

$$
K_k=P_{k|k-1}H^T(HP_{k|k-1}H^T+R)^{-1}
$$

Update:

$$
\hat{x}_{k|k}=\hat{x}_{k|k-1}+K_k(z_k-H\hat{x}_{k|k-1})
$$

$$
P_{k|k}=(I-K_kH)P_{k|k-1}
$$

## EKF linearization

$$
F_k = \frac{\partial f}{\partial x}\bigg|_{\hat{x}_k,u_k}, \qquad H_k = \frac{\partial h}{\partial x}\bigg|_{\hat{x}_k}
$$

## UKF sigma point count

For an $n$-dimensional Gaussian, the unscented transform commonly uses:

$$
2n+1
$$

sigma points.

## Importance sampling

Target expectation:

$$
\mathbb{E}_{p}[f(x)] = \int f(x)p(x)dx
$$

Sample from proposal $q(x)$:

$$
\mathbb{E}_{p}[f(x)] = \int f(x)\frac{p(x)}{q(x)}q(x)dx
$$

Weights:

$$
w_i = \frac{p(x_i)}{q(x_i)}
$$

## Controllability and observability

$$
\mathcal{C} = [B\ AB\ A^2B\ \cdots\ A^{n-1}B]
$$

$$
\text{controllable} \iff \mathrm{rank}(\mathcal{C}) = n
$$

$$
\mathcal{O} = \begin{bmatrix} C \\ CA \\ CA^2 \\ \vdots \\ CA^{n-1} \end{bmatrix}
$$

$$
\text{observable} \iff \mathrm{rank}(\mathcal{O}) = n
$$

## LQR

$$
J = \int_0^\infty (x^TQx + u^TRu)dt
$$

$$
u^* = -Kx
$$

$$
K = R^{-1}B^TP
$$

where $P$ solves the algebraic Riccati equation.

## LQG

$$
\text{LQG} = \text{LQR} + \text{Kalman filter}
$$

The controller and estimator can be designed separately under the separation principle.

## MPC idea

At every timestep:

1. solve a finite-horizon constrained optimization,
2. apply the first input,
3. replan using the new state estimate.

## Pinhole projection

For a 3D point in camera coordinates:

$$
u = f_x\frac{X}{Z}+c_x
$$

$$
v = f_y\frac{Y}{Z}+c_y
$$

Homogeneous form:

$$
\lambda
\begin{bmatrix}
u\\
v\\
1
\end{bmatrix}
=
K
\begin{bmatrix}
R & t
\end{bmatrix}
\begin{bmatrix}
X\\
Y\\
Z\\
1
\end{bmatrix}
$$

## Epipolar geometry

$$
\tilde{x}'^T F \tilde{x}=0
$$

For calibrated cameras:

$$
E = K'^T F K
$$

## Stereo depth uncertainty intuition

$$
Z = \frac{fB}{d}
$$

Small disparity error becomes large depth error when $d$ is small.

That means far-away terrain is geometrically meaner.

## Bundle adjustment

$$
\min_{\{T_i\},\{P_j\}} \sum_{i,j} \left\|p_{ij} - \pi(T_i,P_j)\right\|^2
$$

With covariance weighting:

$$
\min_X \sum_k r_k(X)^T \Omega_k r_k(X)
$$

## Visual servoing

Feature error:

$$
e = s - s^*
$$

Image-based control idea:

$$
\dot{q} = -\lambda J_s^+ e
$$

where $J_s$ or $L_s$ is the image Jacobian / interaction matrix.

## Entropy

$$
H(X) = -\sum_x p(x)\log p(x)
$$

## Joint entropy

$$
H(X,Y) = -\sum_{x,y}p(x,y)\log p(x,y)
$$

## Conditional entropy

$$
H(Y\mid X)=H(X,Y)-H(X)
$$

## KL divergence

$$
D_{KL}(P\|Q)=\sum_x P(x)\log\frac{P(x)}{Q(x)}
$$

## Mutual information

$$
I(X;Y)=H(X)-H(X\mid Y)
$$

Equivalent form:

$$
I(X;Y)=D_{KL}(p(x,y)\|p(x)p(y))
$$

## Gaussian process regression

Prior:

$$
f(X) \sim \mathcal{N}(m(X), K(X,X))
$$

Posterior mean:

$$
\mu_* = k_*^T(K+\sigma_n^2I)^{-1}y
$$

Posterior variance: σ² = k(x*, x*) − k*ᵀ (K + σn²I)⁻¹ k*

## GP-UCB sampling

$$
x_t = \arg\max_x \left[\mu_{t-1}(x) + \sqrt{\beta_t}\sigma_{t-1}(x)\right]
$$

## Regret

Instantaneous regret:

$$
r_t = f(x^*) - f(x_t)
$$

Cumulative regret:

$$
R_T = \sum_{t=1}^T r_t
$$

## Graph Laplacian

$$
L = D - A_G
$$

Consensus:

$$
\dot{x} = -Lx
$$

Algebraic connectivity:

$$
\lambda_2(L)
$$

The second-smallest Laplacian eigenvalue measures connectivity strength.

---

# Module 1: Exploration Robotics Framing - What the Course Is Actually About

## 1.1 Earth and space as one autonomy problem

The course deliberately connects planetary exploration and Earth field robotics.

That is not a branding trick.

Mars rovers, lunar robots, autonomous drones, environmental monitoring boats, geological mapping systems, and agricultural survey robots all share a similar architecture:

1. move through a real environment,
2. sense imperfectly,
3. estimate state and map,
4. choose where to go next,
5. collect mission-relevant data,
6. return results that humans can trust.

The environment changes, but the autonomy pattern remains.

A Mars rover and a lake-monitoring ASV both ask:

- Where am I?
- What do I know?
- What do I not know?
- What can I safely reach?
- Which measurement is worth spending energy on?
- How do I explain my result to a scientist or operator?

That is why the course uses both space and Earth science examples.

## 1.2 Rovers, orbiters, spacecraft, drones, and field robots

The course framing includes rovers, orbiters, spacecraft, UAVs, AUVs, ASVs, and physical robots in the DREAMS Lab.

These systems operate at different scales, but they are all sensing-control systems.

### Rovers

Rovers need:

- terrain traversability analysis,
- localization,
- hazard detection,
- energy-aware motion,
- science target prioritization,
- communication-aware autonomy.

### Orbiters and spacecraft

Orbiters and spacecraft need:

- navigation,
- attitude control,
- orbit determination,
- sensor planning,
- image acquisition,
- downlink prioritization.

### UAVs and field robots

UAVs and field robots need:

- onboard state estimation,
- flight control,
- mapping,
- visual perception,
- camera geometry,
- mission-level planning.

The platforms are different skins on the same skeleton: estimation, control, perception, mapping, decision-making.

## 1.3 Guidance, Navigation, and Control

GNC is the classical spaceflight decomposition.

### Guidance

Where should the vehicle go?

Guidance generates desired paths, trajectories, or mission-level commands.

### Navigation

Where is the vehicle actually?

Navigation estimates state from sensors and dynamics.

### Control

How should inputs be applied to follow the guidance command?

Control turns desired behavior into actuator commands.

In a planetary robot, GNC is not separate from perception.

The robot may need vision to navigate, SLAM to estimate state, semantic mapping to choose science targets, and information theory to decide what measurement should happen next.

## 1.4 Field autonomy is end-to-end

A course project in this area cannot be only a pretty algorithm.

The real stack is:

```text
mission goal
  -> physical platform
  -> sensors
  -> calibration
  -> estimation
  -> control
  -> perception
  -> map / scene representation
  -> decision policy
  -> evaluation metrics
  -> demo / visualization / documentation
```

That is why the course emphasizes deployable, research-grade projects.

The robot has to move from concept to code to simulator to evidence.

## 1.5 Digital twins as mission infrastructure

Digital twins appear repeatedly in the course.

A digital twin is a computational surrogate of a physical environment or system.

For this course, it is especially tied to:

- lunar terrain reconstruction,
- Earth analog sites,
- photogrammetry,
- Gaussian splatting,
- XR visualization,
- robot mission planning,
- high-resolution surface assessment,
- rover operations simulation.

A weak digital twin is just a rendering.

A strong digital twin supports:

- testing perception under different viewpoints,
- planning robot trajectories,
- simulating lighting and sensor constraints,
- validating reconstruction pipelines,
- communicating science context,
- and rehearsing mission operations.

## 1.6 The Lunar Autonomy Challenge as course DNA

The Lunar Autonomy Challenge captures the course in miniature.

The task is to control a virtual lunar robot around a lander in a simulated Moon environment.

The robot must:

- cover terrain efficiently,
- manage battery,
- map surface elevation,
- detect non-traversable rocks,
- finish within mission time.

This is exactly the integrated problem:

- navigation,
- mapping,
- obstacle detection,
- resource management,
- autonomy under constraints.

No single lecture topic solves it alone.

That is the point.

## 1.7 Engineering honesty

Space robotics is not a vibes domain.

It punishes:

- uncalibrated cameras,
- fake confidence,
- unbounded controllers,
- brittle perception,
- missing uncertainty,
- unrealistic simulation assumptions,
- and demos that only work when the moon is polite.

The right mindset is:

> build the stack, track the failures, quantify what worked, and say exactly where the assumptions break.

That is grown-up robotics.

---

# Module 2: Least Squares, MLE, and the Estimation Backbone

## 2.1 Why estimation starts the course

Autonomous exploration systems produce and consume measurements corrupted by noise.

That is not a side issue.

It is the normal condition.

GPS, IMU, lidar, cameras, wheel odometry, depth sensors, and scientific instruments all produce imperfect information.

So the first mature question is:

> given noisy data, what is the best estimate of the underlying state, model, line, map, pose, or field?

Least squares is the first answer.

Maximum likelihood is the probabilistic explanation of why that answer often appears.

## 2.2 Linear regression as the cleanest estimation example

Start with a line:

$$
y = mx + c
$$

Rewrite it using a bias term:

$$
y = w_0\cdot 1 + w_1x
$$

Vector form:

$$
x = [1 x]ᵀ
$$

$$
w = [c m]ᵀ
$$

Then:

$$
y = w^Tx
$$

With noise:

$$
y = w^Tx + \epsilon
$$

This looks tiny, but it is the seed of almost everything:

- calibration curves,
- sensor models,
- terrain regressions,
- local plane fitting,
- camera model fitting,
- system identification,
- Gaussian process baselines.

## 2.3 Matrix form

For $N$ observations:

$$
Y = Xw + \epsilon
$$

where:

$$
X = \begin{bmatrix}
1 & x_1 \\
1 & x_2 \\
\vdots & \vdots \\
1 & x_N
\end{bmatrix}, \qquad
Y = \begin{bmatrix}
y_1 \\
y_2 \\
\vdots \\
y_N
\end{bmatrix}
$$

The squared loss is:

$$
L(w)=\sum_{i=1}^N(y_i-w^Tx_i)^2
$$

The least-squares estimate is:

$$
\hat{w}=(X^TX)^{-1}X^TY
$$

when the inverse exists.

If the inverse is unhealthy or nonexistent, use QR, SVD, regularization, or the pseudoinverse.

### Engineering truth

Never worship the normal equations blindly.

They square the condition number.

For real numerical work, QR or SVD is often the cleaner tool.

## 2.4 Probabilistic interpretation

Assume Gaussian measurement noise:

$$
\epsilon_i \sim \mathcal{N}(0,\sigma^2)
$$

Then:

$$
y_i \mid x_i,w \sim \mathcal{N}(w^Tx_i,\sigma^2)
$$

For i.i.d. data:

$$
L(w)=\prod_{i=1}^N p(y_i\mid x_i,w,\sigma^2)
$$

Taking the log turns products into sums.

The log-likelihood contains the term:

$$
-\frac{1}{2\sigma^2}\sum_{i=1}^N(y_i-w^Tx_i)^2
$$

So maximizing likelihood is equivalent to minimizing squared error.

That is the least-squares and MLE bridge.

## 2.5 Why this matters for robotics

This one idea appears everywhere:

- fit a line to Pacific plate age-distance data,
- estimate a sensor calibration curve,
- compute camera parameters,
- refine robot pose,
- solve bundle adjustment,
- estimate landmark positions,
- fit terrain models,
- smooth noisy trajectories,
- learn environmental fields.

In space robotics, estimation is not a math exercise.

It is the difference between:

- landing safely or hitting a rock,
- mapping the terrain or hallucinating a map,
- choosing a sample or wasting a mission window,
- claiming a science result or producing statistical confetti.

## 2.6 Assignment connection: Pacific plate velocity

The Hawaii / Pacific plate assignment is not random geology garnish.

It teaches:

- linear regression,
- least squares,
- Gaussian residual modeling,
- confidence intervals,
- physical interpretation of slope,
- uncertainty-aware reporting.

A fitted slope is not just a number.

It is a model of motion inferred from noisy Earth science measurements.

That is exactly the Earth robotics version of the space autonomy problem: infer physical structure from incomplete data.

## 2.7 MLE, EM, and K-means

The lecture notes point to expectation maximization as an example of MLE.

EM alternates:

1. estimate hidden assignments or latent variables,
2. update parameters to maximize expected likelihood.

K-means can be interpreted as a special hard-assignment case of this broader probabilistic clustering idea.

Why this matters:

- semantic mapping often has hidden correspondences,
- multi-target tracking has hidden identities,
- data association is an assignment problem,
- mixture structure appears in terrain, science regions, and environment classes.

MLE is not just fitting lines.

It is the probabilistic backbone of estimating hidden structure.

---

# Module 3: State-Space Models, Control, LQR, LQG, and MPC

## 3.1 Why state-space models appear early

A robot evolves over time.

A model of that evolution is usually written as:

$$
x_{k+1}=Ax_k+Bu_k
$$

or in continuous time:

$$
\dot{x}=Ax+Bu
$$

This matters because exploration robots do not just estimate static lines.

They move.

They carry momentum.

They respond to input.

Their state must be predicted before it is corrected.

## 3.2 State, input, output

The basic linear model is:

$$
\dot{x}=Ax+Bu
$$

$$
y=Cx+Du
$$

where:

- $x$ is the state,
- $u$ is the control input,
- $y$ is the measured output,
- $A$ is the dynamics matrix,
- $B$ is the input matrix,
- $C$ maps state to measurement,
- $D$ maps input directly to output.

A good state vector is not decorative.

It is the minimal memory needed to predict the future.

For a cart-pole:

$$
x = [x, \dot{x}, \theta, \dot{\theta}]^T
$$

For a drone:

$$
x = [p, v, \phi,\theta,\psi, p_{rate},q_{rate},r_{rate}]^T
$$

depending on the level of model detail.

## 3.3 Controllability

A system is controllable if the input can move the system anywhere in state space.

For LTI systems:

$$
\mathcal{C}=[B\ AB\ A^2B\ \cdots\ A^{n-1}B]
$$

$$
\mathrm{rank}(\mathcal{C})=n
$$

This asks:

> do the controls reach all dynamic modes?

### Robotics meaning

A lunar rover can have a gorgeous planner, but if the actuation cannot influence a critical state, the planner is writing checks the hardware cannot cash.

## 3.4 Observability

A system is observable if the state can be inferred from outputs over time.

$$
O =
\begin{bmatrix}
C \\
CA \\
CA^2 \\
\vdots \\
CA^{n-1}
\end{bmatrix}
$$

$$
\mathrm{rank}(\mathcal{O})=n
$$

This asks:

> do the sensors reveal all dynamic modes?

### Robotics meaning

If a state cannot be observed, the estimator cannot invent it honestly.

It can only guess, drift, or borrow assumptions.

## 3.5 The tank problem intuition

The tank examples are useful because they make controllability concrete.

A coupled water-tank system can have a state for each tank level.

Depending on where input enters and how tanks exchange flow, one tank may influence the other or not.

That is the point:

- topology matters,
- actuator placement matters,
- sensor placement matters,
- equations reveal reachability.

This transfers directly to robot teams, spacecraft subsystems, and environmental sensor placement.

## 3.6 Types of control

The lecture separates several control modes.

### Open-loop control

No sensing.

The input is applied without correction.

Useful only when the model and environment are extremely predictable.

### Feedback control

Sense error and respond.

$$
e(t)=r(t)-y(t)
$$

This is the normal robot control posture.

### Feedforward control

Use a model or disturbance measurement to predict needed correction before error appears.

### Model predictive control

Plan over a finite horizon, apply the first input, then replan.

MPC is especially natural when constraints matter:

- actuator limits,
- terrain constraints,
- collision avoidance,
- energy limits,
- mission timing,
- safe landing corridors.

## 3.7 LQR

LQR chooses a feedback law:

$$
u=-Kx
$$

to minimize:

$$
J=\int_0^\infty(x^TQx+u^TRu)dt
$$

The matrix $Q$ says what state errors are expensive.

The matrix $R$ says what control effort is expensive.

This is why LQR is not just a controller.

It is a preference statement.

### Tuning interpretation

Large $Q_{ii}$:

- state $i$ must stay small.

Large $R_{jj}$:

- input $j$ is costly.

This is exactly what mission design does:

- keep attitude stable,
- keep energy use low,
- avoid large actuator commands,
- control error without thrashing hardware.

## 3.8 LQG

The lecture emphasizes:

$$
\text{LQG} = \text{LQR} + \text{LQE}
$$

where LQE is the Kalman-filter estimator.

The controller uses an estimated state:

$$
u=-K\hat{x}
$$

The estimator produces $\hat{x}$ from noisy measurements.

The separation principle says controller and estimator can be designed separately for linear systems with Gaussian noise.

### Space robotics meaning

This is the classical stack:

- model the dynamics,
- estimate the state,
- control the estimated state,
- quantify uncertainty.

The Apollo guidance computer and modern spacecraft navigation both live in this philosophical neighborhood.

## 3.9 MPC and receding horizon thinking

MPC solves:

$$
\min_{u_{0:N-1}} \sum_{k=0}^{N-1}(x_k^TQx_k+u_k^TRu_k)+x_N^TQ_fx_N
$$

subject to:

$$
x_{k+1}=f(x_k,u_k)
$$

and constraints:

$$
x_k\in\mathcal{X}, \qquad u_k\in\mathcal{U}
$$

Then it applies only $u_0$.

At the next step, it solves again.

That is receding-horizon control.

### Robotics meaning

MPC is attractive for:

- quadrotor acrobatics,
- payload throwing,
- lander descent,
- rover path constraints,
- power-aware traversal,
- field robots with safety constraints.

Its curse is computational cost.

Its blessing is constraint awareness.

## 3.10 Assignment connection: lawnmower, cart-pole, drone mapping

The assignments map directly onto the modeling ladder.

### Lawnmower survey with first-order dynamics

This teaches:

- waypoint following,
- coverage planning,
- first-order motion,
- basic control loops,
- trajectory error metrics.

### Cart-pole with LQR

This teaches:

- second-order dynamics,
- instability,
- linearization,
- controllability,
- optimal feedback.

### Drone-based mapping

This teaches:

- 3D motion,
- perception-control coupling,
- mapping as a mission objective,
- higher-order dynamics.

The progression is deliberate:

```text
line fitting -> first-order robot -> second-order control -> 3D autonomous mapping
```

That is how the course builds the tower.

---

# Module 4: Bayes Filters, Kalman Filters, EKF, UKF, Particles, and Sampling

## 4.1 Probabilistic robotics mindset

The lecture quotes the central probabilistic robotics idea:

> represent information by probability distributions over possible hypotheses.

That is the correct mental shift.

The robot does not know its state.

It maintains a belief over possible states.

This lets algorithms degrade gracefully when data is noisy, incomplete, or ambiguous.

## 4.2 Localization as Bayes filtering

Localization estimates the robot pose in an external reference frame using sensor data and a map.

Global localization is harder because the robot starts without knowing where it is.

The belief is:

$$
p(x_t\mid z_{1:t},u_{1:t})
$$

This is a probability distribution over possible robot poses.

Not a single point. Not a hope. A distribution.

## 4.3 Markov assumption

The Markov assumption says the future depends on the present state, not the entire past:

$$
p(x_t\mid x_{0:t-1},u_{1:t}) = p(x_t\mid x_{t-1},u_t)
$$

This is what makes recursive filtering possible.

Without it, the robot must carry the whole universe in memory.

Rude. Expensive. Unnecessary if the state is chosen well.

## 4.4 Bayes filter structure

Every Bayes filter has two steps.

### Prediction

Use motion model:

$$
\bar{bel}(x_t)=\int p(x_t\mid x_{t-1},u_t)bel(x_{t-1})dx_{t-1}
$$

### Correction

Use measurement model:

$$
bel(x_t)=\eta p(z_t\mid x_t)\bar{bel}(x_t)
$$

Prediction spreads uncertainty.

Correction sharpens it.

That little rhythm is the heartbeat of robot state estimation.

## 4.5 Kalman filter

For linear dynamics and Gaussian noise:

$$
x_k=Ax_{k-1}+Bu_k+w_k
$$

$$
z_k=Hx_k+v_k
$$

with:

$$
w_k\sim \mathcal{N}(0,Q), \qquad v_k\sim \mathcal{N}(0,R)
$$

The Kalman filter recursively estimates the mean and covariance.

The Kalman gain decides how much to trust prediction versus measurement.

If measurement noise is low, the filter trusts sensors more.

If process noise is low, it trusts the model more.

## 4.6 Why Kalman filters matter in space robotics

Kalman filtering is everywhere:

- spacecraft navigation,
- orbital estimation,
- IMU integration,
- VIO,
- rover localization,
- target tracking,
- hazard tracking during powered descent,
- multi-sensor fusion.

A Kalman filter is not old-school decoration.

It is one of the central machines of autonomous navigation.

## 4.7 EKF

Real systems are nonlinear:

$$
x_k=f(x_{k-1},u_k)+w_k
$$

$$
z_k=h(x_k)+v_k
$$

EKF linearizes around the current estimate using Jacobians:

$$
F_k=\frac{\partial f}{\partial x}
$$

$$
H_k=\frac{\partial h}{\partial x}
$$

Then it applies Kalman-style covariance propagation and correction.

### Engineering truth

EKF can become overconfident.

Linearization error can make covariance too optimistic.

That matters badly in safety-critical autonomy.

## 4.8 UKF

The Unscented Kalman Filter avoids explicit Jacobian linearization.

It chooses deterministic sigma points whose mean and covariance match the current Gaussian belief.

It pushes those sigma points through the nonlinear function.

Then it recombines them into a transformed mean and covariance.

For $n$ dimensions, a common sigma-point set uses:

$$
2n+1
$$

points.

### Mental model

EKF says:

> approximate the function.

UKF says:

> approximate the distribution.

That is why UKF can behave better for nonlinear transformations.

## 4.9 Particle filters

When the belief is nonlinear, non-Gaussian, or multimodal, particles become useful.

Approximate belief by samples:

$$
bel(x_t) \approx \{x_t^{[i]}, w_t^{[i]}\}_{i=1}^{M}
$$

Algorithm:

1. sample particles through motion model,
2. weight particles using measurement likelihood,
3. normalize,
4. resample.

Particle filters can represent multiple hypotheses.

That matters in global localization and ambiguous terrain.

## 4.10 Importance sampling

Importance sampling appears because sometimes we cannot easily sample from the target distribution.

We sample from a proposal distribution $q(x)$ and reweight:

$$
w_i = \frac{p(x_i)}{q(x_i)}
$$

This shows up in particle filters and broader Monte Carlo estimation.

The idea is powerful but fragile.

If $q$ misses important regions of $p$, weights collapse.

That is the sampling goblin in a lab coat.

## 4.11 Filtering hierarchy

A practical hierarchy:

```text
linear + Gaussian -> Kalman filter
nonlinear + approximately Gaussian -> EKF / UKF
nonlinear + non-Gaussian / multimodal -> particle filter
high-dimensional field -> GP / structured approximation / factor graph
```

The estimator should match the geometry of uncertainty.

Do not bring a Gaussian spoon to a multimodal soup fight.

---

# Module 5: Platforms, GNC, PX4, Rovers, Drones, and Lunar Autonomy

## 5.1 Why platforms matter

Algorithms only become robotics when they touch a platform.

The course repeatedly connects theory to:

- rovers,
- drones,
- spacecraft,
- lunar landers,
- PX4-based autonomy stacks,
- digital twin simulation environments,
- physical DREAMS Lab systems.

This matters because every platform changes the constraints.

A rover is terrain-limited.

A drone is power-limited and dynamically aggressive.

A spacecraft is fuel-limited and communication-limited.

A lander is time-limited and safety-critical.

## 5.2 PX4 / Pixhawk architecture

The Pixhawk / PX4 ecosystem separates low-level flight control from higher-level autonomy.

A typical architecture includes:

- flight controller,
- mission computer,
- onboard sensors,
- payload sensors,
- telemetry link,
- ground station,
- middleware,
- autonomy apps.

This separation is important.

The flight controller stabilizes the vehicle.

The mission computer runs heavier autonomy:

- perception,
- mapping,
- planning,
- collision avoidance,
- visual odometry,
- semantic analysis.

## 5.3 Companion computer pattern

A common drone robotics stack is:

```text
Pixhawk / PX4
  -> attitude and rate loops
  -> motor mixing
  -> sensor fusion

Companion computer
  -> ROS2 nodes
  -> perception
  -> SLAM
  -> mapping
  -> mission planning
  -> decision-making
```

This pattern is everywhere because timing matters.

Do not let a heavy neural network starve the attitude loop.

Gravity does not care about your segmentation model.

## 5.4 Quadrotor dynamics

The lecture covers quadrotor dynamics and control using the Mellinger / Kumar minimum-snap trajectory line of work.

The key model ingredients:

- thrust from rotor speeds,
- body attitude,
- angular rates,
- motor dynamics,
- rigid-body dynamics,
- nested feedback loops.

Rotor force is often modeled as:

$$
F_i = k_f \omega_i^2
$$

The system has coupled translation and rotation.

To move horizontally, a quadrotor must tilt.

That means position control and attitude control are deeply coupled.

## 5.5 Nested feedback loops

Quadrotors commonly use nested loops.

### Inner attitude loop

Runs fast, around the order of kHz.

Uses IMU data to control roll, pitch, and yaw.

### Outer position loop

Runs slower.

Uses position and velocity estimates to command desired attitude and thrust.

This hierarchy is why state estimation matters so much.

Bad velocity estimate, bad outer loop.

Bad attitude estimate, goodbye little aircraft.

## 5.6 Hover linearization

Near hover, the dynamics can be linearized around:

- zero velocity,
- zero or small roll/pitch,
- thrust balancing gravity,
- small angular rates.

This enables classical controller design.

But aggressive maneuvers violate hover assumptions.

That is where differential flatness and trajectory optimization become important.

## 5.7 Differential flatness

A system is differentially flat if all states and inputs can be written as algebraic functions of selected flat outputs and a finite number of their derivatives.

For quadrotors, flat outputs often include:

$$
\sigma = [x,y,z,\psi]
$$

This enables:

- polynomial trajectory planning,
- minimum-snap trajectories,
- aggressive maneuvers,
- efficient nonlinear trajectory generation.

## 5.8 Minimum snap intuition

Snap is the fourth derivative of position.

Minimum-snap trajectory generation chooses smooth trajectories that are dynamically feasible for quadrotors.

This matters because drones are not point masses that teleport through waypoints.

They must produce smooth thrust and attitude histories.

## 5.9 Lunar lander analogs and probe throwing

The lecture connects visual servoing, EDL, landing, and drone analog experiments.

The Earth analog objective:

> use a multirotor drone to throw a sensor probe and image the probe landing.

This connects:

- model predictive control,
- acrobatic maneuvering,
- payload delivery,
- visual observation,
- lunar lander analog experiments.

This is a tiny but rich example of exploration robotics.

The robot is not just moving.

It is deploying a sensor while controlling dynamics and gathering evidence.

## 5.10 Lunar Autonomy Challenge stack

The LAC simulator uses a custom CARLA / Unreal lunar environment.

The objective includes:

- local mapping,
- elevation estimation,
- rock detection,
- non-traversability prediction,
- battery-aware planning,
- limited mission time.

A reasonable autonomy pipeline would contain:

```text
sensor input
  -> ego-state estimate
  -> local terrain map
  -> traversability map
  -> elevation grid
  -> rock/hazard classifier
  -> coverage planner
  -> energy-aware controller
  -> mission monitor
```

This is the course compressed into one lunar sandbox.

---

# Module 6: Camera Models, Calibration, Features, Epipolar Geometry, and Stereo

## 6.1 Why camera models matter

A camera turns 3D geometry into 2D measurements.

That projection loses depth.

Recovering depth, pose, terrain, and structure from images requires understanding what was lost and what constraints remain.

The fundamental map is:

$$
\lambda \tilde{p}=K[R\mid t]\tilde{P}
$$

This separates:

- **intrinsics**: how the camera measures,
- **extrinsics**: where the camera is,
- **projective scale**: the lost depth factor.

## 6.2 Pinhole camera model

In camera coordinates:

$$
u = f_x\frac{X}{Z}+c_x
$$

$$
v = f_y\frac{Y}{Z}+c_y
$$

where:

- $f_x, f_y$ are focal lengths in pixels,
- $c_x, c_y$ are principal point coordinates,
- $Z$ is depth.

The division by $Z$ is the little demon that makes vision hard.

## 6.3 Coordinate frames

The lectures emphasize three coordinate systems:

- world frame,
- camera frame,
- image frame.

A point moves through transformations:

```text
world point -> camera coordinates -> normalized image coordinates -> pixel coordinates
```

If these frames are confused, everything downstream becomes cursed geometry soup.

## 6.4 Calibration

Calibration estimates camera parameters.

Important values:

- $f_x, f_y$,
- $c_x, c_y$,
- radial distortion coefficients $k_1,k_2,k_3$,
- tangential distortion coefficients $p_1,p_2$,
- stereo baseline,
- extrinsic rotation and translation.

The course uses ROS camera calibration examples, including RealSense D455 style calibration.

### Distortion

Real lenses create radial and tangential distortion.

The plumb-bob distortion model commonly uses:

$$
D=[k_1,k_2,p_1,p_2,k_3]
$$

Before applying the ideal pinhole model, image points should be undistorted.

### Engineering truth

Camera calibration is not optional paperwork.

It defines whether your geometry is metric or imaginary.

## 6.5 Feature detection and matching

Feature-based vision detects repeatable image structures and matches them across views.

The course discusses:

- direct patch correlation,
- low-level features,
- semantic features,
- ORB,
- SIFT / SURF context,
- optical flow,
- feature descriptors.

ORB stands for **Oriented FAST and Rotated BRIEF**.

It combines a FAST-style detector with a binary descriptor.

Why ORB matters:

- fast,
- lightweight,
- widely used in ORB-SLAM,
- practical for real-time robotics.

## 6.6 Direct versus indirect visual methods

### Direct methods

Compare image intensities or patches directly.

They can use more image information but may be sensitive to illumination and photometric assumptions.

### Indirect methods

Detect and match features.

They are often more sparse but easier to organize into geometric pipelines.

### Semantic feature methods

Use meaningful objects as features.

Examples:

- fruits,
- tree trunks,
- craters,
- boulders,
- rocks,
- hazards.

This becomes important in semantic SLAM.

## 6.7 Epipolar geometry

For two images of the same scene, corresponding points obey:

$$
\tilde{x}'^T F \tilde{x}=0
$$

The fundamental matrix $F$ maps a point in one image to an epipolar line in the other.

Given a point $x$, the matching point $x'$ must lie on:

$$
l' = F\tilde{x}
$$

This reduces correspondence search from a 2D image region to a 1D line.

Tiny miracle. Still fragile.

## 6.8 Essential matrix

For calibrated cameras:

$$
E = K'^T F K
$$

The essential matrix encodes relative rotation and translation direction between camera frames.

From $E$, we recover relative pose up to scale and ambiguity.

## 6.9 Eight-point algorithm

The eight-point algorithm estimates $F$ from point correspondences.

Pipeline:

1. normalize image points,
2. build a linear system $Af=0$,
3. solve using SVD,
4. reshape $f$ into $F$,
5. enforce rank-2 constraint,
6. unnormalize.

This is least squares wearing epipolar armor.

## 6.10 Calibrated versus uncalibrated cameras

Calibrated cameras preserve metric structure better because intrinsics are known.

Uncalibrated reconstruction is projective.

Depth and scale become ambiguous.

The lecture notes emphasize that calibration changes the number of unknowns and what can be recovered reliably.

## 6.11 Stereo depth

For rectified stereo:

$$
Z=\frac{fB}{d}
$$

where:

- $f$ is focal length,
- $B$ is baseline,
- $d$ is disparity.

Depth uncertainty grows as disparity becomes small.

Far terrain has small disparity.

Therefore, far terrain depth is less certain.

### Robotics meaning

Stereo is powerful for rover navigation and landing hazard detection, but its uncertainty is geometry-dependent.

Depth maps are not equally trustworthy everywhere.

## 6.12 Reading stereo like an engineer

When evaluating a stereo pipeline, ask:

- are cameras calibrated?
- are images rectified?
- what is baseline?
- what is focal length?
- what disparity range is expected?
- how does disparity error propagate to depth error?
- what happens on low-texture terrain?
- what happens at specular or shadowed rocks?

Stereo is not magic depth.

It is constrained triangulation under sensor and texture assumptions.

---

# Module 7: SfM, MVS, Bundle Adjustment, Visual Servoing, VIO, and SLAM

## 7.1 Structure from Motion

Structure from Motion reconstructs 3D structure and camera poses from image collections.

The classical SfM pipeline:

1. detect features,
2. match features,
3. estimate epipolar geometry,
4. recover relative camera poses,
5. triangulate sparse 3D points,
6. add more views,
7. run bundle adjustment.

SfM is usually offline or semi-offline.

It is used heavily in photogrammetry, terrain reconstruction, heritage mapping, and planetary analog reconstruction.

## 7.2 Multi-View Stereo

MVS densifies reconstruction after camera poses are known.

SfM gives sparse points.

MVS gives denser geometry.

In a terrain workflow:

```text
images -> camera poses -> sparse point cloud -> dense point cloud -> mesh / DEM / orthomosaic
```

This is why SfM and MVS are central to drone mapping and planetary surface reconstruction.

## 7.3 COLMAP and Agisoft

The course uses tools such as COLMAP and Agisoft Metashape.

### COLMAP

A widely used SfM/MVS system.

Good for metric reconstruction when camera data and images are good.

### Agisoft Metashape

A photogrammetry pipeline used for practical reconstruction workflows.

Both represent the classical approach:

- feature matching,
- geometry estimation,
- camera alignment,
- dense reconstruction,
- meshing.

## 7.4 Bundle adjustment

Bundle adjustment estimates camera poses and 3D points by minimizing reprojection error.

$$
\min_{\{T_i\},\{P_j\}} \sum_{i,j} \left\|p_{ij}-\pi(T_i,P_j)\right\|^2
$$

It is statistically justified under Gaussian projection noise.

It also produces large sparse least-squares problems.

### Why sparsity matters

Each camera observes only some landmarks.

So the Jacobian is sparse.

Exploiting sparsity is the difference between a solver and a space heater.

## 7.5 Reprojection error

Reprojection error compares measured image points to projected 3D estimates.

Pipeline:

1. estimate 3D point and camera pose,
2. project point into image,
3. compare projected pixel to observed pixel,
4. minimize discrepancies.

This is the error signal behind both photogrammetry and visual SLAM.

## 7.6 Visual SLAM

SLAM estimates robot trajectory and map simultaneously.

Visual SLAM uses camera data.

Typical front-end:

- feature detection,
- feature tracking,
- local pose estimation,
- keyframe selection,
- local mapping.

Typical back-end:

- loop closure,
- pose graph optimization,
- global bundle adjustment.

## 7.7 ORB-SLAM

ORB-SLAM uses ORB features and organizes the system into tracking, local mapping, loop closing, and map management.

Why it matters:

- practical monocular / stereo / RGB-D SLAM baseline,
- real-time capable,
- strong example of feature-based SLAM architecture.

But it depends on:

- stable visual features,
- correct matching,
- good initialization,
- loop closure that does not hallucinate,
- sufficient texture.

Planetary terrain can be mean here because it may be repetitive, low-texture, shadowed, or dusty.

## 7.8 Kimera

Kimera appears as a metric-semantic SLAM system.

It connects:

- visual-inertial odometry,
- pose graph optimization,
- mesh reconstruction,
- semantic labeling,
- 3D scene understanding.

This is important because modern robot maps are not only pose graphs.

They increasingly include surfaces, semantics, topology, and task-relevant structure.

## 7.9 SLAM as Bayes net and factor graph

The lectures frame SLAM using graphical models.

Pose variables:

$$
x_1,x_2,\dots,x_T
$$

Landmarks:

$$
\ell_1,\ell_2,\dots,\ell_N
$$

Measurements create factors between variables.

The optimization problem becomes:

$$
X^* = \arg\min_X \sum_k \|h_k(X)-z_k\|_{\Omega_k}^2
$$

where:

$$
\|r\|_{\Omega}^2 = r^T\Omega r
$$

This is nonlinear least squares at mission scale.

## 7.10 Terrain-relative navigation

Terrain-relative navigation uses perception to localize relative to observed terrain.

This matters for powered descent and landing.

Examples:

- hazard tracking,
- crater recognition,
- terrain patch matching,
- descent imagery alignment,
- landing-site correction.

The key idea:

> the terrain becomes a navigation sensor.

That is beautifully brutal.

## 7.11 Visual-inertial odometry

VIO fuses camera and IMU.

The camera gives geometric constraints but can be slow or ambiguous.

The IMU gives high-rate motion data but drifts.

Together:

- visual data corrects drift,
- inertial data bridges fast motion and weak imagery.

This is why VIO is central for drones and landing systems.

## 7.12 Visual servoing

Visual servoing uses visual feedback to control robot motion.

The lecture separates:

### Image-Based Visual Servoing

Use 2D image measurements directly.

Minimize feature error in image space:

$$
e=s-s^*
$$

Control uses image Jacobian / interaction matrix.

### Position-Based Visual Servoing

Recover 3D pose first, then control in Cartesian pose space.

### Hybrid methods

Combine image-space and pose-space advantages.

## 7.13 IBVS versus PBVS

IBVS is often robust to calibration and direct image error.

PBVS gives cleaner 3D geometric interpretation but depends more strongly on accurate pose estimation.

For multirotors without stabilized gimbals, camera motion and vehicle attitude are tightly coupled.

That makes the choice nontrivial.

If image features move because the vehicle tilts, the controller must understand that coupling.

## 7.14 Orbital docking and landing as visual servoing

The course asks whether orbital docking is visual servoing.

In spirit, yes.

If a vehicle closes a perception-control loop using image or sensor measurements of a target, it belongs to that family.

Applications:

- docking,
- station keeping,
- precision landing,
- target tracking,
- robotic manipulation,
- autonomous probe deployment.

Visual servoing is where perception stops narrating and starts commanding.

---

# Module 8: Semantic SLAM, Tracking, Data Association, and Mission-Relevant Landmarks

## 8.1 Why semantic SLAM exists

Traditional SLAM uses low-level geometric features.

Semantic SLAM uses meaningful objects as landmarks.

Traditional pipeline:

```text
SIFT / ORB features -> point cloud -> pose estimation
```

Semantic pipeline:

```text
objects / regions / landmarks -> meaningful map -> decision support
```

The course examples include:

- fruits,
- trunks,
- rocks,
- craters,
- boulders,
- hazards,
- geological structures.

## 8.2 Why semantics matter for exploration

Planetary missions care about meaning.

A rover does not only need to know:

> there is a feature at $(x,y,z)$.

It needs to know:

- is it a hazard?
- is it a rock?
- is it traversable?
- is it a science target?
- is it a crater rim?
- is it a sample site?
- does it indicate geology worth investigating?

A semantic map supports mission decisions.

## 8.3 Tracking is critical

The lectures emphasize tracking in planetary exploration:

- hazard tracking during powered descent,
- persistent landmark tracking for loop closure,
- multi-target tracking for sample site assessment.

Tracking turns frame-wise detections into temporal identity.

Without identity, a robot keeps rediscovering the same object and acting surprised every time.

Tiny robot amnesia. Bad for missions.

## 8.4 The fruit counting problem as semantic mapping

The fruit counting example is actually a space robotics lesson in disguise.

Problem:

- a camera sees fruits across many frames,
- the same fruit appears repeatedly,
- frame-wise detection massively overcounts.

Example intuition:

```text
10 fruits over 30 frames -> 300 detections, but only 10 objects
```

Solution:

- detect fruits,
- track across frames,
- associate detections,
- reconstruct 3D semantic landmarks,
- correct double counts.

Space analog:

- crater counting,
- rock survey,
- sample site assessment,
- landing hazard tracking.

## 8.5 Semantic SfM pipeline

The course presents a semantic SfM pipeline:

### Stage 1: Deep learning detection

Use FCN / Faster R-CNN style detectors for segmentation or bounding boxes.

### Stage 2: 2D tracking and association

Use:

- KLT optical flow,
- Kalman prediction,
- Hungarian assignment,
- track management,
- multi-hypothesis tracking.

### Stage 3: Semantic SfM

Use tracked semantic object centers as feature correspondences.

Then run bundle adjustment to estimate:

- 3D semantic landmarks,
- camera poses.

The big move:

> no SIFT matching needed for those semantic objects.

The object itself becomes the feature.

## 8.6 KLT optical flow

KLT tracks features using brightness constancy and local motion assumptions.

The basic idea is to minimize patch difference between frames:

$$
\|I(x,y,t)-I(x+u,y+v,t+1)\|^2
$$

For semantic tracking, fruit centroids or object points can become tracked features.

For space robotics, crater rims or rock landmarks can play the same role.

## 8.7 Hungarian algorithm

The data association problem:

> which detection in frame $t+1$ corresponds to which track from frame $t$?

Cost terms may include:

- predicted position difference,
- bounding box size similarity,
- IoU overlap,
- track age,
- semantic label consistency.

The Hungarian algorithm solves optimal bipartite assignment in polynomial time:

$$
O(n^3)
$$

This is not glamorous, but it is mission-critical plumbing.

## 8.8 Multi-hypothesis tracking

MHT maintains multiple possible association histories when ambiguity is high.

This matters when:

- objects disappear behind occlusion,
- detections are missed,
- multiple objects look similar,
- sensor noise causes identity ambiguity.

Space version:

- rocks in repetitive terrain,
- craters under shifting shadows,
- hazards during descent,
- moving dust or dynamic obstacles.

## 8.9 2D to 3D semantic data association

2D tracking can still double-count.

The course uses two correction ideas.

### Landmark reprojection

Project 3D semantic landmarks back into images.

Re-associate detections with existing landmarks.

This catches double tracking.

### Depth-based rejection

Use reference landmarks, such as tree trunks, to determine which side objects belong to.

In space terms, use terrain context to reject geometrically impossible associations.

## 8.10 Metric-semantic-topological mapping

The tree topology mapping example combines:

- trunk and branch detection,
- cylinder fitting,
- graph-SLAM smoothing,
- topology extraction.

This is important because some mission maps are not only metric.

They are structural.

A tree map, cave map, crater network, fracture network, or rock-field map may need topology and semantics, not just points.

## 8.11 Precariously Balanced Rocks and geological mapping

The course connects drone mapping, SfM, deep learning, and 3D rock segmentation to geology.

Precariously Balanced Rocks matter because they can act as negative indicators of earthquake shaking.

The robotics pipeline:

- collect aerial or field imagery,
- reconstruct 3D geometry,
- segment rocks,
- estimate shape and orientation,
- build a cyber-physical twin,
- simulate dynamics / fragility.

This is Earth science robotics with the same tools needed for planetary geology.

## 8.12 Lessons for space robotics

Semantic mapping gives several mature lessons.

### Semantic features improve robustness

Objects can be more stable than low-level texture.

### Multi-modal integration is powerful

Vision plus lidar plus DEM plus IMU can beat any one sensor alone.

### Correction layers are essential

3D validation catches 2D errors.

### Computational filtering matters

Track mission-relevant objects instead of every possible feature.

### Earth analogs transfer

Fruit counting becomes sample-site characterization.

Orchard traversal becomes crater-field traversal.

Yield estimation becomes resource assessment.

That is not a stretch.

It is the same algorithmic skeleton wearing a different helmet.

---

# Module 9: Scene Representations, Digital Twins, NeRF, Gaussian Splatting, DUSt3R, and MASt3R

## 9.1 Why scene representation is a core robotics question

A robot does not just need data.

It needs a representation of the world.

The representation determines:

- what can be rendered,
- what can be measured,
- what can be planned through,
- what can be compressed,
- what can be transmitted,
- what uncertainty can be tracked,
- what science can be extracted.

The course covers classical and neural scene representations.

## 9.2 Classical scene representations

### Point cloud

A set of 3D points.

Good for direct geometry.

Weak for topology, surfaces, and planning unless processed further.

### Mesh

Vertices and faces approximating surfaces.

Good for visualization and geometry.

Needs good reconstruction and cleanup.

### Voxels

3D grid cells.

Good for occupancy and planning.

Can be memory heavy.

### Octree

Hierarchical volumetric partition.

Good for multi-resolution occupancy and efficient storage.

### Surfels

Small oriented surface elements.

Good for dense mapping and local surface representation.

## 9.3 DEMs and orthorectified images

For aerial and planetary terrain:

- DEM gives elevation,
- orthomosaic gives map-corrected imagery,
- dense reconstruction supports slope and traversability.

This is central for drones, rovers, and lunar mapping.

A terrain model must not only look good.

It must support decisions:

- can the rover drive here?
- is this slope safe?
- where are rocks?
- where is the science target?
- how uncertain is elevation?

## 9.4 Digital twins

A digital twin for space robotics may combine:

- photogrammetry,
- lidar,
- imagery,
- semantic labels,
- simulation physics,
- XR visualization,
- mission planning interfaces.

The course highlights sub-centimeter lunar digital twin efforts for immersive mission planning.

The pipeline includes:

```text
multi-modal sensing
  -> reconstruction
  -> semantic analysis
  -> view synthesis
  -> simulation / visualization
  -> mission planning
```

## 9.5 Photogrammetry

Photogrammetry estimates 3D geometry from images.

Classical output:

- sparse point cloud,
- dense point cloud,
- mesh,
- textured model,
- DEM,
- orthomosaic.

For Apollo imagery, lunar analog rocks, or outdoor terrain, photogrammetry is a practical bridge from images to physical scene structure.

## 9.6 NeRF

Neural Radiance Fields learn a continuous volumetric scene function.

The model maps:

$$
(x,y,z,\theta,\phi) \rightarrow (c,\sigma)
$$

where:

- $c$ is color / radiance,
- $\sigma$ is density,
- viewing direction matters.

Rendering sends rays through the scene and integrates color/density.

### What NeRF is doing

It overfits a neural representation to one scene.

Different from typical supervised learning, the goal is not broad generalization.

The goal is a high-quality implicit representation of a specific scene.

### Why useful

NeRF can synthesize novel views.

For mission planning, that means:

- inspect terrain from unobserved viewpoints,
- create immersive scenes,
- augment sparse image datasets,
- support visualization and planning.

### Engineering truth

NeRF can look magical and still be geometrically unreliable.

Photorealism is not the same as metric correctness.

## 9.7 Gaussian Splatting

3D Gaussian Splatting represents a scene as many 3D Gaussian blobs.

Each splat has properties such as:

- position,
- covariance / scale,
- orientation,
- opacity,
- color coefficients.

Instead of ray-marching a neural field, splats are rendered efficiently.

This enables real-time radiance-field-style rendering.

### Why it matters

For digital twins and XR mission planning, rendering speed matters.

A digital twin that cannot render interactively becomes a museum artifact instead of a planning tool.

## 9.8 NeRF versus Gaussian Splatting

### NeRF

- implicit neural field,
- high-quality novel view synthesis,
- slower training/rendering depending on variant,
- continuous representation,
- sometimes hard to edit or measure directly.

### Gaussian Splatting

- explicit set of 3D splats,
- fast real-time rendering,
- strong visual quality,
- easier interactive visualization,
- may require careful cleanup and geometric interpretation.

For robotics, ask:

> does the representation support planning, measurement, and uncertainty, or only visual playback?

## 9.9 DUSt3R

DUSt3R reframes 3D reconstruction as direct dense pointmap regression from image pairs.

Key properties from the lecture:

- no prior camera calibration required,
- uses Transformer encoders/decoders,
- predicts dense 3D pointmaps,
- unifies depth estimation, pose estimation, matching, and reconstruction,
- supports monocular and binocular reconstruction scenarios,
- uses global alignment for multi-image settings.

Classical SfM solves many subproblems:

1. matching points,
2. estimating essential matrices,
3. triangulating,
4. sparse reconstruction,
5. camera estimation,
6. dense reconstruction.

DUSt3R tries to collapse much of this into learned dense geometric prediction.

## 9.10 COLMAP versus DUSt3R

### COLMAP

- classical SfM/MVS,
- strong metric accuracy when assumptions hold,
- needs good images and camera handling,
- established and robust,
- can be complex and CPU-heavy.

### DUSt3R

- learned direct reconstruction,
- easier with uncalibrated images,
- GPU-heavy,
- emerging ecosystem,
- potentially less precise in dense or complex scenes.

Engineering truth:

Use learned methods with respect, not blind worship.

If metric accuracy matters, validate against geometry.

## 9.11 MASt3R and MASt3R-SLAM

MASt3R extends the learned matching / 3D reconstruction direction and appears in the course as a bridge toward real-time dense SLAM with 3D reconstruction priors.

The key trend:

> learned geometric priors are entering SLAM front-ends and matching systems.

This is important for difficult planetary or field imagery where classical features struggle.

But the same warning remains:

learned priors must be validated under mission distribution.

## 9.12 Digital twin synthesis workflow

A mature digital twin workflow may combine:

- high-definition imaging,
- event cameras,
- lidar,
- IMU,
- photogrammetry,
- Gaussian splatting,
- NeRF / diffusion-based view synthesis,
- 3D semantic analysis,
- XR visualization,
- Unity or Unreal integration.

The course specifically connects this to lunar mission planning, Apollo imagery, ASU LROC imagery, and outdoor analog sites.

## 9.13 XR frontends

The course discusses devices like Varjo XR4, Meta Quest 3, and Apple Vision Pro.

The robotics point is not consumer gadget comparison.

The point is:

- immersive visualization can support collaborative mission planning,
- mixed reality can expose terrain models to operators,
- digital twins can become shared operational spaces.

This is robotics as interface, not just robotics as control theory.

## 9.14 The hard question

Can view synthesis improve photogrammetry and digital twinning?

Possible benefits:

- fill viewpoint gaps,
- improve visual immersion,
- generate alternate lighting/appearance,
- support training and rehearsal.

Possible risks:

- hallucinated geometry,
- synthetic artifacts,
- misleading terrain cues,
- loss of metric trust.

### Mature answer

Use generative view synthesis as augmentation and visualization.

Do not confuse it with measured truth unless validated.

The Moon does not care that your render had excellent vibes.

---

# Module 10: Information Theory, Gaussian Processes, Active Learning, Bandits, and Optimal Sampling

## 10.1 Why information theory enters robotics

Exploration is information acquisition with motion constraints.

A robot has limited:

- time,
- energy,
- samples,
- bandwidth,
- human attention,
- instrument use.

So the robot must ask:

> which measurement is most valuable?

Information theory gives mathematical language for that question.

## 10.2 Entropy

Entropy measures uncertainty or expected information content.

$$
H(X)=-\sum_xp(x)\log p(x)
$$

For a fair 8-sided die:

$$
H(X)=\log_2(8)=3\text{ bits}
$$

The course connects Boltzmann entropy and Shannon entropy conceptually:

- disorder,
- possibilities,
- information,
- surprise,
- coding length.

## 10.3 Joint and conditional entropy

Joint entropy:

$$
H(X,Y)=-\sum_{x,y}p(x,y)\log p(x,y)
$$

Conditional entropy:

$$
H(Y\mid X)=H(X,Y)-H(X)
$$

This measures remaining uncertainty about $Y$ after knowing $X$.

For sampling, this matters because measurements reduce uncertainty about a field.

## 10.4 KL divergence

KL divergence measures how different one distribution is from another:

$$
D_{KL}(P\|Q)=\sum_x P(x)\log\frac{P(x)}{Q(x)}
$$

It is not symmetric.

It is not a distance metric in the usual sense.

But it is extremely useful for comparing distributions.

## 10.5 Mutual information

Mutual information measures how much knowing one variable reduces uncertainty about another:

$$
I(X;Y)=H(X)-H(X\mid Y)
$$

For sensor placement:

> choose measurements that reduce uncertainty about the rest of the field.

This is why mutual information can beat raw entropy in sensor placement.

The lecture notes highlight an entropy criterion quirk:

- entropy can place sensors at boundaries,
- mutual information searches for sensor subsets that reduce uncertainty about unsensed regions.

## 10.6 Gaussian Processes

A Gaussian process is a distribution over functions:

$$
f(x)\sim\mathcal{GP}(m(x),k(x,x'))
$$

For any finite set of inputs, function values are jointly Gaussian.

This makes GPs natural for environmental fields:

- temperature,
- algae concentration,
- backscatter,
- vegetation index,
- terrain property,
- science value.

## 10.7 GP posterior

Given observations $y$ at locations $X$, prediction at $x_*$ has mean:

$$
\mu_* = k_*^T(K+\sigma_n^2I)^{-1}y
$$

and variance:

$$
\sigma_{*}^{2}
=
k(x_{*},x_{*})
- k_{*}^{T}
\left(K+\sigma_{n}^{2}I\right)^{-1}
k_{*}
$$

This gives both:

- predicted value,
- prediction uncertainty.

That is why GPs are so useful for active sampling.

## 10.8 Kernels

The kernel encodes assumptions about spatial structure.

Common examples:

### RBF / squared exponential

Smooth fields.

$$
k(x,x')=\sigma_f^2\exp\left(-\frac{\|x-x'\|^2}{2\ell^2}\right)
$$

### Periodic kernel

Periodic structure.

### Matérn kernel

Rougher functions, often more physically realistic.

Kernel choice is not cosmetic.

It is the prior over what nature is allowed to look like.

## 10.9 Active learning versus multi-armed bandits

The lectures make a clear distinction.

### Active learning

Goal:

- minimize model uncertainty.

Metrics:

- entropy,
- information gain,
- variance reduction.

Concern:

- sample diversity and global uncertainty reduction.

### Multi-armed bandits

Goal:

- maximize reward.

Metrics:

- reward,
- regret.

Concern:

- exploration-exploitation trade-off.

## 10.10 Exploration versus exploitation

A robot sampling an environment must decide:

- explore high-uncertainty regions,
- exploit high-value predicted regions.

Too much exploitation:

- miss future hotspots.

Too much exploration:

- return with low-value samples.

This is the sample-return knife edge.

## 10.11 GP-UCB

GP-UCB chooses samples using:

$$
x_t=\arg\max_x\left[\mu_{t-1}(x)+\sqrt{\beta_t}\sigma_{t-1}(x)\right]
$$

Mean term:

- exploitation.

Uncertainty term:

- exploration.

$\beta_t$ controls aggressiveness.

This is extremely relevant for:

- marine ecosystem monitoring,
- algae bloom sampling,
- agricultural disease monitoring,
- lunar sample prioritization,
- JWST target selection analogs,
- environmental robotics.

## 10.12 Optimal sensor placement

Optimal sensor placement asks:

> where should sensors be placed to maximize information about the field?

This can be NP-hard.

Greedy approximation methods become important.

The course notes discuss adaptive sampling and greedy sensor placement.

One key insight:

- a sequential closed-loop design may or may not provide advantage depending on the objective and assumptions.

This is not intuitive, which is why formal information criteria matter.

## 10.13 Optimal sample return

Optimal sample return is the robotics version of:

> I can only bring back a few things, which ones should they be?

Examples:

- rover rock samples,
- marine water samples,
- agricultural inspection samples,
- telescope observation targets,
- high-value scientific data packets.

The decision must balance:

- predicted value,
- uncertainty,
- cost to collect,
- travel time,
- energy,
- risk,
- future opportunity.

## 10.14 Online best-choice problem

The lectures connect sampling to optimal stopping and online best-choice problems.

This is useful when choices appear sequentially and cannot all be revisited.

In exploration:

- a rover may pass candidate sample sites,
- a drone may see transient targets,
- a spacecraft may have limited observation windows.

The robot must decide when to stop exploring and commit.

This is not just statistics.

It is mission timing.

## 10.15 JWST sampling optimization

The syllabus includes JWST sampling optimization as a hands-on project theme.

The robotics connection is conceptual:

- limited observation budget,
- high-dimensional scientific value,
- uncertainty over targets,
- scheduling constraints,
- reward versus information.

Even without wheels, the telescope is an autonomous sensing resource allocation problem.

Exploration is not always locomotion.

Sometimes it is pointing a precious instrument at the right part of the universe.

Tiny cosmic scheduler goblin, but elegant.

---

# Module 11: Multi-Robot Coordination, Graphs, Consensus, Decentralized Bandits, and Adversarial Uncertainty

## 11.1 Why multi-robot systems close the course

Single-robot exploration is hard.

Multi-robot exploration adds:

- communication topology,
- coordination,
- distributed sensing,
- local information,
- network delays,
- adversarial uncertainty,
- team-level regret,
- robustness to individual failures.

This is where planetary reconnaissance starts becoming a networked autonomy problem.

## 11.2 Multi-robot dimensions

The lecture frames multi-robot systems using:

- centralized versus distributed,
- fully connected versus partially connected,
- communication frequency,
- adversarial versus non-adversarial settings.

These are not implementation afterthoughts.

They define the math.

## 11.3 Graphs

Robots can be represented as nodes.

Communication, sensing, or interaction links are edges.

$$
\mathcal{G}=(\mathcal{V},\mathcal{E})
$$

Common graph types:

- connected,
- disconnected,
- directed,
- undirected,
- tree,
- star,
- complete,
- path,
- k-regular.

## 11.4 Graphs encode information and action flow

The course makes this point clearly:

Graphs encode who can:

- perceive whom,
- communicate with whom,
- share information,
- influence whom,
- coordinate actions.

A sensing graph may differ from a communication graph.

A communication graph may differ from an action graph.

A robot team is not one graph.

It is often several graphs stacked on one another.

## 11.5 Adjacency, degree, and Laplacian

Adjacency matrix:

$$
A_G=[a_{ij}]
$$

Degree matrix:

$$
D=\mathrm{diag}(d_i)
$$

Laplacian:

$$
L=D-A_G
$$

The Laplacian links graph structure to continuous vector-space dynamics.

This is why graph theory and control theory become friends.

Slightly awkward friends, but productive.

## 11.6 Algebraic connectivity

The Fiedler value is:

$$
\lambda_2(L)
$$

It is the second-smallest eigenvalue of the graph Laplacian.

It measures algebraic connectivity.

Larger $\lambda_2$ generally means stronger connectivity.

For multi-robot systems, this affects:

- consensus speed,
- robustness,
- information diffusion,
- formation cohesion,
- communication-aware planning.

## 11.7 Consensus

Consensus dynamics:

$$
\dot{x}=-Lx
$$

Robots use local neighbor differences to agree on a shared quantity.

Applications:

- rendezvous,
- alignment,
- distributed estimation,
- clock synchronization,
- average temperature estimation,
- shared reward statistics.

Consensus is the quiet arithmetic of robot teamwork.

## 11.8 Formation control

Formation control asks robots to maintain a spatial pattern.

The lecture emphasizes time-varying topology:

- robots can gain or lose neighbors,
- teams may split and rejoin,
- clutter can force temporary separation,
- leaders may control global motion.

This is important for exploration because communication and sensing links change with terrain.

## 11.9 Multi-armed bandits

A bandit problem has arms/actions.

At each round, an agent chooses an arm and receives a reward.

Goal:

- maximize cumulative reward,
- or minimize regret relative to the best fixed arm in hindsight.

The agent only sees rewards for chosen actions.

That partial feedback is the difficulty.

## 11.10 Non-adversarial versus adversarial bandits

### Non-adversarial

Rewards are drawn from fixed unknown distributions.

### Adversarial

Rewards may be chosen by an adversary aware of the strategy, though not necessarily the random draw.

Space/defense relevance enters here.

An exploration environment may be naturally uncertain.

A contested environment may actively deny, spoof, obscure, or manipulate information.

## 11.11 Distributed bandits

In distributed bandits:

- multiple agents pull arms,
- agents receive local rewards,
- agents communicate over a graph,
- the team wants low network-wide regret.

Each agent does not know:

- total number of times each arm was pulled across the network,
- global empirical reward means.

So agents approximate global statistics through communication.

## 11.12 Decentralized delayed UCB

The lecture discusses decentralized cooperative stochastic bandits and DDUCB-style ideas.

Core ingredients:

- UCB decision-making,
- gossip communication,
- accelerated consensus,
- delayed estimates of rewards and counts,
- regret penalty depending on network spectral properties.

The UCB rule is generally:

$$
\text{UCB}_k = \hat{\mu}_k + \text{exploration bonus}
$$

In a distributed setting, $\hat{\mu}_k$ and counts are approximated locally through network communication.

## 11.13 Gossip

Gossip algorithms average information over a graph.

A common update form:

$$
z(t+1)=Pz(t)+y(t)
$$

where $P$ respects communication topology and $y(t)$ is new local information.

The goal is approximate synchronization or averaging.

Gossip is what happens when the swarm whispers statistics until the network agrees.

## 11.14 Spectral gap and regret

The regret penalty in decentralized bandits depends on spectral properties of the communication matrix or graph.

Better connectivity means faster information mixing.

Poor connectivity means agents learn more slowly from each other.

This is the deep connection:

> network topology changes learning performance.

Not metaphorically. Mathematically.

## 11.15 Multi-agent planetary reconnaissance under adversarial uncertainty

This phrase captures a possible endpoint of the course.

Imagine a team of rovers, drones, orbiters, and surface sensors exploring a planetary site with:

- uncertain terrain,
- communication gaps,
- sensor degradation,
- adversarial interference or denial,
- limited samples,
- high-value science targets,
- energy constraints,
- decentralized decisions.

The stack required:

- state estimation,
- SLAM,
- semantic mapping,
- information-theoretic planning,
- multi-agent graph control,
- distributed learning,
- robust communication.

That is the full serpent.

And yes, it bites.

---

# The Full Concept Chain

This course is easiest to remember as one long ladder.

## Ladder

1. Start with **exploration as an information-gathering mission**.
2. Recognize that all measurements are noisy.
3. Use **least squares** to estimate model parameters from noisy data.
4. Interpret least squares through **maximum likelihood** under Gaussian noise.
5. Move from static fitting to **state-space dynamics**.
6. Analyze **controllability** and **observability**.
7. Use **LQR** to design optimal state feedback.
8. Use **Kalman filtering** to estimate hidden state.
9. Combine them as **LQG** under the separation principle.
10. Use **MPC** when constraints and receding-horizon planning matter.
11. Treat localization as **Bayes filtering**.
12. Use **EKF, UKF, particles, and importance sampling** as uncertainty becomes nonlinear or multimodal.
13. Connect platform autonomy to **GNC, PX4, rovers, drones, landers, and lunar simulators**.
14. Learn how 3D structure becomes pixels through the **camera model**.
15. Calibrate cameras so geometry becomes metric instead of decorative.
16. Use **features, epipolar geometry, stereo, and triangulation** to recover structure.
17. Use **SfM and MVS** to build 3D terrain models.
18. Use **bundle adjustment** to refine camera poses and landmarks.
19. Use **SLAM and factor graphs** to estimate trajectory and map over time.
20. Use **visual servoing** to close the perception-control loop.
21. Replace anonymous features with **semantic landmarks**.
22. Use **tracking, Hungarian assignment, KLT, Kalman prediction, and MHT** to maintain identity.
23. Build **metric-semantic-topological maps** for decision-making.
24. Represent scenes as **point clouds, meshes, voxels, octrees, surfels, implicit fields, radiance fields, and splats**.
25. Build **digital twins** for mission planning, simulation, XR, and reconstruction.
26. Use **NeRF, Gaussian splatting, DUSt3R, and MASt3R** with validation, not blind trust.
27. Use **entropy, KL, mutual information, and covariance** to quantify uncertainty.
28. Use **Gaussian processes** to model spatial fields.
29. Use **active learning and GP-UCB** to choose informative samples.
30. Use **bandits and regret** when reward and exploration compete.
31. Extend to **multi-robot systems** using graphs, Laplacians, consensus, gossip, and decentralized bandits.
32. End with autonomous Earth and space robots as **mission-driven information systems under uncertainty**.

That is the course arc.

---

# Robotics Connection Sheet

## A. Planetary rovers

Use:

- terrain-relative navigation,
- stereo mapping,
- rock and hazard detection,
- semantic SLAM,
- elevation grids,
- traversability maps,
- battery-aware planning,
- sample-site selection.

Key idea:

**A rover is a mobile scientific instrument with wheels and consequences.**

## B. Lunar landers and EDL

Use:

- visual servoing,
- terrain-relative navigation,
- hazard tracking,
- state estimation,
- model predictive control,
- landing-site assessment.

Key idea:

**Landing is perception-control under extreme timing pressure.**

## C. UAVs and Earth analogs

Use:

- PX4 / Pixhawk,
- ROS2,
- camera calibration,
- VIO,
- SfM / MVS,
- DEM generation,
- semantic mapping,
- probe deployment,
- field validation.

Key idea:

**Earth analog autonomy is the rehearsal room for planetary robotics.**

## D. Spacecraft and orbiters

Use:

- GNC,
- Kalman filtering,
- attitude control,
- sensor scheduling,
- image planning,
- communication-aware data return.

Key idea:

**A spacecraft is an estimator-controller wrapped around a mission instrument.**

## E. Digital twins

Use:

- photogrammetry,
- COLMAP / Agisoft,
- NeRF,
- Gaussian splatting,
- semantic labels,
- XR visualization,
- Unity / Unreal.

Key idea:

**A digital twin is useful only if it supports decisions, not just screenshots.**

## F. Environmental robotics

Use:

- Gaussian processes,
- active sampling,
- GP-UCB,
- sensor placement,
- field monitoring,
- marine / agricultural / ecological sampling.

Key idea:

**Environmental robotics is planetary robotics without needing a rocket.**

## G. Multi-robot exploration

Use:

- graphs,
- Laplacians,
- consensus,
- formation control,
- gossip,
- decentralized bandits,
- communication-aware sampling.

Key idea:

**A robot team is a distributed estimator and decision system.**

---

# What to Never Forget

## 1. Exploration is information gathering under constraints.

Distance traveled is not the final metric.

Mission value is.

## 2. Least squares is the first estimation engine.

It appears in regression, calibration, SLAM, SfM, bundle adjustment, and system identification.

## 3. MLE explains why least squares appears under Gaussian noise.

Squared error is not arbitrary.

It is a likelihood story.

## 4. Controllability asks whether inputs can move the state.

If rank fails, control dreams become theater.

## 5. Observability asks whether sensors reveal the state.

Unobservable states cannot be recovered honestly.

## 6. LQR is preference encoded as optimal control.

$Q$ and $R$ are not magic. They are engineering priorities.

## 7. LQG is LQR plus Kalman estimation.

State feedback often means estimated-state feedback.

## 8. A Bayes filter maintains belief.

Robots should represent uncertainty, not hide it under a rug.

## 9. EKF linearizes functions, UKF transforms sigma points.

Both are approximations. Neither is a spell.

## 10. Particle filters handle multimodal belief.

Useful when Gaussian assumptions are too clean for the mud.

## 11. Camera calibration creates metric trust.

Without calibration, geometry becomes suspicious.

## 12. Epipolar geometry turns matching into constrained search.

$$
\tilde{x}'^TF\tilde{x}=0
$$

## 13. Stereo depth gets worse with distance.

$$
Z=\frac{fB}{d}
$$

Small disparity errors can become large depth errors.

## 14. Bundle adjustment is sparse nonlinear least squares.

It is the mathematical furnace inside photogrammetry and VSLAM.

## 15. SLAM is joint estimation over pose and map.

Wrong data association can poison the entire map.

## 16. Visual servoing closes the loop through perception.

Vision becomes control, not just observation.

## 17. Semantic SLAM maps meaning, not only geometry.

Mission decisions need objects, hazards, samples, and context.

## 18. Digital twins must be decision tools.

Pretty render, no planning utility, no crown.

## 19. NeRF and Gaussian splatting are view-synthesis tools, not automatic truth machines.

Validate metric geometry.

## 20. DUSt3R and MASt3R show learned geometry entering reconstruction and SLAM.

Powerful, but mission distribution matters.

## 21. Entropy measures uncertainty.

Mutual information measures useful uncertainty reduction.

## 22. Gaussian processes give both prediction and uncertainty.

That is why they are natural for environmental fields.

## 23. Active learning minimizes uncertainty.

Bandits maximize reward while managing regret.

## 24. GP-UCB balances mean and uncertainty.

$$
\mu(x)+\sqrt{\beta}\sigma(x)
$$

## 25. Graph topology changes team intelligence.

Connectivity affects consensus, gossip, and decentralized learning.

## 26. Spectral gap matters.

Network structure has mathematical consequences.

## 27. Space robotics is system integration under punishment.

The field rewards clean assumptions only after they survive contact with reality.

---

# Intuition Anchors by Topic

## Least squares

Best-fit estimate by minimizing squared residuals.

## MLE

Choose parameters that make observed data most probable.

## Gaussian noise

The reason squared error becomes likelihood-optimal.

## Bayes filter

Prediction plus correction over a belief distribution.

## Kalman filter

Bayes filter for linear Gaussian systems.

## EKF

Kalman filter with local Jacobian linearization.

## UKF

Kalman-like filtering by propagating sigma points.

## Particle filter

Belief represented by weighted samples.

## LQR

Optimal feedback from quadratic state/input cost.

## LQG

LQR control using a Kalman state estimate.

## MPC

Repeated finite-horizon optimization with constraints.

## Pinhole camera

3D point divided by depth becomes pixel position.

## Calibration

Recover the camera parameters that make projection physically meaningful.

## Epipolar geometry

Two-view correspondence lives on a line, not anywhere in the image.

## Stereo

Depth from disparity.

## Bundle adjustment

Jointly refine cameras and 3D points by minimizing reprojection error.

## SLAM

Estimate pose and map together.

## Factor graph

Represent estimation as variables and measurement factors.

## Visual servoing

Use visual feature error to command motion.

## Semantic SLAM

Map objects and meaning, not just anonymous points.

## Digital twin

Reconstructed, simulated, and visualized environment used for planning and analysis.

## NeRF

Neural volumetric scene function for novel view synthesis.

## Gaussian splatting

Fast rendering using explicit 3D Gaussian primitives.

## DUSt3R

Learned dense pointmap reconstruction without requiring camera calibration.

## Entropy

Average uncertainty.

## Mutual information

Uncertainty reduction from measurement.

## Gaussian process

Distribution over functions with predictive mean and variance.

## Active learning

Choose samples to reduce uncertainty.

## Bandit

Choose actions to maximize reward under partial feedback.

## Regret

The cost of not always choosing the best action.

## Consensus

Distributed agreement through local neighbor exchange.

## Gossip

Randomized or structured local communication to average information.

## Algebraic connectivity

How strongly the graph holds together.

---

# 30-Years-Later Refresher

If future-me forgets almost everything, remember this skeleton:

1. Space robotics is **autonomous exploration under uncertainty**.
2. The robot is usually trying to gather **mission-relevant information**, not just move.
3. Noisy measurements lead first to **least squares** and **maximum likelihood**.
4. Dynamic systems require **state-space models**.
5. Useful control requires checking **controllability** and **observability**.
6. **LQR** gives optimal state feedback when a quadratic cost makes sense.
7. **Kalman filtering** estimates hidden state from noisy measurements.
8. **LQG** combines LQR and Kalman filtering.
9. **MPC** handles constraints by replanning over a horizon.
10. **Bayes filtering** is the general belief-update idea.
11. **EKF, UKF, and particle filters** are different approximations to nonlinear uncertainty.
12. Cameras map 3D to 2D through **projection**, and calibration is what makes that map usable.
13. **Epipolar geometry and stereo** recover 3D constraints from multiple views.
14. **SfM, MVS, and bundle adjustment** reconstruct terrain and camera motion.
15. **SLAM** turns perception into long-horizon localization and mapping.
16. **Visual servoing** uses vision inside the control loop.
17. **Semantic SLAM** makes maps meaningful for mission decisions.
18. **Scene representation** determines what planning, analysis, rendering, and science can do.
19. **Digital twins** support simulation, reconstruction, mission planning, and XR.
20. **NeRF, Gaussian splatting, DUSt3R, and MASt3R** are powerful but must be validated for metric truth.
21. **Entropy and mutual information** quantify uncertainty and measurement value.
22. **Gaussian processes** model spatial fields with uncertainty.
23. **Active learning and GP-UCB** decide where to sample next.
24. **Bandits and regret** formalize reward-seeking exploration.
25. **Graphs, Laplacians, consensus, and gossip** describe robot teams.
26. Multi-robot exploration is a distributed learning and control problem.
27. The whole field is one question: **what should the robot do next, given what it knows, what it does not know, and what the mission values?**

If this skeleton is alive, the rest can be rebuilt.

---

# Final Robotics Takeaway

This course teaches one deep lesson:

**autonomous exploration is not one algorithm.**

It is a stack.

A field or space robot must:

- estimate its state,
- control its body,
- model uncertainty,
- understand images,
- reconstruct geometry,
- build maps,
- assign meaning,
- decide what information matters,
- sample efficiently,
- coordinate with other agents,
- and report something scientifically useful.

The beginner sees a rover.

The mature roboticist sees:

```text
belief + dynamics + perception + representation + utility + constraints
```

That is why Space Robotics & AI is such a good final course.

It forces every earlier subject to cooperate.

Linear algebra, control, vision, machine learning, generative models, multi-robot systems, and mechatronics all become one mission machine.

The robot is not just moving through terrain.

It is moving through uncertainty.

---

# Minimal Core Formula Sheet

## Linear regression

$$
y=w^Tx+\epsilon
$$

## Least squares

$$
\hat{w}=\arg\min_w\|Y-Xw\|^2
$$

$$
\hat{w}=(X^TX)^{-1}X^TY
$$

## Gaussian likelihood

$$
y_i\mid x_i,w\sim\mathcal{N}(w^Tx_i,\sigma^2)
$$

## Bayes rule

$$
p(x\mid z)=\frac{p(z\mid x)p(x)}{p(z)}
$$

## Bayes filter

$$
\bar{bel}(x_t)=\int p(x_t\mid x_{t-1},u_t)bel(x_{t-1})dx_{t-1}
$$

$$
bel(x_t)=\eta p(z_t\mid x_t)\bar{bel}(x_t)
$$

## Kalman prediction

$$
\hat{x}_{k|k-1}=A\hat{x}_{k-1|k-1}+Bu_k
$$

$$
P_{k|k-1}=AP_{k-1|k-1}A^T+Q
$$

## Kalman update

$$
K_k=P_{k|k-1}H^T(HP_{k|k-1}H^T+R)^{-1}
$$

$$
\hat{x}_{k|k}=\hat{x}_{k|k-1}+K_k(z_k-H\hat{x}_{k|k-1})
$$

## Controllability

$$
\mathcal{C}=[B\ AB\ A^2B\ \cdots\ A^{n-1}B]
$$

$$
\mathrm{rank}(\mathcal{C})=n
$$

## Observability

$$
O =
\begin{bmatrix}
C \\
CA \\
CA^2 \\
\vdots \\
CA^{n-1}
\end{bmatrix}
$$

$$
\mathrm{rank}(\mathcal{O})=n
$$

## LQR

$$
J=\int_0^\infty(x^TQx+u^TRu)dt
$$

$$
u=-Kx
$$

## Camera projection

$$
\lambda \tilde{p}=K[R\mid t]\tilde{P}
$$

## Pinhole equations

$$
u=f_x\frac{X}{Z}+c_x
$$

$$
v=f_y\frac{Y}{Z}+c_y
$$

## Epipolar constraint

$$
\tilde{x}'^TF\tilde{x}=0
$$

## Essential matrix relation

$$
E=K'^TFK
$$

## Stereo depth

$$
Z=\frac{fB}{d}
$$

## Bundle adjustment

$$
\min_{\{T_i\},\{P_j\}}\sum_{i,j}\|p_{ij}-\pi(T_i,P_j)\|^2
$$

## Visual servoing

$$
e=s-s^*
$$

$$
\dot{q}=-\lambda J_s^+e
$$

## Entropy

$$
H(X)=-\sum_xp(x)\log p(x)
$$

## KL divergence

$$
D_{KL}(P\|Q)=\sum_xP(x)\log\frac{P(x)}{Q(x)}
$$

## Mutual information

$$
I(X;Y)=H(X)-H(X\mid Y)
$$

## Gaussian process

$$
f(x)\sim\mathcal{GP}(m(x),k(x,x'))
$$

## GP posterior 

GP posterior mean: μ* = k*ᵀ (K + σn²I)⁻¹ y

GP posterior variance: σ*² = k(x*, x*) − k*ᵀ (K + σn²I)⁻¹ k*

## GP-UCB

$$
x_t=\arg\max_x[\mu_{t-1}(x)+\sqrt{\beta_t}\sigma_{t-1}(x)]
$$

## Regret

$$
R_T=\sum_{t=1}^T(f(x^*)-f(x_t))
$$

## Graph Laplacian

$$
L=D-A_G
$$

## Consensus

$$
\dot{x}=-Lx
$$

## Algebraic connectivity

$$
\lambda_2(L)
$$

---

## **End Note**

This is not just a space robotics course.

It is the integration test for the whole robotics concept stack.

A space robot has to estimate, control, perceive, map, learn, sample, coordinate, and survive.

That is why this belongs at the end.

It is the capstone where all the quiet math finally puts on a helmet.

---