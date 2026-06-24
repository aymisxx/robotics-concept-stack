# **Multi-Robot System Dynamics**

### **From Stochastic Swarms to Networked Collective Intelligence**

**Focus:** durable understanding of how large robot collectives are modeled, analyzed, and controlled - from microscopic robot rules to macroscopic swarm behavior, from graph consensus to coverage and collective transport.

---

# Why this course matters in robotics

Multi-robot systems are where robotics stops being a single-machine problem and becomes a systems problem.

A single robot already has to sense, estimate, plan, and control itself.
A multi-robot system must do all of that **and** coordinate interactions among many robots under communication limits, sensing limits, failures, uncertainty, and geometry.

That makes this subject a convergence point of almost everything else in the stack:

- **Linear algebra** through Laplacians, spectra, nullspaces, eigenvalues, and pseudoinverses.
- **Dynamics and control** through stability, Lyapunov analysis, feedback design, and constrained control.
- **Robot modeling** through kinematics, rigid transformations, and motion constraints.
- **Perception and estimation** through mapping, filtering, occupancy grids, and target tracking.
- **Machine learning / RL intuition** through population models, uncertainty, and distributed decision-making.

This course matters because it answers the central question of swarm and team robotics:

> How do simple local rules, limited information, and distributed control produce useful global collective behavior?

That question shows up everywhere:

- warehouse fleets,
- drone swarms,
- distributed sensing,
- search and rescue,
- mapping of dangerous environments,
- environmental monitoring,
- formation control,
- collective transport,
- self-assembly,
- and large-scale autonomy in denied or communication-poor environments.

The deep lesson is that multi-robot systems are not just "many copies of one robot."
They are networks, populations, fields, graphs, probability distributions, and geometry - all at once.

# How to use this document

This file is meant to work at three speeds.

## Mode 1: 5-minute refresh

Read:
- Big Picture Map.
- Core Formula Sheet.
- Robotics Connection Sheet.
- What to Never Forget.

## Mode 2: 1-hour revision

Read the modules in order:

1. course framing, architectures, and model classes.
2. stochastic processes and mean-field swarm models.
3. graph-based consensus, flocking, and synchronization.
4. potential-field and navigation-function control.
5. optimization, connectivity, and safety constraints.
6. geometric representations and coverage control.
7. estimation, mapping, and exploration.
8. collective manipulation and transport.

## Mode 3: Deep rebuild

Read everything and mentally connect:
- local robot rules,
- collective state abstractions,
- graph topology,
- distributed feedback,
- probabilistic population models,
- geometric deployment,
- safety constraints,
- and manipulation by teams.

---

# Big Picture Map

At the highest level, this course answers eight giant questions.

## 1. What even counts as a multi-robot system?

> A set of robots whose useful behavior depends not only on individual dynamics, but on **interaction structure**.

## 2. How should we model a collective?

> At multiple levels: **microscopic**, **mesoscopic**, **macroscopic**, graph-based, geometric, and probabilistic.

## 3. How can a swarm redistribute itself without central assignment?

> Through **stochastic state switching**, **Markov chains**, **chemical reaction network models**, and **mean-field dynamics**.

## 4. How do robots agree on a common value or motion?

> Through **consensus**, **Laplacian dynamics**, and graph-theoretic conditions on connectivity.

## 5. How can robots spread out, avoid obstacles, or form patterns?

> Through **potential fields**, **navigation functions**, **implicit functions**, and geometric controllers.

## 6. How do optimization and safety enter the picture?

> Through **convex optimization**, **semidefinite programs**, **algebraic connectivity maximization**, and **control barrier functions**.

## 7. How do collectives reason about space?

> Through **Voronoi partitions**, **Lloyd algorithms**, **shape abstractions**, **occupancy maps**, **Bayes filters**, and **PHD filters**.

## 8. How do multiple robots physically manipulate the world together?

> Through **collective transport**, **force / form / object closure**, decentralized force coordination, and cooperative manipulation.

That is the course arc.

---

# Notation and foundational objects

We mostly work with robot states, graph objects, densities, and collective variables.

**Robot state:**

$$
x_i \in \mathbb{R}^n
$$

**Robot position in the plane:**

$$
r_i = \begin{bmatrix} x_i \\
y_i \end{bmatrix}
$$

**Single-integrator model:**

$$
\dot r_i = u_i
$$

**Double-integrator model:**

$$
\dot r_i = v_i, \qquad \dot v_i = u_i
$$

**Graph:**

$$
\mathcal{G} = (\mathcal{V}, \mathcal{E})
$$

**Adjacency matrix:**

$$
A = [a_{ij}]
$$

**Degree matrix:**

$$
D = \mathrm{diag}(d_1,\dots,d_N)
$$

**Graph Laplacian:**

$$
L = D - A
$$

**Consensus state vector:**

$$
x = [x_1, x_2, \dots, x_N]^T
$$

**Continuous-time consensus dynamics:**

$$
\dot x = -Lx
$$

**Mean-field population fraction vector:**

$$
x(t) = [x_1(t), x_2(t), \dots, x_S(t)]^T
$$

with conservation constraint:

$$
\sum_{i=1}^{S} x_i(t) = 1
$$

**Bayes filter state estimate:**

$$
p(x_t \mid z_{1:t}, u_{1:t})
$$

**Occupancy log-odds:**

$$
\ell_t(m) = \log \frac{p(m \mid z_{1:t}, s_{1:t})}{1-p(m \mid z_{1:t}, s_{1:t})}
$$

**Control barrier function safe set:**

$$
\mathcal{C} = \{x : h(x) \ge 0\}
$$

---

# Module 1: Multi-Robot Systems Framing - What the Course Is Actually About

## 1.1 Single robot versus collective

A single robot problem asks:
- what is the robot state,
- how does it move,
- what does it sense,
- what control input should it use?

