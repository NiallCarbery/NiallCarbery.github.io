---
title: Langevin Dynamics & Data Sampling
date: 2025-02-11 20:21:00 +0100
categories: [Quantum Mechanics, Langevin Dynamics]
tags: [lab, simulation]     # TAG names should always be lowercase
author: Niall
description: Numerical Simulations of Langevin Dynamics.  
math: true
---

# Exploring Particle Behavior: A Deep Dive into Langevin Dynamics

In this blog post, I’ll share insights from a recent lab where I explored the fascinating world of **Langevin Dynamics**. The lab was designed to simulate particle behavior within a Leonard-Jones potential, integrating deterministic forces with stochastic thermal noise. This blend provides a powerful toolset not only for understanding molecular motion but also for data sampling applications. Below, I detail the theory, simulation methods, results, and implications of this experiment.

---

## Introduction

Langevin Dynamics offers a compelling way to model systems where thermal fluctuations and damping forces play significant roles. In my lab, the focus was on simulating a single particle moving within a Leonard-Jones potential. The simulation combined:
- **Deterministic forces** from the particle potential.
- **Stochastic forces** representing thermal fluctuations.
- **Damping forces** to mimic solvent effects.

These elements together produce trajectories that, over time, converge toward thermal equilibrium. The equilibrium state, characterized by Boltzmann-like statistics, enables the extraction of physically meaningful properties such as the position and velocity distributions, and even the potential of mean force.

---

## Theory Overview

### Langevin Dynamics

At its core, Langevin Dynamics extends Newtonian mechanics by incorporating both friction and random forces. The modified equation of motion is:

$$
M \ddot{X} = -\nabla U(X) - \gamma M \dot{X} + \sqrt{2M\gamma k_BT} \, R(t)
$$

where:
- $$ U(X) $$ is the potential energy (here modeled by the Leonard-Jones potential).
- $$ \gamma $$ is the damping coefficient.
- $$ R(t) $$ represents Gaussian white noise (zero mean).
- $$ k_B $$ and $$ T $$ are Boltzmann’s constant and temperature, respectively.

This formulation allows us to simulate the complex interplay between deterministic and stochastic influences on the particle’s motion.

### Leonard-Jones Potential

The Leonard-Jones potential is a simplified model to describe interatomic interactions. It is given by:

$$
U(r) = 4\epsilon \left[\left(\frac{\sigma}{r}\right)^{12} - \left(\frac{\sigma}{r}\right)^6\right]
$$

Key insights from its form include:
- A **repulsive** component at very short distances.
- An **attractive** component at intermediate distances.
- A clear minimum at the equilibrium separation (around $$ r=1 $$ for $$ \epsilon, \sigma = 1 $$), where the force between particles is balanced.

### Velocity Verlet Algorithm

For integrating the motion of the particle, the lab used the **velocity Verlet algorithm**. This method is ideal because it simultaneously updates both positions and velocities, ensuring higher numerical precision—critical when dealing with stochastic forces. The algorithm involves:
1. A half-step update for velocity.
2. A full-step update for position.
3. A second half-step update for velocity using the newly computed acceleration.

The update equations are:

$$
\begin{aligned}
x(t + \Delta t) &= x(t) + v(t) \Delta t + \frac{1}{2} a(t) \Delta t^2, \\
v(t + \Delta t) &= v(t) + \frac{1}{2} \big(a(t) + a(t + \Delta t)\big) \Delta t.
\end{aligned}
$$

This scheme minimizes the error associated with discretizing the equations of motion while preserving the symplectic nature of the system.

### Potential of Mean Force

The **potential of mean force (PMF)** is a concept used to understand how the free energy of a system varies with particle position. The PMF is related to the probability density $$ p(r) $$ of finding the particle at a position $$ r $$ by:

$$
U_{PMF}(r) = -k_BT \ln(1 + h(r))
$$

where $$ h(r) $$ represents deviations from an ideal gas distribution. In the simulation, the PMF helps to capture both the underlying deterministic potential and the perturbations due to thermal noise.

### Equipartition and Thermal Equilibrium

