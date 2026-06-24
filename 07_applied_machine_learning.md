# **Applied Machine Learning for Mechanical and Aerospace Engineering**

### **From Statistical Learning Basics to Data-Driven Dynamics, Estimation, and Control**

**Focus:** build machine learning from first principles, keep the mathematics honest, and carry it all the way into the forms that actually matter for robotics, autonomy, and engineering systems.

---

# Why this course matters in robotics and engineering

Applied machine learning is not just "fit a model and report accuracy."

In an engineering setting, it is about learning useful structure from data when:
- first-principles models are incomplete,
- sensing is noisy,
- systems are high-dimensional,
- exact inference is too expensive,
- and decisions still have to be made.

That makes this subject deeply relevant to robotics.

You see it in:
- **regression** for calibration, identification, and surrogate modeling,
- **classification** for perception and fault detection,
- **clustering** for regime discovery and anomaly structure,
- **dimensionality reduction** for state compression and visualization,
- **Gaussian processes** for uncertainty-aware prediction,
- **neural networks** for high-capacity function approximation,
- **sequential probabilistic models** for tracking and estimation,
- **data-driven dynamics** for learning reduced models from trajectories,
- **machine-learning control** where prediction, estimation, and control merge.

This course matters because it connects several worlds that are too often taught separately:
- statistical learning,
- optimization,
- probabilistic modeling,
- numerical linear algebra,
- deep learning,
- dynamical systems,
- and control.

The real lesson is that machine learning is not a bag of black-box tricks.
It is structured approximation under uncertainty.

And in robotics, that is basically daily life.

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

1. what machine learning is and how problems are framed,
2. probability, statistics, and inference,
3. regression,
4. classification,
5. model selection and generalization,
6. kernels and Gaussian processes,
7. clustering, mixtures, EM, and latent structure,
8. neural networks and deep learning,
9. sequential and graphical models,
10. data-driven dynamics and control,
11. robotics applications and deployment logic.

## Mode 3: Deep rebuild

Read everything and mentally connect:
- data,
- loss,
- likelihood,
- optimization,
- generalization,
- uncertainty,
- latent structure,
- nonlinear approximation,
- dynamics,
- estimation,
- and control.

# Big Picture Map

At the highest level, this course answers twelve giant questions.

## 1. What does it mean to learn from data?

> Choose a model class, define an objective, fit parameters, and hope the learned structure generalizes beyond the training set.

## 2. What kinds of learning problems exist?

> Supervised, unsupervised, probabilistic, sequential, and control-oriented learning.

## 3. Why do probability and statistics sit underneath machine learning?

Because noise, uncertainty, missing information, and finite data are not edge cases. They are the normal case.

## 4. What is the simplest useful learning problem?

> Regression: predict a continuous output from inputs.

## 5. What changes when the output is discrete?

> Then the problem becomes classification, decision theory, and calibrated probability estimation.

## 6. Why is fitting the training set not enough?

Because generalization matters more than memorization.

## 7. How do we handle nonlinear structure without hand-designing everything?

> With basis functions, kernels, Gaussian processes, and later neural networks.

## 8. How do we discover structure when labels are absent?

> With clustering, mixture models, dimensionality reduction, and latent-variable thinking.

## 9. Why do deep neural networks matter?

Because they learn layered nonlinear representations that scale to high-dimensional data.

## 10. What changes when data evolves over time?

> Then independence assumptions break, and sequential models, filtering, hidden states, and dynamical systems enter.

## 11. What does machine learning look like for physical systems?

> It becomes system identification, reduced-order modeling, Koopman embeddings, sparse equation discovery, and learning-assisted control.

## 12. What is the robotics endpoint of all this?

> Learn representations, predict uncertain outcomes, estimate state, identify dynamics, and support control decisions in the real world.

# Notation and foundational objects

We work with data, models, parameters, losses, and probability distributions.

**Input / feature vector:**

$$
x \in \mathbb{R}^{d}
$$

**Target / label:**

$$
y
$$

which may be continuous (regression) or discrete (classification).

**Dataset:**

$$
\mathcal{D} = \{(x_i, y_i)\}_{i=1}^{N}
$$

**Model prediction:**

$$
\hat{y} = f_{\theta}(x)
$$

**Parameter vector:**

$$
\theta
$$

**Loss function:**

$$
\mathcal{L}(\theta)
$$

**Empirical risk:**

$$
\frac{1}{N}\sum_{i=1}^{N} \ell(f_{\theta}(x_i), y_i)
$$

**Random variable:**

$$
X
$$

**Probability density / mass:**

$$
p(x)
$$

**Posterior by Bayes' rule:**

$$
p(\theta \mid \mathcal{D}) = \frac{p(\mathcal{D} \mid \theta)p(\theta)}{p(\mathcal{D})}
$$

**State for dynamical systems:**

$$
x_k \quad \text{or} \quad x(t)
$$

**Linear state-space system:**

$$
\dot{x} = Ax + Bu, \qquad y = Cx + Du
$$

**Data matrix:**

$$
X = [x_1\; x_2\; \cdots\; x_m]
$$

or its row-wise equivalent depending on convention.

---

# Module 1: What Machine Learning Actually Is

## 1.1 The core problem

Machine learning is the problem of using data to infer a rule that performs a task well on unseen examples.

That sentence sounds small, but it contains almost the whole field.

You need:
- data,
- a task,
- a model family,
- a loss function,
- an optimization method,
- and a way to judge whether the result generalizes.

## 1.2 The three most important objects

For almost every ML method, three objects define the game:

1. **Representation** - how inputs are encoded.
2. **Objective** - what is being optimized.
3. **Inductive bias** - what kinds of solutions the method naturally prefers.

A beginner often focuses only on the optimizer.
A mature engineer asks:
- what assumptions are baked into the model,
- what structure is being imposed,
- and what failure modes that structure creates.

## 1.3 Supervised learning

In supervised learning, the dataset contains inputs and desired outputs.

Examples:
- image -> class label,
- sensor vector -> force estimate,
- trajectory features -> predicted drag,
- system state -> next-state value.