A multi-robot problem asks all of that **plus**:
- who interacts with whom,
- what information is shared,
- what variables should synchronize,
- what global behavior should emerge,
- how collective performance scales with team size,
- and how failure of individual robots changes the whole.

That extra layer is why this subject feels so broad.
It is not broad by accident.
It is broad because collectives can be modeled from several valid viewpoints.

## 1.2 Major application classes

The overview lectures place multi-robot systems across a huge application space:
- environmental monitoring and restoration,
- infrastructure inspection and maintenance,
- agriculture and mining,
- logistics and warehouses,
- manufacturing and construction,
- search and rescue,
- defense and ISR,
- telecommunications,
- exploration,
- targeted medicine at micro and nano scales.

That range matters.
It means the course is not teaching one platform.
It is teaching reusable coordination principles.

## 1.3 Swarms versus coordinated teams

A very important distinction appears early.

### Swarms
- many relatively simple robots,
- usually decentralized,
- usually local communication or no communication,
- behavior emerges from simple rules,
- strong emphasis on scalability and resilience.

### Coordinated teams
- fewer, often more capable robots,
- more structured communication,
- more explicit cooperation,
- often distributed or centralized planning.

This is not a rigid binary.
It is a spectrum.
But the distinction helps because methods that work elegantly for 100 simple robots are not always the same methods used for 4 highly capable robots carrying a payload.

## 1.4 Control architectures

The early modules also separate control architectures into:

### Centralized
A central node has global information and makes decisions.

### Distributed
Each robot computes locally, but information may still be exchanged across the network.

### Decentralized
Each robot relies only on local sensing and perhaps minimal or no communication.

These are not just implementation details.
They determine:
- scalability,
- robustness to failures,
- communication burden,
- theoretical analysis tools,
- and what assumptions are realistic.

## 1.5 Model classes

The Sept. 3 lecture does something subtle but very important: it places multi-robot models inside classical system-theory categories.

Systems may be:
- distributed-parameter or lumped-parameter,
- stochastic or deterministic,
- discrete-time or continuous-time,
- nonlinear or linear,
- time-varying or time-invariant,
- open-loop or closed-loop.

That matters because the course is quietly saying:

> multi-robot systems are not outside control theory.
> They are a special, difficult, networked instance of it.

## 1.6 Point-mass and vehicle models

Before the collective layer, the course still needs robot motion models.
Typical abstractions include:

### Fully actuated point mass

$$
\dot r_i = u_i
$$

### Double integrator

$$
\dot r_i = v_i, \qquad \dot v_i = u_i
$$

### Wheeled / nonholonomic models
with forward velocity and angular velocity as inputs.

This becomes crucial later, because many beautiful collective controllers are first derived for point-mass robots and then must be translated to real platforms.

## 1.7 Why implementation matters

The Robotarium lecture is a reality slap.
A consensus law like

$$
\dot x_i = \sum_{j \in \mathcal{N}_i}(x_j - x_i)
$$

may have elegant guarantees, but implementation on differential-drive hardware immediately raises problems:
- finite sensing range,
- actuator limits,
- nonholonomic motion,
- finite robot size,
- delays,
- disturbances,
- barrier-certificate conservatism,
- and possible deadlock.

That lecture is important because it prevents the whole course from turning into theorem cosplay.

# Module 2: Stochastic Processes and Mean-Field Models

## 2.1 Why stochastic swarm models appear

If a swarm is large, tracking each robot exactly can be the wrong abstraction.

Instead of asking:
- where is robot 137 right now?

we ask:
- what fraction of robots are in each state?
- how does that population distribution evolve?
- what controller parameters shape the equilibrium distribution and convergence rate?

That is the mean-field / population viewpoint.

## 2.2 Three modeling levels

This module is easiest to remember as three levels.

### Microscopic model
Tracks individual robots and their random transitions.

### Mesoscopic model
Tracks probability distributions over robot population states, often through a master equation.

### Macroscopic model
Tracks expected concentrations or fractions through ODEs or PDEs.

The course repeatedly moves between these levels.
That is a major conceptual theme.

## 2.3 Stochastic state switching and Markov chains

Suppose a robot can occupy one of several discrete task states or sites.
It may switch from state $i$ to state $j$ with rate $k_{ij}$ or probability proportional to $c_{ij} \Delta t$ in a short time step.

Then the swarm distribution evolves like a continuous-time Markov chain at the population level.

A simple linear mean-field model has the form:

$$
\dot x = Kx
$$

with conservation of total population.

Interpretation:
- each entry of $x$ is the fraction of robots at a site or in a task,
- the matrix $K$ encodes transfer rates,
- equilibrium corresponds to desired task allocation.

## 2.4 Chemical reaction network (CRN) viewpoint

One of the coolest moves in the course is to reinterpret robot task transitions as chemical reactions.

A reaction like

$$
X_i \to X_j
$$

means a robot switches from task $i$ to task $j$.

A bimolecular reaction can represent interaction-dependent switching, such as a robot changing behavior only after encountering another robot or object.

This is powerful because CRN theory already gives tools for:
- equilibrium analysis,
- stability reasoning,
- network structure interpretation,
- stochastic simulation,
- and robustness to parameter variation.

## 2.5 Gillespie simulation and the stochastic physical basis

The module emphasizes that the stochastic formulation has the more physically faithful microscopic basis.

The Gillespie direct method simulates reaction events and event times for the CRN view of the swarm.

This matters because deterministic ODE models are approximations of expected behavior.
They may miss fluctuations, rare transitions, and finite-population effects.

That is why the notes explicitly warn:
- macro models are powerful,
- but the mesoscopic / stochastic model can exhibit behaviors absent from the mean ODE.

## 2.6 Mass-action kinetics and multi-affine dynamics

Under mass-action assumptions, reaction rates are proportional to products of reactant concentrations.

That gives nonlinear but structured mean-field models.
For a 2-state example, a multi-affine vector field might look like:

