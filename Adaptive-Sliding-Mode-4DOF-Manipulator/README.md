# Adaptive Sliding-Mode Control of a 4-DOF Manipulator

A 4-DOF planar robot arm performs multi-target pick-and-place while its controller is deliberately given the wrong model: link masses underestimated by 20%, payload mass by 57%, and the payload mass changes instantaneously at pickup. The adaptive sliding-mode controller tracks anyway.

https://github.com/user-attachments/assets/07cabbb9-8b7c-4f54-b42b-d2f3b118daed

*Four sequential pick-and-place operations inside a constrained diamond workspace. The last mass was intentionally made larger than the motor gains could handle.*

## Overview

Perfect models don't exist on real robots — payloads vary, links flex, parameters drift. This project builds the whole stack in Mathematica and stress-tests it against that reality:

- Full Euler–Lagrange dynamics for a 4-DOF serial manipulator, derived symbolically
- Inverse kinematics and trajectory generation into a narrow, collision-constrained workspace
- An adaptive sliding-mode controller that estimates mass parameters online
- A contact extension: the arm collides with a wall, and the impact is resolved with an impulse model

Everything is integrated with Mathematica's DAE-aware `NDSolve`.

## Results

| Metric | Result |
|---|---|
| Pick-and-place cycles | 4 of 4 successful |
| Total operation time | 44 s |
| Payload mass error | 57% underestimated |
| Tracking recovery after pickup | about 1–2 s |
| Stability | maintained for all targets |

Tracking error spikes at each pickup (the mass changes instantly), then converges back to zero — the expected behavior of a well-tuned sliding-mode controller.

https://github.com/user-attachments/assets/1e1aa50d-9fc5-4f41-ac36-7a1ad51ee75e

### Plots

<table>
<tr>
<td width="50%">

<img src="https://github.com/user-attachments/assets/6774cf2e-ad96-4138-9771-5968a526f8f7" width="100%"/>

Control torques, mass 1

</td>
<td width="50%">

<img src="https://github.com/user-attachments/assets/3d3e2037-2abc-4284-a2c6-0ee23aabecc0" width="100%"/>

Tracking error, mass 1

</td>
</tr>
<tr>
<td width="50%">

<img src="https://github.com/user-attachments/assets/acf1f531-9c1f-49a1-a1d0-78b0cd9397f9" width="100%"/>

Mass parameter error, link 4

</td>
<td width="50%">

<img src="https://github.com/user-attachments/assets/6aa295ce-be83-45b1-a79f-1a3159f41a52" width="100%"/>

Joint angles, all 4 masses

</td>
</tr>
<tr>
<td width="50%">

<img src="https://github.com/user-attachments/assets/c4e70d44-bb0e-4370-84a5-4157ffe8cfa2" width="100%"/>

Joint-1 torque comparison

</td>
<td></td>
</tr>
</table>

## The controller

The robot follows the standard manipulator equation:

```math
M(q)\ddot q + C(q,\dot q)\dot q + G(q) = \tau
```

Define the tracking error and a sliding surface:

```math
\tilde q = q - q_d, \qquad s = \dot{\tilde q} + \Lambda \tilde q
```

Writing the dynamics along a reference trajectory in regressor form separates the known structure from the unknown parameters:

```math
M(q)\ddot q_r + C(q,\dot q)\dot q_r + G(q) = Y(q,\dot q,\dot q_r,\ddot q_r)\,a
```

The control law combines a feedforward term using the estimated parameters with a robust sliding term:

```math
\tau = Y\,\hat a - K\,\mathrm{sat}(s)
```

The saturation function is a boundary-layer replacement for the sign function — it kills the chattering that a pure switching law would produce. The parameter estimate `â` is updated online, which is what lets the controller absorb a 57% payload error without losing stability.

## Part 2: wall impact and bounce

Most trajectory-tracking projects stop at free space. This one adds contact: the arm is commanded to a target deliberately placed past a wall, so a collision is unavoidable.

https://github.com/user-attachments/assets/06b2e12b-590d-44d9-97f1-db28dafec640

<p align="center">
  <img src="https://github.com/user-attachments/assets/ebb155c5-d221-496b-915a-00e5557a35fe" width="400">
</p>

*Blue is the approach path, green is the bounce.*

When the tip crosses the wall plane, the simulation applies an instantaneous impulse update:

- Normal direction (restitution): `vₓ⁺ = −e·vₓ⁻` with e = 0.7
- Tangential direction (Coulomb friction): impulse `Jₜ = μ·Jₙ` with μ = 0.1, including the sliding-vs-sticking condition
- The controller then re-initializes at the collision state and tracks a new trajectory away from the wall

The log records collision at t ≈ 1.56 s, the pre/post-impact velocities, energy retention across the impact, and a successful bounce-phase recovery. Collision detection, impulse response, state reset, and re-planning are the building blocks of real manipulation — this was the point of the extension.

## Full derivation

A technical report deriving the dynamics, the sliding-mode stability argument, the adaptive law, and the impact mechanics is included:

**[Technical report (PDF)](Report/Adaptive-Sliding-Mode-4DOF-Manipulator.pdf)**

## Author

Gustavo Torres — [GitHub](https://github.com/gustavotorr) · [LinkedIn](https://www.linkedin.com/in/gustavo-torres111/)