The goal is to learn a mapping

$$
f_{\theta}: x \mapsto y
$$

that minimizes prediction error on unseen data.

### Regression
Continuous output.

### Classification
Discrete output.

That split sounds trivial, but it changes the geometry of the loss, the meaning of uncertainty, and the evaluation metrics.

## 1.4 Unsupervised learning

Here the data arrives without explicit labels.

You are not told what the correct answer is.
Instead, you try to discover structure such as:
- clusters,
- latent coordinates,
- manifolds,
- modes,
- dependencies,
- anomalies,
- or lower-dimensional summaries.

Unsupervised learning matters enormously in robotics because the world does not come pre-labeled.

## 1.5 Probabilistic learning

Probabilistic learning asks not only:
- what is the best prediction,

but also:
- how uncertain am I,
- what latent variables might explain the data,
- and what distribution might have generated the observation.

That is where Bayes, likelihoods, posteriors, and graphical models enter.

## 1.6 Learning as optimization

Most ML problems are framed as minimizing an objective of the form

$$
\theta^{*} = \arg\min_{\theta} \mathcal{L}(\theta)
$$

or maximizing likelihood / posterior probability.

So despite the branding complexity, a large part of ML is optimization plus modeling.

## 1.7 Training, validation, and test sets

A concept so basic that people still mess it up:

- **training set**: fit parameters,
- **validation set**: choose model class / hyperparameters,
- **test set**: estimate final generalization.

If information leaks from test into training, the performance estimate is contaminated.

That is not a paperwork issue.
That is a scientific validity issue.

## 1.8 Why this matters in engineering

In engineering, models are not judged only by benchmark score.
They are judged by:
- reliability,
- interpretability,
- robustness,
- uncertainty behavior,
- and whether they fail gracefully.

A model that wins a leaderboard but destabilizes a robot is not a success.

# Module 2: Probability, Statistics, and the Uncertainty Backbone

## 2.1 Why probability is unavoidable

Data is noisy.
Sensors are noisy.
Labels are noisy.
The world is partially observed.
Finite samples distort everything.

So probability is not optional decoration.
It is the language of uncertainty.

## 2.2 Random variables and distributions

A random variable maps outcomes to numbers.
A distribution tells us how likely values are.

For discrete variables we work with probabilities.
For continuous variables we work with densities.

The probability axioms are simple.
The consequences are not.

## 2.3 Expectation, variance, and covariance

**Expectation** is the average value:

$$
\mathbb{E}[X]
$$

**Variance** measures spread:

$$
\mathrm{Var}(X) = \mathbb{E}[(X - \mathbb{E}[X])^{2}]
$$

**Covariance** measures co-variation:

$$
\mathrm{Cov}(X,Y) = \mathbb{E}[(X-\mathbb{E}[X])(Y-\mathbb{E}[Y])]
$$

These become central for:
- Gaussian models,
- PCA,
- Kalman filtering,
- Gaussian processes,
- and uncertainty propagation.

## 2.4 Bayes' rule

Bayes' rule is the backbone of probabilistic inference:

$$
p(\theta \mid \mathcal{D}) = \frac{p(\mathcal{D} \mid \theta)p(\theta)}{p(\mathcal{D})}
$$

Interpretation:
- prior belief about parameters,
- updated using observed data,
- resulting in a posterior belief.

That is the cleanest formal statement of learning from evidence.

## 2.5 The Gaussian distribution

The Gaussian is everywhere because it is mathematically convenient and often physically reasonable.

Univariate form:

$$
p(x) = \frac{1}{\sqrt{2\pi\sigma^{2}}}\exp\left(-\frac{(x-\mu)^{2}}{2\sigma^{2}}\right)
$$

Multivariate form:

$$
p(x) \propto \exp\left(-\frac{1}{2}(x-\mu)^{T}\Sigma^{-1}(x-\mu)\right)
$$

Why it matters:
- least squares under Gaussian noise,
- Kalman filters,
- Gaussian processes,
- mixture models,
- linear-Gaussian state-space models.

## 2.6 Likelihood, maximum likelihood, and maximum a posteriori

Given a model with parameters $\theta$, the **likelihood** is

$$
p(\mathcal{D} \mid \theta)
$$

Maximum likelihood estimation solves

$$
\theta_{\mathrm{ML}} = \arg\max_{\theta} p(\mathcal{D} \mid \theta)
$$

or equivalently minimizes negative log-likelihood.

Maximum a posteriori estimation adds a prior:

$$
\theta_{\mathrm{MAP}} = \arg\max_{\theta} p(\mathcal{D} \mid \theta)p(\theta)
$$

This is the conceptual bridge between:
- statistics,
- regularization,
- and Bayesian thinking.

## 2.7 Decision theory

Prediction is not the same as decision.

A classifier might output probabilities.
A decision system still has to choose an action.

That action depends on a **loss function**.

In some cases, false positives and false negatives have very different costs.
In robotics, that matters a lot.
Missing an obstacle and hallucinating an obstacle are not the same kind of mistake.

## 2.8 Information theory

Entropy measures uncertainty:

$$
H(X) = -\sum_x p(x)\log p(x)
$$

Relative entropy / KL divergence measures mismatch between two distributions:

$$
D_{\mathrm{KL}}(p \Vert q) = \sum_x p(x)\log\frac{p(x)}{q(x)}
$$

Cross-entropy shows up directly in classification losses.

These quantities matter because learning often becomes:
- compress uncertainty,
- reduce mismatch,
- or minimize surprise.

## 2.9 The curse of dimensionality

As dimension grows:
- volume expands fast,
- density estimation becomes sparse,
- neighborhoods become weird,
- and naive intuition breaks.

This is why:
- nonparametric methods struggle,
- feature engineering matters,
- dimensionality reduction matters,
- and representation learning matters.

## 2.10 Robotics meaning

Probability gives robotics the language for:
- sensor uncertainty,
- state estimation,
- risk-aware decisions,
- tracking,
- map uncertainty,
- and prediction under incomplete information.

Without probability, a robot is just doing geometry with false confidence.

# Module 3: Regression - The First Serious Learning Problem