$$
f(x_1, x_2) = c_0 + c_1 x_1 + c_2 x_2 + c_3 x_1 x_2
$$

This is one of the course's quiet lessons:

> swarm dynamics can be nonlinear even when individual switching rules look simple.

## 2.7 Mean-field equilibrium analysis

Once we have a macroscopic ODE

$$
\dot x = f(x),
$$

we care about:
- existence of equilibrium,
- uniqueness,
- stability,
- convergence rate,
- controllability / stabilizability,
- and sensitivity to parameters.

Lyapunov methods become the main stability language.
CRN theory and graph theory may also help characterize equilibrium structure.

## 2.8 Canonical applications

### Stochastic swarm allocation
Robots redistribute among sites to achieve a target fraction without explicit centralized assignment.

### Nest-site style collective decision-making
Bio-inspired collective selection between options.

### Swarm robotic assembly
Robots assemble and disassemble products using probabilistic policies, and the probabilities are optimized for fast convergence.

### Inspection / monitoring
Population fractions evolve as robots encounter assets and execute local behaviors.

These applications all share the same deep structure:
- local stochastic rule,
- global population objective,
- macroscopic model for design and analysis.

## 2.9 Random walks and exploration

Brownian motion and Levy walks also enter through this stochastic lens.

Why?
Because for exploration by many simple robots:
- explicit planned trajectories may be impossible,
- localization may be weak,
- communication may be absent,
- and random motion can still provide strong coverage behavior.

A Levy walk uses long-tailed step-length statistics, often giving more aggressive dispersion toward unexplored regions than ordinary Brownian-style motion.

## 2.10 Robotics meaning

This entire module teaches a very powerful design philosophy:

> Do not always control each robot directly.
> Sometimes it is better to control the **distribution**.

That idea is deeply relevant to:
- swarms,
- distributed sensor deployments,
- task allocation,
- self-assembly,
- statistical control,
- and large-population autonomy.

---

# Module 3: Graph-Based Network Models - Consensus, Flocking, and Synchronization

## 3.1 Why graphs are the right language

If robots exchange information or influence each other, then we need a structure for who interacts with whom.
That structure is a graph.

Vertices represent robots.
Edges represent communication, sensing, or influence.

This single abstraction powers:
- consensus,
- rendezvous,
- formation control,
- flocking,
- synchronization,
- distributed mapping,
- and connectivity maintenance.

## 3.2 Adjacency, degree, and Laplacian

For a graph with adjacency matrix $A$ and degree matrix $D$,

$$
L = D - A
$$

is the Laplacian.

This matrix is one of the central objects of the whole course.

Why?
Because the continuous-time consensus law becomes:

$$
\dot x = -Lx
$$

and a huge amount of behavior is encoded in the spectral properties of $L$.

## 3.3 Consensus as distributed averaging

Consensus algorithms update robot information states using neighbors' values.
The goal is for all states to converge to a common value.

For a scalar state $x_i$,

$$
\dot x_i = \sum_{j \in \mathcal{N}_i} a_{ij}(x_j - x_i)
$$

or in compact form:

$$
\dot x = -Lx.
$$

Interpretation:
- if my value is below neighbors, I move upward,
- if above neighbors, I move downward,
- eventually the network smooths itself to agreement.

## 3.4 Undirected versus directed graphs

### Undirected graph
$L$ is symmetric positive semidefinite.
All nonzero eigenvalues are positive.

### Directed graph
$L$ need not be symmetric.
Nonzero eigenvalues have positive real parts under appropriate connectivity conditions.

That asymmetry matters for both convergence guarantees and what equilibrium is reached.

## 3.5 Connectivity and algebraic connectivity

For undirected graphs, the second-smallest eigenvalue of $L$ is the algebraic connectivity:

$$
\lambda_2(L).
$$

This value is positive if and only if the graph is connected.
It also governs convergence speed of consensus.

So:
- larger $\lambda_2$ means faster information diffusion,
- smaller $\lambda_2$ means slower convergence and weaker connectivity robustness.

This is one of the most important eigenvalue interpretations in all of robotics.

## 3.6 Average consensus

Consensus means agreement.
Average consensus means agreement at the average of initial conditions.

For undirected connected graphs, average consensus is achieved naturally.
For directed graphs, stronger conditions are needed, especially strong connectivity and balance.

This distinction matters because not every consensus law preserves the network average.

## 3.7 Time-varying graphs

Real robot networks change.
Robots move, links drop, and communication ranges matter.

So the graph may become $\mathcal{G}(t)$, and the Laplacian becomes time-varying.

The course emphasizes union-of-graphs conditions:
- consensus can still be achieved if across repeated bounded time windows, the union graph has sufficient rooted spanning-tree structure.

This is a deeply practical result:

> the network does not need to be connected at every instant,
> but connectivity must appear often enough over time.

## 3.8 Consensus with a reference state

Sometimes not all robots just want to agree with each other.
They need to agree on a leader state or reference value.

Examples:
- formation around a virtual leader,
- consensus on a target heading,
- collective tracking of a broadcast state.

This adds leader-follower structure and rooted spanning tree conditions.

## 3.9 Flocking control

Flocking is consensus plus geometry plus collision avoidance plus cohesion.

A classical flocking objective is:
- align velocities,
- avoid collisions,
- maintain cohesion / preferred spacing,
- move as one group.

A typical double-integrator flocking controller combines:
- velocity alignment terms,
- potential-based spacing terms.

This is where graph theory and potential functions meet.

## 3.10 Synchronization of coupled oscillators

The course also includes synchronization, where each robot is modeled as an oscillator or phase variable and pairwise couplings produce collective rhythmic behavior.

This is a useful reminder that consensus is not only about positions.
It can also be about:
- phase,
- heading,
- attitude,
- frequency,
- or map beliefs.

## 3.11 Mapping over communication graphs

A nice application appears when robots update occupancy maps and exchange map information only within communication range.

Then mapping itself becomes a consensus-like information diffusion process over a time-varying graph.

