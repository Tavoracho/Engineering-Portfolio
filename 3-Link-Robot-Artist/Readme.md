# 3-Link Robot Artist

A 3-DOF planar robot arm traces a cursive letter 'a' using sliding-mode control, with its equations of motion derived from scratch via Lagrangian mechanics. The controller keeps tracking despite deliberate errors in the model.

https://github.com/user-attachments/assets/2b4e689a-bb18-45c2-b1b4-682679f814c3

## Overview

The point of this project was to build the full pipeline by hand rather than lean on a robotics toolbox:

1. Define the geometry and mass properties of the three links
2. Derive kinetic and potential energy, form the Lagrangian, and extract the dynamics symbolically — the mass matrix M(q), Coriolis terms C(q,q̇), and gravity vector G(q)
3. Design a sliding-mode controller with a Lyapunov stability argument
4. Integrate numerically and animate the result

Everything runs in a single Mathematica notebook.

## Performance

| Metric | Value |
|---|---|
| Steady-state tracking error | < 0.01 rad |
| Settling time | ~3.5 s |
| Peak control torque | ~15 N·m |
| End-effector path accuracy | ±2 mm |
| Chattering | eliminated |

## The controller

Standard manipulator dynamics:

```math
M(q)\ddot q + C(q,\dot q)\dot q + G(q) = \tau
```

Define the tracking error and a sliding surface that forces it to decay exponentially:

```math
\tilde q = q - q_d, \qquad s = \dot{\tilde q} + \Lambda \tilde q
```

The control law is model-based feedforward plus a robust switching term:

```math
\tau = \hat M \ddot q_r + \hat C \dot q_r + \hat G - K\,\mathrm{sat}(s)
```

A pure sign function here would chatter — switch at high frequency and hammer the actuators. Replacing it with a saturation function inside a boundary layer (Φ = 0.1) trades an infinitesimally thin sliding surface for a smooth control signal, which is the practical version used on real hardware. Stability follows from the Lyapunov argument V̇ = −sᵀK·sat(s) < 0.

Gains: λ = 4 (convergence rate), k = 1 (robustness), Φ = 0.1 (boundary layer).

## The trajectory

Each joint follows a smooth sinusoid whose superposition traces the cursive pattern at the end-effector:

```math
q_1(t) = \tfrac{\pi}{3} + \tfrac{\pi}{8}\sin(t/3), \quad
q_2(t) = \tfrac{\pi}{5} + \tfrac{\pi}{6}\cos(t/3), \quad
q_3(t) = \tfrac{\pi}{2} + \tfrac{\pi}{4}\sin(t/3)
```

Links are 1 m / 1 kg each.

## Running it

Open `Code/3-Link-Robot-Artist.nb` in Wolfram Mathematica (12.0+, no extra packages). A PDF export is alongside it. The final traced output is in `Media/Final Output.png`.

## Author

Gustavo Torres — [GitHub](https://github.com/gustavotorr) · [LinkedIn](https://www.linkedin.com/in/gustavo-torres111/)

MIT license.