## 3.1 What regression is

Regression predicts a continuous target from an input.

Examples:
- force from strain gauge readings,
- drag coefficient from operating conditions,
- battery temperature from load history,
- next-state estimate from current state and input,
- actuator wear estimate from telemetry.

## 3.2 Linear regression

The simplest model is

$$
\hat{y} = w^{T}x + b
$$

or with basis expansion

$$
\hat{y} = w^{T}\phi(x)
$$

where $\phi(x)$ may contain nonlinear features even though the model is linear in the parameters.

That distinction matters a lot.

## 3.3 Least squares

For dataset $\{(x_i,y_i)\}$, least squares minimizes

$$
\sum_{i=1}^{N} (y_i - w^{T}x_i)^{2}
$$

In matrix form:

$$
\min_{w} \|Xw - y\|^{2}
$$

The normal equations are

$$
X^{T}Xw = X^{T}y
$$

and if $X^{T}X$ is invertible,

$$
w = (X^{T}X)^{-1}X^{T}y
$$

This is not just a homework formula.
It is one of the central computational structures in all of engineering data fitting.

## 3.4 Geometry of least squares

When $Xw = y$ is not exactly solvable, least squares finds the projection of $y$ onto the column space of $X$.

Residual:

$$
r = y - X\hat{w}
$$

satisfies orthogonality condition

$$
X^{T}r = 0
$$

That geometric fact is more important than the closed-form solution itself.

## 3.5 Regression as maximum likelihood

If observation noise is Gaussian:

$$
y_i = f_{\theta}(x_i) + \epsilon_i, \qquad \epsilon_i \sim \mathcal{N}(0, \sigma^{2})
$$

then maximizing likelihood is equivalent to minimizing squared error.

That is one of the cleanest links between probability and optimization.

## 3.6 Regularization: why plain least squares is not enough

High-capacity models can overfit.
Ill-conditioned design matrices can create unstable parameters.

Two standard fixes:

### Ridge regression

$$
\min_{w} \|Xw-y\|^{2} + \lambda \|w\|^{2}
$$

This shrinks coefficients smoothly.

### Lasso

$$
\min_{w} \|Xw-y\|^{2} + \lambda \|w\|_{1}
$$

This promotes sparsity.

Ridge says "use all features gently."
Lasso says "try to use only the important ones."

## 3.7 Bias-variance trade-off

Expected prediction error decomposes conceptually into:
- noise,
- squared bias,
- variance.

Low-capacity models:
- high bias,
- low variance.

High-capacity models:
- low bias,
- high variance.

Machine learning maturity is largely the skill of managing this trade-off without lying to yourself.

## 3.8 Bayesian linear regression

Instead of treating $w$ as a fixed unknown, treat it as a random variable.

Prior:

$$
p(w)
$$

Likelihood:

$$
p(y \mid X, w)
$$

Posterior:

$$
p(w \mid X, y)
$$

Now prediction becomes a distribution, not a single line.

This gives:
- uncertainty over parameters,
- uncertainty over predictions,
- and automatic complexity control through the prior.

## 3.9 Model comparison and evidence

A bigger model is not always a better model.
You pay for flexibility.

That is why we care about:
- validation error,
- evidence,
- AIC / BIC-like information criteria,
- and cross-validation.

## 3.10 Robotics meaning

Regression is everywhere in robotics:
- calibration curves,
- friction estimation,
- aerodynamic coefficients,
- soft-sensor inference,
- load prediction,
- surrogate modeling for optimization,
- and residual-based fault detection.

The world is full of quantities that are not class labels. They are numbers.
That is regression land.

# Module 4: Classification - Decisions, Boundaries, and Probabilities

## 4.1 What classification is

Classification predicts a discrete label.

Examples:
- obstacle / free space,
- normal / fault,
- graspable / not graspable,
- terrain class,
- object category,
- command intent.

## 4.2 Decision boundaries

A classifier partitions input space into regions.

The simplest linear classifier uses a discriminant function

$$
g(x) = w^{T}x + b
$$

and predicts class by sign or maximum score.

That means classification is geometry:
which side of the boundary are you on?

## 4.3 Generative vs discriminative classification

Two major viewpoints.

### Generative
Model

$$
p(x \mid y) \quad \text{and} \quad p(y)
$$

then use Bayes' rule to infer

$$
p(y \mid x)
$$

### Discriminative
Model

$$
p(y \mid x)
$$

directly, or learn a direct decision boundary.

Generative methods model how the data was produced.
Discriminative methods focus on prediction itself.

Both matter.

## 4.4 Logistic regression

Binary logistic regression models

$$
p(y=1 \mid x) = \sigma(w^{T}x + b)
$$

where

$$
\sigma(z) = \frac{1}{1+e^{-z}}
$$

Training usually minimizes cross-entropy / negative log-likelihood.

This is one of the most important classification models because it is:
- probabilistic,
- interpretable,
- differentiable,
- and a gateway to neural networks.

## 4.5 Softmax for multiclass problems

For classes $k = 1,\dots,K$,

$$
p(y=k \mid x) = \frac{e^{a_k(x)}}{\sum_{j=1}^{K} e^{a_j(x)}}
$$

This turns raw scores into a normalized class distribution.

## 4.6 Cross-entropy loss

For one-hot targets, cross-entropy is

$$
\mathcal{L} = -\sum_{k=1}^{K} y_k \log \hat{y}_k
$$

Interpretation:
- reward high probability on correct classes,
- punish confident wrong predictions heavily.

This loss survives all the way into deep learning.

## 4.7 Fisher discriminant and class separation

Fisher's idea is elegant:
find a projection that makes classes far apart while keeping each class tight.

So classification is not only about fitting labels.
It is also about finding a geometry where separation becomes easier.

## 4.8 Perceptron intuition

The perceptron is historically important because it gives the basic linear-threshold neuron idea.

If data are linearly separable, the perceptron can find a separator.
If not, you need softer losses or richer models.

That failure is historically what pushed the field toward hidden layers and kernels.

## 4.9 Support vector machines

SVMs choose the separating hyperplane with maximum margin.