This is a very mature idea:
- not only motion,
- but also knowledge,
can be distributed over a graph.

## 3.12 Implementation reality check

The Robotarium lecture hammers home that the ideal single-integrator consensus model ignores:
- actuator saturation,
- finite sensing range,
- differential-drive constraints,
- body geometry,
- latency,
- disturbances.

A practical fix is to map single-integrator outputs to unicycle or differential-drive commands using a near-identity diffeomorphism.

That is a huge engineering lesson:

> collective control laws often live in a mathematically convenient space first,
> and only then get translated to actual vehicle dynamics.

---

# Module 4: Potential Field-Based Control and Navigation Functions

## 4.1 Why potential fields appear

Potential-field methods define a scalar function over space and let the robot move along the negative gradient.

Basic rule:

$$
u = -K \nabla \phi(q)
$$

or in planar coordinates:

$$
\dot q = -K \nabla \phi(q).
$$

Interpretation:
- goals are made attractive,
- obstacles are made repulsive,
- motion follows descent of an energy-like landscape.

This is implicit motion planning: planning and control are collapsed into one vector field.

## 4.2 Classical attractive and repulsive fields

In the simplest 2D version:
- attractive potential grows with distance to goal,
- repulsive potential blows up near obstacles.

Total potential is their sum.
The robot moves downhill.

This is conceptually elegant and computationally simple.
It is also notoriously cursed if used naively.

## 4.3 Inherent limitations of naive potential fields

The notes explicitly list the classical problems:
- local minima,
- failure to pass through narrow gaps,
- oscillation in clutter or narrow passages.

This is a central maturity point:

> potential fields are not bad,
> but naive potential fields are not enough.

## 4.4 Navigation functions

Navigation functions are a more rigorous class of potentials designed to avoid unwanted local minima under strong assumptions.

They are constructed to be:
- smooth,
- polar (single minimum at the goal),
- admissible (maximal at the boundary / obstacles),
- Morse (nondegenerate critical points).

The Rimon-Koditschek construction is the flagship theory here.

The real idea is beautiful:
- design a scalar field whose gradient flow almost always reaches the goal,
- and whose geometry respects the free space.

## 4.5 Sphere worlds, star worlds, forests of stars

This is one of the most mathematically rich parts of the course.

### Sphere worlds
Obstacle sets are discs / spheres in a structured workspace.

### Star worlds
Obstacles are star-shaped sets.

### Forests of stars
Even more general obstacle unions handled through structured transformations.

The key move is to use diffeomorphisms that map complicated spaces into simpler ones where navigation functions are easier to construct.

That is where algebraic topology and smooth geometry quietly enter robotics control.

## 4.6 Switching and hybrid potential-field controllers

When a single potential is not enough, switching logic can be used.

This brings hybrid-systems flavor:
- multiple modes,
- piecewise-defined potentials or controllers,
- switching to maintain progress or avoid local failure.

This is another sign that practical navigation often needs more structure than one smooth field.

## 4.7 Coverage with potential fields

Potential fields are not only for one robot reaching one goal.
They also support distributed deployment and coverage.

In the Howard-Mataric-Sukhatme style model:
- robots repel each other,
- robots repel obstacles,
- damping dissipates energy,
- and the network spreads through the environment toward a static equilibrium that improves coverage.

A very important insight here is the energy argument:
- total energy decreases over time due to damping,
- kinetic energy vanishes asymptotically,
- and the system approaches an equilibrium deployment.

## 4.8 Constrained coverage and connectivity maintenance

Coverage alone can spread robots too aggressively and break network structure.
So constrained coverage introduces additional inter-robot attraction or degree constraints.

That creates the engineering trade-off:
- maximize area coverage,
- while preserving communication connectivity or minimum node degree.

This is a recurring theme in multi-robot systems:

> geometry and information constraints fight each other.

## 4.9 Shape generation with implicit functions and RBFs

Another elegant branch of the module uses implicit functions to define desired curves or shapes.
Robots descend toward the zero level set while interaction terms maintain spacing or avoid collisions.

Radial basis functions are used to interpolate smooth implicit functions from boundary constraint points.

This allows decentralized shape generation with local sensing, scalable to team size.

## 4.10 Circulation around a boundary

Potential-style methods also generate limit behaviors like converging to a boundary and then circulating along it.

This is a nice example of composing:
- normal-to-boundary attraction,
- tangent-direction motion,
- collision-avoidance modulation.

So the field is not only descent.
It can encode structured motion on manifolds and boundaries.

## 4.11 Robotics meaning

Potential fields are one of the clearest examples of the course philosophy:
- local vector fields,
- no full centralized plan,
- geometry encoded in a scalar function,
- collective behavior emerging from local descent or interaction.

They are simple enough to implement,
rich enough to require topology,
and practical enough to matter in real multi-robot navigation.

---

# Module 5: Controller Design Using Optimization Methods

## 5.1 Why optimization enters

At some point, heuristic local rules are not enough.
We want to design controllers that are optimal with respect to:
- convergence rate,
- traffic or communication cost,
- connectivity margin,
- energy use,
- safety constraints,
- or control effort.

This is where convex optimization, semidefinite programming, and barrier-constrained control arrive.

## 5.2 Convexity basics

The Nov. 3 and Nov. 5 lectures revisit fundamentals:
- convex sets,
- convex functions,
- Jensen's inequality,
- first- and second-order characterizations,
- proper cones,
- linear, quadratic, conic, and semidefinite programs.

Why is this worth spending time on?
Because in convex optimization:
- local optimum equals global optimum,
- strong algorithms exist,
- and controller synthesis can become computationally tractable.

## 5.3 Fast stochastic swarm allocation

A beautiful application is optimizing the switching-rate matrix $K$ in a stochastic allocation model.

The objective is often to maximize convergence speed to a desired allocation while constraining traffic or idle motion.

