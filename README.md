# Programming Assignment 1

## Gridworld Value Iteration & Acrobot Reinforcement Learning

This repository contains the solutions for the Programming Assignment
consisting of two independent problems:

1.  **Gridworld MDP with Value Iteration**
2.  **TD-based Control in Acrobot-v1**

The solutions are implemented in two separate Jupyter notebooks:

PA1_GridworldandValueIteration.ipynb\
PA1_Acrobot_Solution.ipynb

All required dependencies are listed in `requirements.txt`.

------------------------------------------------------------------------

# Contributions

1. NA23B040 - Chandran K. - Section - 3 (1, 4, 5, BONUS)
2. ED23B051 - Kalyan S. - Section - 3 (1, 2, 3, 5)
3. EE23B004 - Amritam - Section - 2 (fully)

------------------------------------------------------------------------

# Installation

To install the required libraries inside a runtime (for example **Google
Colab**), run the following command inside a notebook cell:

``` python
!pip install -r requirements.txt
```

------------------------------------------------------------------------

# 1. Gridworld and Value Iteration

Notebook: `PA1_GridworldandValueIteration.ipynb`

This notebook implements a **Markov Decision Process (MDP)** for a drone
navigating a hazardous grid environment.

The drone must:

-   Collect water from a **lake**
-   Deliver water to the **fire zone**
-   Avoid **smoke fumes** and **boulders**

The notebook includes:

-   Environment modelling
-   MDP formulation
-   Value Iteration
-   Policy visualization
-   Q-value verification

------------------------------------------------------------------------

## Environment Representation

The gridworld is defined as a **NumPy array**, where each cell
represents a terrain type.

  Symbol   Meaning
  -------- ------------------
  `l`      Lake
  `sf`     Smoke fumes
  `b`      Boulders
  `g`      Goal / Fire zone

Example:

``` python
map = np.array([
["l", "", "", "", ""],
["", "", "sf", "", ""],
["", "", "", "", ""],
["", "", "sf", "", ""],
["", "", "", "", "g"]
])
```

Different environments can be tested by modifying this array.

------------------------------------------------------------------------

## Main Components

### Gridworld Class

Handles:

-   State transitions
-   Environment representation
-   Terrain constraints

### DroneMDP Class

Defines:

-   State space
-   Action space
-   Rewards and penalties
-   Transition probabilities

Penalty values can be customized while initializing the class.

Example:

``` python
mdp = DroneMDP(smoke_penalty=-5, crash_penalty=-100)
```

------------------------------------------------------------------------

## Value Iteration

The optimal value function is computed using:

``` python
value_iteration(gamma)
```

Where:

-   `gamma` is the discount factor.

The algorithm iteratively updates state values until convergence.

------------------------------------------------------------------------

## Policy Visualization

The notebook visualizes:

-   Optimal value function
-   Optimal policy
-   State transitions

Additionally, a verification cell provides **expected optimal Q-values**
for each state--action pair.

------------------------------------------------------------------------

# 2. Acrobot Reinforcement Learning

Notebook: `PA1_Acrobot_Solution.ipynb`

This section implements **Temporal Difference (TD) learning algorithms**
to solve the **Acrobot-v1** control problem.

The Acrobot is a **two-link pendulum** where only the second joint is
actuated.\
The goal is to swing the end-effector above a specified height.

------------------------------------------------------------------------

## Environment

Gymnasium environment used:

`Acrobot-v1`

Observation space:

    [cos(θ1), sin(θ1), cos(θ2), sin(θ2), θ̇1, θ̇2]

This forms a **6-dimensional continuous state space**.

------------------------------------------------------------------------

## State Discretisation

To apply tabular reinforcement learning methods, the continuous
observation space is discretised using **binning**.

Each dimension is divided into `n_bins`.

Example:

    n_bins = 10
    Total states = n_bins^6 = 1,000,000

This converts the continuous state into a discrete index used for the
Q-table.

------------------------------------------------------------------------

## Implemented Algorithms

### SARSA (On-policy TD)

Update rule:

    Q(S,A) ← Q(S,A) + α [R + γ Q(S',A') − Q(S,A)]

### Q-learning (Off-policy TD)

Update rule:

    Q(S,A) ← Q(S,A) + α [R + γ max_a Q(S',a) − Q(S,A)]

Both algorithms are implemented and compared experimentally.

------------------------------------------------------------------------

## Hyperparameter Tuning

The following hyperparameters are explored:

    α ∈ {0.1, 0.2}
    ε ∈ {0.01, 0.1, 1.0}

Training is performed with:

-   constant ε
-   exponential ε-decay

The best performing configurations are selected based on average
returns.

------------------------------------------------------------------------

## Multi-seed Evaluation

The best hyperparameters are evaluated across **10 random seeds**.

Performance is reported using **95% confidence intervals**:

    mean ± 1.96 * std / sqrt(n)

This provides statistically reliable performance estimates.

------------------------------------------------------------------------

## Exploration Scheduling

An exploration schedule is implemented where:

    ε : 1.0 → 0.1 (linearly decayed)

After decay:

    ε = 0.1

Two metrics are evaluated:

1.  **Online performance** (during training)
2.  **Final policy performance** using greedy policy (ε = 0)

------------------------------------------------------------------------

## Effect of Discretisation

State discretisation resolution is analysed using:

    n_bins ∈ {5, 10, 15, 20}

Increasing bins improves policy resolution but increases the state space
exponentially.

------------------------------------------------------------------------

## Reward Shaping Analysis

A shaped reward function is analysed:

r = (ηh / 2) + sign(-1 + ηh)((2 − ηh)/2)

where:

-   `h` is the tip height of the second link
-   `η > 0`

This simplifies to:

    r = ηh − 1   if h < 1/η
    r = 1        if h ≥ 1/η

This reward provides **denser feedback** to encourage the agent to raise
the tip height progressively.

------------------------------------------------------------------------

## Empirical Evaluation of Shaped Reward

Experiments are conducted for:

    η ∈ {0.5, 1, 2, 5}

Learning curves are compared with the standard reward function.

------------------------------------------------------------------------

## Physics-Based Reward Function

A custom reward function based on physical intuition is also explored:

    reward = 0.4  * height_of_tip
           + 0.02 * resonance_condition
           + 0.01 * alignment
           - 0.04 * angular_velocity_penalty

This reward encourages physically meaningful swing-up behavior.

------------------------------------------------------------------------

## Energy-comparison Reward Function

A custom reward function based on total system energy is explored:

    reward = -|Upright_energy - (Kinetic_energy + Potential_Energy)|

This reward encourages system to reach the most upright condition.

------------------------------------------------------------------------

## Running the Notebooks

Both notebooks can be executed by **running cells sequentially**.

Recommended order:

1.  Install dependencies
2.  Run environment setup cells
3.  Execute algorithm implementation cells
4.  Run evaluation and visualization cells

------------------------------------------------------------------------

# Repository Structure

    .
    ├── PA1_GridworldandValueIteration.ipynb
    ├── PA1_Acrobot_Solution.ipynb
    ├── requirements.txt
    └── README.md

------------------------------------------------------------------------