For linearly separable data, the core idea is:
- do not merely separate,
- separate as robustly as possible.

Soft-margin form:

$$
\min_{w,b,\xi} \frac{1}{2}\|w\|^{2} + C\sum_i \xi_i
$$

subject to classification constraints with slack variables $\xi_i$.

Why margin matters:
- larger margin generally means better robustness,
- only a subset of training points, the support vectors, define the boundary.

## 4.10 Calibration and cost asymmetry

A classifier can be accurate yet badly calibrated.
It can also be good under one loss and terrible under another.

For robotics, cost-sensitive classification matters because:
- missed hazard and false alarm are not equal,
- class imbalance is common,
- operating thresholds often matter more than raw accuracy.

## 4.11 Robotics meaning

Classification powers:
- object recognition,
- terrain labeling,
- fault alarms,
- event detection,
- contact-state recognition,
- anomaly flags,
- and semantic segmentation pipelines.

But in robotics, the key question is often not only "which class?"
It is "with what confidence, under what cost, and should I act on it now?"

# Module 5: Generalization, Model Selection, and Evaluation Discipline

## 5.1 Overfitting and underfitting

A model underfits when it is too simple to capture the structure.
A model overfits when it captures idiosyncrasies of the training data that do not generalize.

That sounds obvious.
In practice it is one of the hardest things to manage honestly.

## 5.2 Capacity

Model capacity is the ability to fit complex functions.

Increase capacity and you gain expressiveness.
You also gain new ways to fail.

## 5.3 Hyperparameters

Hyperparameters are not learned directly by standard training.
Examples:
- regularization weight $\lambda$,
- network depth,
- learning rate,
- kernel bandwidth,
- number of mixture components,
- number of retained PCA modes.

These are chosen through validation, prior knowledge, or more advanced search.

## 5.4 Cross-validation

Cross-validation estimates generalization by repeated train/validation splits.

Why it matters:
- finite data can make one split misleading,
- cross-validation gives a more stable estimate,
- especially useful when data is limited.

## 5.5 Information criteria and parsimony

The Brunton-Kutz engineering view emphasizes the Pareto front and **Lex Parsimoniae**:
prefer models that are simple enough while still performing well.

This is very grown-up engineering.
A slightly worse but interpretable and stable model can be better than a fragile black box.

## 5.6 Performance metrics

Use the metric that matches the task.

Regression:
- RMSE,
- MAE,
- $R^{2}$,
- predictive intervals.

Classification:
- accuracy,
- precision,
- recall,
- F1,
- ROC / PR behavior,
- calibration.

Sequential / control settings:
- rollout error,
- stability,
- cumulative cost,
- constraint violation,
- tracking error.

A bad metric can reward the wrong behavior.

## 5.7 Leakage, shifts, and realism

Common ways people accidentally cheat:
- normalize using whole dataset before splitting,
- tune on test set,
- leak future information into past predictions,
- use IID assumptions on strongly time-correlated data,
- ignore domain shift between lab and field.

Engineering honesty means designing evaluation that matches deployment.

## 5.8 Robotics meaning

For robotics and autonomy, model selection is not just about accuracy.
It is about:
- latency,
- memory,
- stability,
- uncertainty behavior,
- compute budget,
- and safety under distribution shift.

# Module 6: Kernels, Gaussian Processes, and Nonlinear Learning with Structure

## 6.1 Why kernels appear

Linear models are beautiful.
Reality is not always linear.

One fix is explicit nonlinear features.
Another is the **kernel trick**.

## 6.2 Kernel idea

A kernel computes similarity:

$$
k(x,x') = \langle \phi(x), \phi(x') \rangle
$$

without explicitly building $\phi(x)$.

This means we can behave as though we mapped data into a high-dimensional feature space, while computing only pairwise similarities.

That is conceptually elegant and computationally powerful.

## 6.3 Common kernels

- linear kernel,
- polynomial kernel,
- radial basis function (RBF) kernel.

RBF form:

$$
k(x,x') = \exp\left(-\frac{\|x-x'\|^{2}}{2\sigma^{2}}\right)
$$

This says nearby points should behave similarly.

## 6.4 Kernel machines and SVMs

With kernels, SVMs become nonlinear classifiers in the original input space while remaining linear in feature space.

So the decision boundary can be highly flexible without explicitly fitting a huge feature vector.

## 6.5 Gaussian processes

A Gaussian process is a distribution over functions.

Instead of saying "the parameter vector has a prior," you say:

> the function values themselves follow a joint Gaussian structure determined by a kernel.

This is one of the most elegant models in all of ML.

## 6.6 GP regression intuition

Given observed pairs $(x_i, y_i)$, a GP produces:
- a mean prediction,
- and predictive uncertainty.

That uncertainty grows naturally away from data-rich regions.

This makes GPs valuable when uncertainty matters as much as the prediction.

## 6.7 Why GPs are loved

Because they provide:
- nonlinear regression,
- principled uncertainty,
- flexible prior structure through kernels,
- and beautiful Bayesian mathematics.

## 6.8 Why GPs are painful

Because naive training and inference scale poorly with dataset size.

So GPs are conceptually elite, but computationally expensive.

## 6.9 Automatic relevance determination

Some kernels can learn separate length scales per feature dimension.
This gives a kind of built-in feature relevance estimate.

That is useful when some measured variables matter much more than others.

## 6.10 Robotics meaning

Kernels and GPs matter in robotics for:
- terrain modeling,
- Bayesian optimization,
- surrogate modeling for expensive experiments,
- system identification with uncertainty,
- mapping spatial fields,
- and safe learning where confidence matters.

When the robot must know not only a guess but also how shaky that guess is, GP-style thinking becomes very attractive.

# Module 7: Clustering, Mixtures, Latent Variables, and EM

## 7.1 Why unsupervised structure matters

Not all useful structure comes with labels.
Sometimes the right question is:
- what natural regimes exist,
- what hidden modes generate the data,
- and what lower-dimensional structure the observations lie near.

## 7.2 k-means clustering

k-means partitions data into $K$ clusters by minimizing within-cluster squared distance.