Because convergence rate is tied to spectral properties of the allocation dynamics, eigenvalue-based objectives appear.
This links optimization directly back to mean-field models and algebraic graph theory.

## 5.4 Fastest mixing / spectral objectives

The optimization lecture explicitly echoes fastest-mixing Markov chain logic.
That is a very important bridge.

The idea is:
- choose transition structure to move population efficiently,
- while respecting graph constraints and traffic limits,
- and shape the second eigenvalue or related spectral quantity to accelerate convergence.

This is a theme worth remembering:

> if the collective dynamics are linear enough, eigenvalues become design targets.

## 5.5 Connectivity control via algebraic connectivity

For communication networks,

$$
\lambda_2(L)
$$

again becomes the hero.

If positive, the network is connected.
If larger, the network is more robust and supports faster information propagation.

So optimization-based connectivity control often tries to maximize algebraic connectivity subject to motion constraints.

This is a deeply robotics-flavored optimization problem:
- move robots in space,
- so that graph structure stays useful.

## 5.6 Epigraph and SDP formulations

Some of these design problems are transformed into equivalent convex or semidefinite forms using:
- epigraph reformulations,
- linear matrix inequalities,
- variational eigenvalue characterizations.

That matters because the final controller may look geometric, but the synthesis lives in convex analysis.

## 5.7 Barrier certificates and control barrier functions

This is one of the most important safety modules.

### Barrier certificate
A scalar function whose inequalities certify that trajectories cannot enter an unsafe set.

### Control barrier function (CBF)
The input-enabled extension used to enforce safety through control design.

For a safe set

$$
\mathcal{C} = \{x : h(x) \ge 0\},
$$

a standard CBF condition is:

$$
\dot h(x,u) \ge -\alpha(h(x)).
$$

Interpretation:
- if you are near the boundary of safety, the controller must act so the system does not cross out of the safe set.

## 5.8 Safety-critical control as optimization

A common pattern is:
- start with a nominal control law that does the task,
- then solve a quadratic program that keeps the actual control as close as possible to nominal,
- while enforcing barrier constraints.

This is a major modern-control pattern:

> do what you wanted, unless safety says no.

## 5.9 Multi-robot collision avoidance with CBFs

The course includes centralized and decentralized formulations.

### Centralized
One unit computes safe controls for all robots.

### Decentralized
Each robot solves its own local problem with assumptions about other robots.

The decentralized version is more scalable, but typically more conservative and more deadlock-prone.

## 5.10 Deadlocks and realism

The lectures explicitly note that CBFs can suffer from deadlock, especially in symmetric decentralized situations.
They may also be conservative and sensitive to model mismatch, wheel slip, latency, and differential-drive details.

Again the course refuses to let theory become fantasy.
Safety guarantees are powerful, but they do not remove implementation difficulty.

## 5.11 Long-duration autonomy and robot ecology

A very nice broader perspective appears in the robot ecology example:
- robots cover while avoiding obstacles,
- but must also preserve battery and long-duration feasibility.

That expands safety beyond collision avoidance into resource constraints and viability.

## 5.12 Robotics meaning

This module teaches one deep engineering lesson:

> collective behavior is not enough.
> You also need **guarantees**.

Optimization and barrier methods are the bridge from elegant swarm behavior to deployable autonomy under constraints.

---

# Module 6: Geometric Representations and Coverage Control

## 6.1 Why geometry matters here

If graph models capture who talks to whom,
geometric representations capture how robots occupy space together.

This module is about deployment, spatial partitioning, and shape.

The central mathematical objects are:
- Voronoi partitions,
- Delaunay and proximity graphs,
- locational cost functions,
- centroidal configurations,
- and low-dimensional shape abstractions.

## 6.2 Coverage as locational optimization

Coverage control asks:

> where should robots position themselves to optimize sensing performance over a domain?

A standard formulation uses a density function $\phi(q)$ describing task importance over space and a performance function penalizing sensing from far away.

The domain is partitioned into Voronoi cells,
and each robot is responsible for its own cell.

This is a gorgeous idea because it converts a global deployment problem into local geometry.

## 6.3 Voronoi partitions and Delaunay graphs

A Voronoi cell of robot $i$ is the set of points in the domain closer to robot $i$ than to any other robot.

The Delaunay graph is the adjacency graph dual to the Voronoi diagram.

These objects matter because:
- they localize computation,
- they define neighborhoods,
- they support distributed coverage updates,
- and they connect geometry directly to communication structure.

## 6.4 Lloyd algorithms

The classic coverage-control result is the Lloyd algorithm family.
The high-level idea is simple:

1. compute each robot's Voronoi region,
2. compute the centroid of that region under the importance density,
3. move the robot toward that centroid,
4. repeat.

Equilibria are centroidal Voronoi configurations.

These algorithms are powerful because they are:
- distributed,
- asynchronous,
- adaptive,
- and provably descent-based with respect to the coverage objective.

## 6.5 Discrete-time coverage with sensor feedback

A practical twist appears when robots do not know the density function exactly and must estimate it from measurements.

Then centroid computation becomes approximate and sensor-driven.
That is a more realistic robotics version of coverage control.

The hardware implementation notes are valuable here:
- finite communication range,
- approximate local Voronoi regions,
- heuristic truncation near boundaries,
- imperfect gradient information,
- and position-control implementation rather than idealized velocity control.

That is exactly how geometric algorithms survive contact with real robots.

## 6.6 Equal-mass partitioning and construction

Voronoi-style ideas also appear in decentralized construction.
The objective is not just spatial coverage, but workload partitioning.

Each robot should be assigned a region or set of tasks so that the mass / workload is balanced.

This connects locational optimization to construction planning.

## 6.7 Shape abstractions of collectives

The Belta-Kumar style abstraction is one of the most elegant theoretical ideas in the course.

Map the full high-dimensional configuration of many robots into a low-dimensional manifold that captures:
- collective position,
- collective orientation,
- collective shape.

