# Rotating-Link Projectile

Throw a rigid rod with both linear velocity and spin. Its center of mass flies a clean parabola, but each tip traces a looping path through the air — so which end hits the ground first, where, and how fast? This simulation answers that exactly.

https://github.com/user-attachments/assets/4af4847c-ee67-41d3-81ef-741fe2b7693d

## The problem

This is a hybrid kinematics and root-finding problem: coupled translation and rotation, with the answer determined by which contact point reaches a boundary first. The same structure shows up in tumbling-debris reentry analysis, thrown-object prediction in robotics, and contact detection in physics engines.

## The model

The center of mass follows ballistic motion and the rod rotates at constant rate:

```math
x(t) = x_0 + v_{x0}t, \qquad y(t) = y_0 + v_{y0}t - \tfrac{1}{2}gt^2, \qquad \theta(t) = \theta_0 + \omega t
```

Each tip sits half a link length from the center:

```math
x_{A,B}(t) = x(t) \pm \tfrac{L}{2}\cos\theta(t), \qquad y_{A,B}(t) = y(t) \pm \tfrac{L}{2}\sin\theta(t)
```

Ground contact is the first root of y_tip(t) = 0 — a transcendental equation (parabola plus sinusoid), solved numerically with `FindRoot` for each tip. Whichever root comes first wins.

## Initial conditions and result

Launched from (0, 18) m at (6, 20) m/s, with a 20 m link starting vertical (θ₀ = π/2) and spinning at ω = 0.9π rad/s.

**Tip B strikes first**, at t = 4.444 s and x = 26.67 m, having swept through θ = 14.14 rad (about 4.5π). At impact the rod's state is ẋ = 6.00 m/s, ẏ = −23.6 m/s, θ̇ = 2.83 rad/s. Physically, the rotation sweeps tip B downward faster than the ballistic descent alone, while tip A is momentarily carried upward — the frame of the impact is captured in `Media/ImpactFrame.png`.

## Visualization

The notebook includes an interactive animation built with `Manipulate`: both tips, the center of mass, the link body, the trailing COM trajectory, and a persistent marker at the point of first contact. Time scrubs from 0 to 5.5 s at 0.01 s resolution.

## Running it

Open `Code/Projectile-Motion-Simulation.nb` in Wolfram Mathematica (12.0+, no extra packages). A PDF export is in the same folder.

## Author

Gustavo Torres — [GitHub](https://github.com/gustavotorr) · [LinkedIn](https://www.linkedin.com/in/gustavo-torres111/)

MIT license.