Core loop:
1. assign each point to nearest center,
2. recompute centers,
3. repeat.

It is simple, fast, and useful.
It is also limited.

It assumes roughly spherical clusters under Euclidean geometry.

## 7.3 Why k-means is still important

Because it teaches a very deep idea:

> unsupervised learning often alternates between latent assignment and parameter update.

That exact logic reappears in EM.

## 7.4 Mixture models

A mixture model says data comes from several underlying components.

Gaussian mixture model:

$$
p(x) = \sum_{k=1}^{K} \pi_k \, \mathcal{N}(x \mid \mu_k, \Sigma_k)
$$

Interpretation:
- first choose a hidden component $k$,
- then sample from that component distribution.

This is already latent-variable thinking.

## 7.5 Why mixtures are more expressive than k-means

Because they provide:
- soft assignments instead of hard assignments,
- covariance structure instead of only centers,
- probabilistic interpretation,
- and richer cluster geometry.

## 7.6 Expectation-Maximization (EM)

EM is one of the central algorithms in probabilistic ML.

It alternates:

### E-step
Estimate the latent responsibilities.

### M-step
Update parameters using those responsibilities.

Mental model:
- if latent variables were known, fitting would be easier,
- if parameters were known, inferring latent variables would be easier,
- so alternate between the two.

That is EM.

## 7.7 EM beyond mixtures

EM is not just for Gaussian mixtures.
It is a general strategy for latent-variable maximum likelihood.

Once you understand EM, many probabilistic models feel less mysterious.

## 7.8 PCA and dimensionality reduction

Principal component analysis finds directions of maximum variance.

Given centered data matrix $X$, PCA is closely tied to the SVD:

$$
X = U\Sigma V^{T}
$$

The top singular directions define the principal subspace.

PCA is the cleanest example of low-dimensional structure extraction.

## 7.9 Why PCA matters so much

Because high-dimensional engineering data often has strong low-rank structure.

Examples:
- correlated sensors,
- images,
- trajectory bundles,
- deformation fields,
- flow snapshots,
- point-cloud feature embeddings.

## 7.10 Probabilistic PCA and factor analysis

PCA can be reinterpreted probabilistically.
Then dimensionality reduction becomes latent-variable modeling rather than only algebraic compression.

That viewpoint is much more aligned with modern ML.

## 7.11 Robotics meaning

Unsupervised structure matters in robotics for:
- environment regime discovery,
- anomaly clusters,
- latent state compression,
- map summarization,
- perception embeddings,
- motion primitive discovery,
- and reduced-order state representations.

Many real systems do not hand you labels. They hand you structure if you know how to look.

# Module 8: Neural Networks and Deep Learning

## 8.1 Why neural networks matter

Neural networks are universal nonlinear function approximators with trainable hierarchical representations.

That is the formal-sounding version.

The practical version is:
- they can fit very complex functions,
- learn features automatically,
- and scale to images, sequences, and massive datasets.

## 8.2 The basic feedforward network

A layer computes something like

$$
h = \phi(Wx + b)
$$

Stack layers and you get a composition of affine maps and nonlinearities.

That composition is where expressive power comes from.

## 8.3 Why nonlinearity matters

Without nonlinear activations, stacked linear layers collapse to one linear map.

So the whole point of hidden layers is not depth alone.
It is depth plus nonlinearity.

## 8.4 Common activations

- sigmoid,
- tanh,
- ReLU,
- leaky ReLU,
- GELU in more modern contexts.

Different activations change optimization behavior and representational structure.

## 8.5 Backpropagation

Backpropagation is just chain rule applied efficiently through a computational graph.

If the loss is $\mathcal{L}$, training requires gradients such as

$$
\nabla_{W}\mathcal{L}
$$

Backprop computes these layer by layer from output back to input.

This is not magic.
It is disciplined bookkeeping of derivatives.

## 8.6 Gradient descent and SGD

Parameter update in its most basic form:

$$
\theta \leftarrow \theta - \eta \nabla_{\theta}\mathcal{L}
$$

where $\eta$ is the learning rate.

In practice we use mini-batch stochastic gradient descent and variants like Adam.

## 8.7 Optimization is not the whole story

The deep learning text emphasizes a crucial distinction:
training is not pure numerical optimization in a vacuum.

It is optimization under:
- finite data,
- generalization pressure,
- nonconvex objectives,
- noisy gradients,
- and architecture-induced bias.

That is why good training is a systems skill, not just running an optimizer.

## 8.8 Regularization in deep learning

Common tools:
- weight decay,
- early stopping,
- data augmentation,
- dropout,
- parameter sharing,
- noise injection.

These exist because powerful networks can memorize aggressively.

## 8.9 Convolutional neural networks

CNNs exploit spatial locality and parameter sharing.

Why they matter:
- images are not generic vectors,
- nearby pixels interact strongly,
- and the same local pattern can occur in many places.

That is why CNNs dominate vision pipelines.

## 8.10 Recurrent and sequence models

Sequential data needs models that respect temporal structure.

RNN idea:

$$
h_t = f(h_{t-1}, x_t)
$$

Now the network has memory.

Later architectures improve on basic RNN limitations, but the conceptual point remains:
sequence data requires state.

## 8.11 Practical deep learning methodology

The Goodfellow text puts unusual emphasis on practical method, and rightly so.

Important habits:
- establish baseline models,
- watch training and validation curves,
- debug data pipelines first,
- tune learning rate before exotic tricks,
- normalize inputs,
- and never trust a single lucky run.

## 8.12 Robotics meaning

Deep learning matters in robotics for:
- vision,
- sensor fusion,
- dynamics approximation,
- inverse models,
- policy networks,
- sequence prediction,
- and representation learning from raw observations.

But the adult lesson is this:
architecture choice, dataset quality, and evaluation discipline matter as much as raw expressive power.

# Module 9: Structured Probabilistic Models and Sequential Data

## 9.1 Why structure matters

High-dimensional joint distributions are hard to model directly.
Graphical models encode conditional independence structure so the problem becomes tractable.

## 9.2 Bayesian networks