Examples include:
- spanning rectangle,
- concentration ellipsoid.

The point is profound:

> sometimes you do not need to control every robot individually.
> sometimes it is enough to control the collective's abstract shape variables.

## 6.8 Spanning rectangle versus concentration ellipsoid

These are two different shape summaries.

### Spanning rectangle
- rigorous bound on occupied region,
- does not assume distributional form,
- can become conservative for large swarms.

### Concentration ellipsoid
- tied to mean and covariance,
- more compact and distribution-aware,
- but not a strict occupancy bound.

This is a beautiful example of abstraction trade-offs in collective control.

## 6.9 Broadcast and abstraction-based control

Some architectures use low-dimensional abstract states broadcast to all robots.
This allows control laws whose dimension does not scale with team size.

That is a central swarm-control theme:
- huge physical system,
- small control interface.

## 6.10 Robotics meaning

This module shows that multi-robot coordination is not only about communication graphs or stochastic rates.
It is also about **how a collective occupies space**.

Coverage, sensing, deployment, and group shape are all geometric control problems.

---

# Module 7: Strategies for Estimation, Mapping, and Exploration

## 7.1 Why mapping is different in multi-robot settings

Multi-robot mapping is not just one robot building one map.
It adds:
- distributed sensing,
- distributed uncertainty,
- communication constraints,
- data fusion among robots,
- and exploration policies that decide who goes where.

This makes mapping both an estimation problem and a coordination problem.

## 7.2 Bayes rule and probabilistic inference

The mapping lectures begin in exactly the right place: Bayes rule.

Given unknown quantity $x$ and data $d$,

$$
p(x \mid d) \propto p(d \mid x) p(x).
$$

In words:
- likelihood says how compatible the data is with a hypothesis,
- prior says what we believed before,
- posterior is the updated belief.

This is the archetype of probabilistic mapping.

## 7.3 Bayes filter

When data arrives over time, the state estimate becomes recursive.
A generic Bayes filter computes:

$$
p(x_t \mid z_{1:t}, u_{1:t})
$$

through repeated prediction and correction.

That is the core engine behind many mapping and localization algorithms.

## 7.4 Kalman filter viewpoint

When motion and measurement models are linear with Gaussian noise, the Bayes filter collapses into the Kalman filter family.

This gives a compact parametric belief representation through mean and covariance.

The lecture notes remind you that these assumptions are restrictive but very useful.

## 7.5 Occupancy grid maps

Occupancy grids are one of the workhorse map representations.

The environment is discretized into cells,
and each cell has a probability of occupancy.

For static mapping with known poses, the binary Bayes filter is often written in log-odds form because additive updates are convenient:

$$
\ell_t(m) = \ell_{t-1}(m) + \log \frac{p(m \mid z_t, s_t)}{1-p(m \mid z_t, s_t)} - \ell_0(m).
$$

Interpretation:
- measurements push cells toward occupied or free,
- repeated evidence accumulates over time.

## 7.6 Sensor models

Two different ideas matter:

### Forward sensor model
Probability of obtaining a measurement given map and pose.

### Inverse sensor model
Probability a cell is occupied given a measurement and pose.

Practical occupancy mapping often uses heuristic inverse sensor models because they are easier to implement distributively.

This is a good example of robotics honesty:
- the exact probabilistic route may be elegant,
- the approximate route may be what actually runs.

## 7.7 Exploration strategies

Robots must move to gain information.
So mapping naturally connects to exploration.

Strategies include:
- random exploration,
- guided exploration,
- information-based exploration.

For swarms, random and Levy-walk-based approaches are especially attractive because they avoid centralized planning and heavy localization assumptions.

## 7.8 Information-correlated Levy walk

A particularly nice hybrid idea appears in the notes:
- move with Levy-walk-like dispersion,
- but bias the heading toward information gain.

This is a very good example of how stochastic swarm behavior and mapping objectives can be fused.

## 7.9 Topological mapping

Not every environment admits accurate metric mapping, especially with small, resource-constrained robots.

Then topological mapping becomes attractive:
- describe connectivity,
- holes,
- tunnels,
- and qualitative structure,
without relying on detailed metric geometry.

This is where algebraic topology and topological data analysis enter.

The lesson is important:

> sometimes the right map is not a precise geometric picture,
> but a structural description of connectivity.

## 7.10 Random finite sets and PHD filters

For multi-target tracking, the course moves to random finite set ideas.

A set of targets is random in both:
- how many targets exist,
- and where they are.

The probability hypothesis density (PHD) filter tracks the first moment of that random finite set.
Its density has the special property that integrating over a region yields expected target count in that region.

The Gaussian-mixture PHD (GM-PHD) approximation gives a tractable decentralized multi-target search-and-tracking framework.

This is advanced material, but the big idea is simple:

> track not one target state vector,
> but a random spatial intensity over many possible targets.

## 7.11 Multi-robot meaning

The mapping module teaches another deep lesson:

> collective intelligence is not just about moving together.
> It is also about building and sharing belief.

Swarm estimation, distributed maps, and target tracking all extend the coordination problem from motion into uncertainty.

---

# Module 8: Control Strategies for Object Manipulation and Collective Transport

## 8.1 Why manipulation is a different regime

Navigation and coverage are hard.
Manipulation is harder because the collective is now coupled through a payload.

Robots are no longer merely coordinating motion with each other.
They are jointly applying forces to a shared object.

That introduces:
- object dynamics,
- attachment geometry,
- contact constraints,
- force distribution,
- and more severe failure modes.

## 8.2 Classification of transport strategies

The course classifies collective transport along several axes:
- local only versus global knowledge,
- homogeneous versus heterogeneous teams,
- leader-follower versus fully decentralized,
- obstacle-free versus cluttered environments,
- pushing / caging / towing / grasping,
- fixed versus variable team size.

This is very helpful because "collective transport" is not one problem.
It is a family of problems.

## 8.3 Manipulation techniques