The **equipartition theorem** asserts that, at thermal equilibrium, energy is distributed equally among all quadratic degrees of freedom. For translational motion in one dimension, this means:

$$
\langle KE \rangle = \frac{1}{2} k_BT
$$

This relationship was verified by analyzing the velocity histograms, which, when projected, closely follow a Maxwell-Boltzmann (Gaussian) distribution.

---

## Simulation Methods and Implementation

The simulation was carried out with the following key parameters:
- **Time step ($$ \Delta t $$)**: 0.001
- **Number of steps**: 100,000 per run
- **Damping coefficient ($$ \gamma $$)**: Varied to assess its influence on particle behavior.
- **Initial conditions**: Set for position and velocity to observe the evolution toward equilibrium.

### Data Collection

- **Single Run**: Data was recorded every 10 time steps to construct histograms of both particle position and velocity.
- **Multi-Run Sampling**: To reduce statistical fluctuations and get reliable probability distributions, the simulation was repeated over 100 iterations. This averaging process confirmed the convergence to the expected Boltzmann distributions.

### Impact of Damping

An interesting part of the lab was analyzing how the damping coefficient affects the sampling:
- **Lower damping ($$ \gamma $$ close to zero)** results in broader exploration of the potential energy surface, as the particle retains higher kinetic energy.
- **Higher damping values** lead to more localized behavior around the potential minimum, as the particle loses energy more quickly.

A square root dependency of the time spent in the potential well on $$ \gamma $$ was observed, aligning with theoretical predictions.

---

## Results and Discussion

### One Sample Run

The results from a single simulation run showed:
- **Position Histogram**: A clear peak near the expected minimum of the Leonard-Jones potential, indicating that the particle spends most time in regions of lower potential energy.
- **Velocity Histogram**: A Gaussian distribution (with a slight shift due to potential asymmetry) confirming the Maxwell-Boltzmann distribution.

This suggests that even in a single run, the system approaches thermal equilibrium with predictable statistical properties.

### Multi-Run Sampling

By averaging over 100 runs:
- **Enhanced Consistency**: The position and velocity distributions became smoother, and the Boltzmann fit improved significantly.
- **Observed Temperature**: Calculated from the kinetic energy, it closely matched the input temperature, supporting that the simulation correctly reproduced equilibrium behavior.

### Damping Coefficient Effects

The study clearly illustrated how varying $$ \gamma $$ modulates particle dynamics:
- **Low $$ \gamma $$**: The system explores a broader region of the potential, sampling higher energy states.
- **High $$ \gamma $$**: The particle is more confined, leading to finer, localized sampling near the potential minimum.

This tunability is crucial for applications in data sampling and machine learning, where controlling the breadth of the data distribution is important.

---

## Conclusion

The lab successfully demonstrated the use of Langevin Dynamics in modeling particle motion under combined deterministic and stochastic forces. Key takeaways include:
- The **velocity Verlet algorithm** is highly effective for integrating the equations of motion in a system influenced by thermal fluctuations.
- The **Leonard-Jones potential** provides a meaningful framework for understanding interparticle interactions, with clear implications for equilibrium properties.
- Varying the damping coefficient $$ \gamma $$ can significantly influence the sampling of the energy landscape—a feature that can be exploited in data sampling and machine learning.

By integrating theory with simulation, this lab not only reinforced fundamental concepts in molecular dynamics but also highlighted the practical utility of Langevin Dynamics in exploring and sampling complex systems.

---

## Further Reading

For those interested in deeper theoretical details or practical implementations, consider exploring the following resources:
- **Caffrey’s Langevin Dynamics Lab Guidelines** for detailed experimental setup and parameter selection.
- **Chandler, D. (1987). Introduction to Modern Statistical Mechanics.** – A classic text on statistical mechanics.
- **Frenkel & Smit, Understanding Molecular Simulation** – A comprehensive guide on simulation methods.
- **Schlick, T. Molecular Modeling and Simulation: An Interdisciplinary Guide** – For further insights into simulation techniques and applications.

I hope you found this exploration of Langevin Dynamics insightful. Stay tuned for more blog posts where I dive further into the nuances of molecular simulation and computational physics!