A Bayesian network is a directed graphical model representing factorization of a joint distribution.

It says:
- not everything depends on everything else,
- and we should exploit that.

## 9.3 Markov random fields

MRFs are undirected graphical models.
They are useful when mutual local relationships matter more than causal direction.

They show up naturally in image modeling, spatial structure, and regularization.

## 9.4 Hidden Markov models

An HMM assumes:
- latent state evolves as a Markov chain,
- observation depends on current latent state.

This gives the classic hidden-state sequence model.

That is the first serious probabilistic model for temporal structure many engineers meet.

## 9.5 Linear dynamical systems

A continuous latent state with linear-Gaussian evolution yields state-space models and filtering logic.

Generic form:

$$
x_{k+1} = A x_k + w_k
$$

$$
y_k = C x_k + v_k
$$

This is the probabilistic-estimation side of control theory.

## 9.6 Kalman filtering link

If the model is linear and noise is Gaussian, the Kalman filter gives optimal recursive state estimation.

That is where ML, statistics, and control stop pretending to be separate subjects.

## 9.7 Particle filtering and nonlinear sequential inference

When linear-Gaussian assumptions break, exact inference can break with them.
Then we use approximate sequential inference such as particle filters.

## 9.8 Why this matters

Many robotics problems are not IID.
They are time-correlated, partially observed, and stateful.

Any ML pipeline that ignores this temporal structure may look good frame-by-frame and fail system-level behavior badly.

# Module 10: Numerical Computation and Optimization Reality

## 10.1 The math is not the implementation

Machine learning formulas often assume exact arithmetic and stable linear algebra.
Real implementations face:
- poor conditioning,
- overflow and underflow,
- gradient explosion or vanishing,
- ill-posed inverses,
- and optimizer sensitivity.

## 10.2 Conditioning matters

If a matrix is poorly conditioned, small perturbations in data can cause large changes in the solution.

That matters for:
- least squares,
- regression,
- kernel methods,
- covariance inversion,
- Gaussian process training,
- and Kalman filters.

## 10.3 Why SVD keeps showing up

Because SVD is the adult way to understand and stabilize many matrix problems:
- rank,
- pseudoinverse,
- low-rank approximation,
- denoising,
- latent structure,
- and reduced models.

For any matrix $A$,

$$
A = U \Sigma V^{T}
$$

and that decomposition reveals what directions matter most.

## 10.4 Sparse regression and compressed sensing intuition

Sometimes the right assumption is not smooth shrinkage, but sparsity.

That means:
- only a few features matter,
- only a few modes dominate,
- only a few library terms generate the dynamics.

This becomes decisive in system identification and interpretable model discovery.

## 10.5 Engineering meaning

The numerical layer is where many beautiful ML ideas either survive deployment or die embarrassingly.

# Module 11: Data-Driven Dynamical Systems

## 11.1 Why this module matters so much for robotics

A lot of machine learning education stops at static prediction.
Engineering systems evolve in time.

Robots are dynamical systems.
Aircraft are dynamical systems.
Power systems are dynamical systems.
Fluid flows are dynamical systems.

So applied ML for engineers must eventually confront dynamics.

## 11.2 Snapshot data and state evolution

Suppose you observe states $x_1, x_2, \dots, x_m$ from a trajectory or many trajectories.
The problem becomes:
- what reduced structure explains them,
- what law advances them,
- and what controller can exploit that law.

## 11.3 Dynamic Mode Decomposition (DMD)

DMD tries to identify a best-fit linear operator that advances snapshot data:

$$
X' \approx AX
$$

where $X$ is the data matrix of current snapshots and $X'$ the time-shifted snapshot matrix.

The point is not that every nonlinear system is truly linear.
The point is that useful dominant evolution patterns may still be extracted from data.

## 11.4 Why DMD matters

DMD gives:
- coherent spatio-temporal modes,
- approximate linear evolution structure,
- mode frequencies and growth/decay rates,
- reduced models for prediction and control.

This is extremely attractive in engineering because it is interpretable.

## 11.5 Koopman viewpoint

The Koopman idea says:
- nonlinear state evolution may look linear in a lifted observable space.

That is conceptually profound.

Instead of forcing nonlinearity directly into state space, search for coordinates where evolution becomes approximately linear.

This connects ML representation learning with classical spectral analysis.

## 11.6 Sparse Identification of Nonlinear Dynamics (SINDy)

SINDy assumes the governing dynamics can be expressed sparsely in a library of candidate functions.

Generic idea:

$$
\dot{x} \approx \Theta(x)\Xi
$$

where:
- $\Theta(x)$ is a library of candidate nonlinear terms,
- $\Xi$ is a sparse coefficient matrix.

Interpretation:
- propose many possible terms,
- select only the small subset actually needed.

This is one of the coolest bridges between ML and interpretable physics discovery.

## 11.7 Why SINDy is important

Because it does not only predict.
It tries to discover equations.

That makes it especially attractive for engineering science, where interpretability is often part of the goal.

## 11.8 System identification

System identification asks:
- from input-output or state data, what model best explains the system?

This is one of the oldest and most practical ML-for-control problems.

It may be:
- parametric,
- nonparametric,
- linear,
- nonlinear,
- black-box,
- or reduced-order.

## 11.9 Reduced-order modeling

Many physical systems are huge, but their interesting behavior lives in a smaller subspace.

That is why POD, SVD, balanced truncation, DEIM, and other reduced-order methods matter.

Machine learning enters here as:
- compression,
- mode discovery,
- parameter interpolation,
- nonlinear closure modeling,
- and fast surrogate dynamics.

## 11.10 Robotics meaning

For robotics and autonomy, data-driven dynamics matters in:
- learned system models,
- residual dynamics correction,
- adaptive control,
- model-based reinforcement learning,
- fast simulation surrogates,
- and reduced models for planning or MPC.

# Module 12: Control-Oriented Machine Learning

## 12.1 Why control appears in an ML summary

Because in engineering, prediction is often not the final objective.
Action is.

That means the learned model must eventually support:
- estimation,
- decision,
- tracking,
- stabilization,
- or optimization.