### Pushing
Robots impart forces without necessarily enclosing the object.

### Caging
Robots trap the object inside a bounded region of configuration space.

### Towing
Robots pull via attachments.

### Grasping
Robots maintain structured contacts to control the object more directly.

Each technique implies a different control and sensing burden.

## 8.4 Force closure, form closure, object closure

This trio is conceptually important.

### Force closure
Contacts can resist arbitrary external wrench disturbances.
Usually friction matters.

### Form closure
Motion is prevented by geometric constraints even without friction.

### Object closure / caging
The object is trapped in a bounded region though not necessarily under continuous contact.

These are different levels of control authority over the object.

## 8.5 Vector-field composition and caging

The course includes manipulation-by-caging ideas where robots compose approach, surround, and push vector fields.

This is nice because it extends the vector-field and potential-field worldview into manipulation.

## 8.6 Dynamical models of payload transport

A payload introduces real coupled dynamics:
- mass,
- moment of inertia,
- contact forces,
- friction,
- and geometry of attachment points.

This is where collective transport begins to look like networked control plus multi-body dynamics.

## 8.7 Leader-follower manipulation without communication

A striking result in the notes is that robots can coordinate manipulation with no direct communication by exploiting local measurements of payload motion.

The payload itself becomes an implicit communication medium.

That is a very swarm-like principle:
- interaction through the environment,
- not necessarily through explicit messaging.

## 8.8 Force consensus and Barbalat-style analysis

The leader-follower force-coordination results rely on consensus-style force dynamics and use nonlinear analysis tools such as Barbalat's lemma to prove asymptotic convergence.

This is an important bridge:
- graph-consensus thinking survives,
- but now inside a mechanical manipulation problem.

## 8.9 Leaderless manipulation and pseudoinverse flavor

Another line of work in the notes uses local Jacobians and Moore-Penrose pseudoinverse ideas to distribute control actions among robots without explicit centralized coordination.

That is beautiful because it echoes manipulator control:
- payload control objective,
- decentralized force allocation,
- local Jacobians,
- pseudoinverse logic.

So multi-robot manipulation secretly loops back to robot control fundamentals.

## 8.10 Energy efficiency and coordination quality

Experimental metrics include:
- smoothness of payload velocity,
- path efficiency,
- agreement of applied forces,
- robustness to distribution of robots around the payload.

This is a useful reminder that "success" is not binary.
Better coordination often means smoother, more efficient transport.

## 8.11 Open problems

The later lectures are refreshingly honest:
most collective transport methods still assume at least one of:
- predefined trajectories,
- explicit communication,
- known payload properties,
- known robot distribution around payload,
- known environment.

Real open problems involve:
- unknown obstacles,
- GPS-denied environments,
- limited onboard resources,
- aerial cooperative manipulation,
- collision avoidance without getting trapped,
- robust decentralized control in uncertain environments.

This is where current research actually lives.

## 8.12 Construction and stigmergy

The final material broadens manipulation toward collective construction.

A major biological idea appears here: **stigmergy** - coordination through modifications of the environment.

That is important because it gives a third coordination channel beyond:
- explicit communication,
- and direct physical contact.

The environment itself can store information and guide later actions.

This is one of the most beautiful ideas in collective robotics.

---

# The Full Concept Chain

This course is easiest to remember as a logical ladder.

## Ladder

1. Start with **many robots, limited information, and a collective task**.
2. Choose a modeling level: **microscopic, mesoscopic, or macroscopic**.
3. If behavior is population-like, use **Markov / CRN / mean-field models**.
4. If behavior is interaction-network-like, use **graphs and Laplacians**.
5. Use graph dynamics for **consensus, flocking, reference tracking, synchronization**.
6. Use scalar fields for **navigation, coverage, and pattern formation**.
7. Repair naive potential fields with **navigation functions, switching, and topology-aware constructions**.
8. Add guarantees with **convex optimization, SDP, LMIs, and barrier methods**.
9. Use **Voronoi partitions and locational optimization** for deployment and coverage.
10. Use **Bayes filters, occupancy grids, topology, and PHD filters** for collective estimation and mapping.
11. Use **force coordination, closure concepts, and payload dynamics** for collective manipulation.
12. Finally, translate elegant theory into hardware reality with sensing limits, actuation limits, motion constraints, and implementation safety.

That is the course arc.

---

# Robotics Connection Sheet

## A. Warehouse and logistics swarms

- stochastic allocation among sites and tasks,
- mean-field redistribution models,
- graph-based coordination,
- traffic-aware optimization.

## B. Drone swarms

- consensus and formation control,
- algebraic connectivity for communication robustness,
- exploration under GPS-denied conditions,
- collision avoidance with barrier methods,
- collective aerial manipulation as an emerging frontier.

## C. Environmental monitoring and coverage

- potential-field deployment,
- Voronoi-based coverage,
- density-weighted sensing,
- long-duration autonomy with safety/resource constraints.

## D. Swarm mapping in hostile or communication-poor domains

- Levy-walk exploration,
- occupancy-grid fusion,
- time-varying graph consensus,
- topological mapping when metric mapping is too expensive.

## E. Collective transport and construction

- force / form / object closure,
- decentralized manipulation,
- payload dynamics,
- stigmergic coordination,
- adaptive team size and uncertain payloads.

## F. Real hardware and testbeds

- Robotarium-style implementation issues,
- unicycle / differential-drive constraints,
- actuator saturation,
- sensing limitations,
- minimally invasive safety layers,
- gap between theorem and deployment.

---

# What to Never Forget

01. **Multi-robot systems need multiple modeling levels.**
> Microscopic, mesoscopic, and macroscopic views are all valid and useful.

02. **The right abstraction depends on the task.**
> Individual trajectories matter for some problems; population fractions matter for others.

03. **Mean-field control is distribution control.**
> It designs how fractions of robots evolve, not necessarily where each robot goes.

04. **Graph Laplacians are central.**