## 12.2 Closed-loop thinking

A predictor used inside a feedback loop is not just a predictor anymore.
Its errors influence actions, which influence future states, which change future data.

That feedback coupling changes everything.

## 12.3 LQR, Kalman, and LQG as the classical spine

The Brunton-Kutz engineering arc makes this explicit:
- LQR for optimal state feedback,
- Kalman filter for optimal state estimation under linear-Gaussian assumptions,
- LQG for combining the two.

This is the classical model-based baseline that ML should be compared against, not ignored.

## 12.4 Machine learning control

ML enters control in several ways:
- learn the model,
- learn a residual on top of a known model,
- learn a low-dimensional embedding,
- optimize a controller in the learned space,
- or use data directly to shape the controller.

## 12.5 Extremum seeking and adaptive control flavors

Some control settings do not require a full world model.
They require online optimization of a performance objective.

This reminds us that not all learning in control is supervised prediction.
Sometimes it is iterative performance shaping.

## 12.6 Ambiguity under feedback

A very important systems point from data-driven control:
when a system is already under feedback, it can be hard to disentangle true plant dynamics from the controller effect.

This is one of those nasty adult problems that simple ML demos ignore.

## 12.7 Why uncertainty matters even more in control

A bad classifier gives a bad label.
A bad model inside control can destabilize the system.

So uncertainty is not an academic luxury.
It is operationally important.

## 12.8 Robotics meaning

Control-oriented ML matters in:
- quadrotor modeling,
- manipulator inverse dynamics correction,
- adaptive locomotion,
- soft robot modeling,
- learned observers,
- surrogate MPC,
- and data-driven estimation for autonomy.

# Module 13: Deep Learning for Engineering Data

## 13.1 Why engineering data is different

Engineering data often has structure that benchmark ML courses underplay:
- temporal correlation,
- multiscale behavior,
- partial observability,
- rare failure events,
- conservation constraints,
- physics priors,
- and expensive data collection.

That changes what "good ML" looks like.

## 13.2 Feature engineering vs representation learning

Traditional engineering ML often starts with carefully chosen features.
Deep learning often tries to learn features automatically.

The mature stance is not ideological.
It is pragmatic.

If good physics-based features are available, use them.
If raw data contains richer structure than hand-designed features can capture, learn representations.

## 13.3 Hybrid models

Some of the most powerful engineering approaches are hybrid:
- physics + learned residual,
- known state-space structure + learned nonlinear correction,
- model reduction + neural closure,
- control law + learned estimator.

Pure black-box learning is not always the best play.

## 13.4 Practical truth

In physical systems, sample efficiency, interpretability, and stability often matter more than having the fanciest architecture.

# Module 14: From Basics to Robotics - What Actually Survives

## 14.1 The whole course distilled

From the books and notes, the big survival lessons are:
- probability matters,
- optimization matters,
- linear algebra matters,
- model selection matters,
- uncertainty matters,
- temporal structure matters,
- and low-dimensional structure hides inside huge systems surprisingly often.

## 14.2 What the mechanical/aerospace version adds

A standard CS-style ML course may stop at regression, classification, and deep nets.

A mechanical/aerospace flavored course keeps going to:
- dynamical systems,
- system identification,
- reduced-order models,
- control,
- estimation,
- and deployment on physical systems.

That extension is exactly why this course is powerful for robotics.

---

# The Full Concept Chain

This course is easiest to remember as a logical ladder.

## Ladder

1. Start with **data, task, model, loss**.
2. Use **probability** to reason about uncertainty and noise.
3. Learn simple mappings with **regression**.
4. Move to discrete decisions with **classification**.
5. Control complexity using **regularization, validation, and model selection**.
6. Handle nonlinear similarity with **kernels**.
7. Get uncertainty-aware nonlinear prediction with **Gaussian processes**.
8. Discover unlabeled structure using **clustering, mixtures, and EM**.
9. Compress data with **PCA / latent-variable thinking**.
10. Scale function approximation with **neural networks and deep learning**.
11. Respect temporal structure with **graphical and sequential models**.
12. Learn evolving physics with **DMD, Koopman, SINDy, and system identification**.
13. Connect prediction to action through **estimation and control**.
14. Deploy all of it in **robotics and autonomy**, where errors become physical consequences.

That is the course arc.

# Robotics Connection Sheet

## A. Perception and state estimation

- regression maps sensor readings to continuous estimates,
- classification turns images or signals into semantic categories,
- probabilistic models express confidence,
- sequential models and Kalman logic handle temporal state estimation,
- PCA / embeddings compress high-dimensional observation spaces.

## B. Manipulation and contact-rich robotics

- regression estimates forces, torques, and latent contact variables,
- classification detects grasp success or contact mode,
- dimensionality reduction compresses tactile or vision features,
- learned residual dynamics can improve model-based control.

## C. Autonomous driving and drones

- classification and regression power perception stacks,
- Gaussian processes and uncertainty-aware models help with risk,
- sequence models capture temporal context,
- data-driven dynamics and control help with adaptation and surrogate prediction.

## D. Robot dynamics and control

- least squares appears in calibration and identification,
- SVD and PCA support reduced-order state representations,
- DMD and Koopman provide interpretable dynamic modes,
- SINDy offers sparse equation discovery,
- learned models support LQR / MPC-style control pipelines.

## E. Scientific machine learning for physical systems

- engineering datasets often require structure-aware models,
- hybrid physics-plus-learning approaches are often stronger than pure black boxes,
- the right inductive bias can reduce sample cost dramatically.

## F. Deployment and systems engineering

- real-time constraints matter,
- numerical conditioning matters,
- training metrics and deployment metrics may differ,
- uncertainty calibration matters,
- a slightly less accurate but much more robust model may be the right engineering choice.

# What to Never Forget

01. Machine learning is not magic. It is structured approximation from data.

02. Data, model, loss, and evaluation are the four pillars.

03. Probability is the language of uncertainty, not optional decoration.

04. Least squares is one of the central workhorses of engineering ML.

$$
\hat{w} = \arg\min_{w} \|Xw-y\|^{2}
$$

05. Under Gaussian noise, least squares is maximum likelihood.

06. Regularization is not a hack. It is encoded prior preference.

07. Overfitting is about generalization failure, not just low training error.

08. Logistic regression is the canonical probabilistic classifier.

$$
p(y=1 \mid x) = \sigma(w^{T}x+b)
$$

09. SVMs care about margin, not just separation.

10. Kernels let linear methods behave nonlinearly.

11. Gaussian processes provide prediction plus uncertainty.

12. EM is alternating inference for latent-variable models.

13. PCA is variance-maximizing linear compression tied to the SVD.

14. Neural networks are compositions of affine maps and nonlinearities.

15. Backpropagation is efficient chain rule.

16. Sequential data needs state, not IID fantasy.

17. Data-driven dynamics methods matter because robots are dynamical systems.

18. Control-oriented learning is harder than static prediction because errors feed back into the world.

19. In robotics, uncertainty and robustness matter as much as average accuracy.

20. A model that looks smart offline but fails in the loop is not smart.

---

# Intuition Anchors by Topic

## Supervised learning
Learning from corrected examples.

## Unsupervised learning
Finding shape without labels.

## Likelihood
How compatible the observed data is with a parameter choice.

## MAP estimation
Likelihood plus prior preference.

## Least squares
Closest algebraic compromise when exact fit is impossible or undesirable.

## Regularization
A polite way of saying "do not believe wild parameter values unless the data really earns them."

## Bias-variance trade-off
The tension between being too stiff and too twitchy.

## Logistic regression
A linear score wrapped into a probability.

## SVM
The boundary with the most breathing room.

## Kernel
A similarity machine pretending the world is linear in a hidden feature space.

## Gaussian process
A Bayesian distribution over possible functions.

## k-means
Find representative centers and assign by proximity.

## EM
Alternate between guessing hidden assignments and refitting the model.

## PCA
Find the directions where the data actually lives.

## Neural network
A trainable nonlinear function factory.

## Backpropagation
Chain rule with industrial discipline.

## DMD
Best-fit linear evolution extracted from snapshot data.

## SINDy
Search a dictionary of candidate physics terms and keep only the ones the data truly uses.

## Koopman thinking
Maybe the nonlinear system looks linear in smarter coordinates.

## Control-oriented ML
Prediction that must survive consequences.

---

# 30-Years-Later Refresher

If future-me forgot almost everything, remember this skeleton:

1. Machine learning learns patterns from data that must generalize.
2. Probability handles uncertainty, noise, and latent structure.
3. Regression predicts continuous outputs.
4. Classification predicts discrete labels.
5. Generalization requires regularization and honest evaluation.
6. Kernels and Gaussian processes handle nonlinear prediction with structure and uncertainty.
7. Clustering, mixtures, and EM discover unlabeled structure.
8. PCA and latent models compress high-dimensional data.
9. Neural networks scale nonlinear approximation and feature learning.
10. Sequential and graphical models handle temporal dependence and hidden state.
11. Engineering ML must eventually confront dynamics.
12. DMD, SINDy, Koopman, and system identification bring ML into physical systems.
13. Estimation and control are the robotics endpoint.
14. Robustness, uncertainty, and deployment reality matter more than benchmark vanity.

If this skeleton is alive, the rest can be rebuilt.

---

# Final Robotics Takeaway

This course teaches one deep lesson:

Machine learning for engineers is not just about fitting data.

It is:
- statistical inference,
- function approximation,
- uncertainty quantification,
- latent-structure discovery,
- dynamic model learning,
- and decision support for physical systems.

The real jump from beginner to practitioner happens when you stop seeing machine learning as isolated algorithms and start seeing it as a layered engineering stack:
- represent the data well,
- model uncertainty honestly,
- choose the right complexity,
- respect dynamics,
- and only trust models that survive contact with deployment.

That is when applied ML stops being coursework and starts becoming systems instinct.

---

# Minimal Core Formula Sheet

$$
\hat{y} = f_{\theta}(x)
$$

$$
\theta^{*} = \arg\min_{\theta} \frac{1}{N}\sum_{i=1}^{N} \ell(f_{\theta}(x_i), y_i)
$$

$$
p(\theta \mid \mathcal{D}) = \frac{p(\mathcal{D} \mid \theta)p(\theta)}{p(\mathcal{D})}
$$

$$
\mathrm{Var}(X) = \mathbb{E}[(X-\mathbb{E}[X])^{2}]
$$

$$
\min_{w} \|Xw-y\|^{2}
$$

$$
X^{T}Xw = X^{T}y
$$

$$
\min_{w} \|Xw-y\|^{2} + \lambda \|w\|^{2}
$$

$$
\min_{w} \|Xw-y\|^{2} + \lambda \|w\|_{1}
$$

$$
p(y=1 \mid x) = \sigma(w^{T}x+b)
$$

$$
p(y=k \mid x) = \frac{e^{a_k(x)}}{\sum_{j=1}^{K}e^{a_j(x)}}
$$

$$
\mathcal{L}_{\text{CE}} = -\sum_{k} y_k \log \hat{y}_k
$$

$$
k(x,x') = \langle \phi(x), \phi(x') \rangle
$$

$$
p(x) = \sum_{k=1}^{K} \pi_k \, \mathcal{N}(x \mid \mu_k, \Sigma_k)
$$

$$
X = U \Sigma V^{T}
$$

$$
\hat{x}_{\text{PCA}} = U_r U_r^{T} x
$$

$$
h = \phi(Wx+b)
$$

$$
\theta \leftarrow \theta - \eta \nabla_{\theta}\mathcal{L}
$$

$$
x_{k+1} = A x_k + w_k, \qquad y_k = C x_k + v_k
$$

$$
X' \approx AX
$$

$$
\dot{x} \approx \Theta(x)\Xi
$$

$$
\dot{x} = Ax + Bu, \qquad y = Cx + Du
$$

---

# End Note

This is not "just machine learning."

It is the bridge between data, inference, dynamics, and action.

And that is exactly why it belongs in a robotics concept stack.

---