> $\dot x = -Lx$ is one of the foundational equations of the subject.

05. **Connectivity and convergence are spectral.**

> $\lambda_2(L)$ measures algebraic connectivity for undirected graphs and governs consensus speed.

06. **Naive potential fields are elegant but flawed.**
> Local minima and oscillations are structural issues.

07. **Navigation functions are the rigorous cure, under strong assumptions.**
> They are not the same thing as generic artificial potential fields.

08. **Coverage control is geometry plus optimization.**
> Voronoi partitions and Lloyd algorithms are not side facts - they are core.

09. **Safety can be enforced by optimization.**
> Control barrier functions let you modify nominal control minimally while preserving invariance of a safe set.

10. **Mapping is probabilistic inference.**
> Bayes filters, Kalman filters, and occupancy grids are all ways of updating belief.

11. **Random exploration is not primitive. It is strategic.**
> Brownian and Levy-style motion can be exactly the right abstraction for simple swarms.

12. **Collective manipulation is not just many robots pushing together.**
> It involves closure, contact geometry, payload dynamics, force distribution, and decentralized coordination.

13. **Implementation assumptions matter.**
> Theoretical single-integrator laws do not directly equal real differential-drive behavior.

14. **Local rules can produce global behavior.**
> That is the whole point of the course.

15. **But global behavior still needs proof.**
> That is why Lyapunov analysis, spectra, convexity, and barrier methods keep appearing.

---

# Intuition Anchors by Topic

### Microscopic model
> A movie of individual robots.

### Mesoscopic model
> A probability law over swarm population states.

### Macroscopic model
> A fluid-like description of the collective.

### Mean-field control
> Steer the crowd, not the individual.

### Consensus
> Distributed disagreement removal.

### Laplacian
> The matrix that punishes differences across edges.

### Algebraic connectivity
> How well the network can hold itself together informationally.

### Flocking
> Consensus with geometry and spacing.

### Potential field
> Motion as descent in an energy landscape.

### Navigation function
> A potential field with actual mathematical discipline.

### Barrier function
> A wall in state space that trajectories are not allowed to cross.

### Voronoi partition
> Each robot's natural territory.

### Lloyd algorithm
> Move toward the center of responsibility.

### Occupancy grid
> A map made of local binary beliefs.

### PHD filter
> A density over possible targets, not just one target state.

### Collective transport
> A network of robots turning into a temporary distributed manipulator.

### Stigmergy
> The environment remembers what the robots did.

---

# 30-Years-Later Refresher

If future-me forgot almost everything, remember this skeleton:

1. Multi-robot systems can be modeled as **networks**, **populations**, or **geometric collectives**.
2. Large swarms often use **stochastic** and **mean-field** models.
3. Information exchange and agreement live on **graphs**.
4. **Laplacian dynamics** explain consensus and much of flocking.
5. **Potential fields** encode local navigation and deployment behaviors.
6. **Navigation functions** are the rigorous version that avoids many bad minima under structured assumptions.
7. **Convex optimization** and **SDPs** design better collective controllers.
8. **Control barrier functions** enforce safety while preserving nominal behavior as much as possible.
9. **Voronoi partitions** and **Lloyd algorithms** are the geometry engine of coverage control.
10. **Bayes filters**, **occupancy grids**, and **PHD filters** are the uncertainty engine of mapping and tracking.
11. **Collective transport** requires force coordination and payload dynamics, not just motion agreement.
12. Real robots break assumptions, so implementation matters.

If this skeleton is alive, the rest can be rebuilt.

---

# Final Robotics Takeaway

This course teaches one deep lesson:

A robot collective is not just a bigger robot.

It is:
- a network,
- a population,
- a geometric structure,
- a distributed estimator,
- a constrained optimization problem,
- and sometimes a temporary mechanical organism.

The real jump from beginner to practitioner happens when you stop seeing multi-robot systems as a bag of separate topics - consensus here, swarms there, mapping somewhere else - and start seeing them as different mathematical views of the same question:

> How do local interactions and limited information create reliable global intelligence?

That is when multi-robot systems stops being a course and starts becoming systems instinct.

---

# Minimal Core Formula Sheet

## Point-mass robot models

$$
\dot r_i = u_i
$$

$$
\dot r_i = v_i, \qquad \dot v_i = u_i
$$

## Graph objects

$$
L = D - A
$$

$$
L\mathbf{1} = 0
$$

## Consensus

$$
\dot x_i = \sum_{j \in \mathcal{N}_i} a_{ij}(x_j - x_i)
$$

$$
\dot x = -Lx
$$

## Algebraic connectivity

$$
\lambda_2(L) > 0 \iff \text{graph connected (undirected case)}
$$

## Mean-field conservation

$$
\sum_{i=1}^{S} x_i(t) = 1
$$

## Generic mean-field population model

$$
\dot x = f(x)
$$

## Potential-field control

$$
\dot q = -K \nabla \phi(q)
$$

## CBF safe set

$$
\mathcal{C} = \{x : h(x) \ge 0\}
$$

## CBF condition

$$
\dot h(x,u) \ge -\alpha(h(x))
$$

## Coverage control idea

$$
p_i \leftarrow c_{V_i}
$$

move robot $i$ toward the centroid of its Voronoi region $V_i$.

## Bayes rule

$$
p(x \mid d) \propto p(d \mid x)p(x)
$$

## Bayes filter

$$
p(x_t \mid z_{1:t}, u_{1:t})
$$

## Occupancy log-odds

$$
\ell_t(m) = \log \frac{p(m \mid z_{1:t}, s_{1:t})}{1-p(m \mid z_{1:t}, s_{1:t})}
$$

## PHD interpretation

$$
\int_R v(x)\,dx = \text{expected number of targets in region } R
$$

---

# End Note

This is not just "swarm robotics."

It is distributed control, collective decision-making, geometry, probability, optimization, and mechanics learning to cooperate.

---