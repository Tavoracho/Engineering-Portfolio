# Statically Balanced Mechanism

A 3D-printed arm that holds its payload perfectly still at any angle — no motor, no brake, zero holding torque. A linear spring, routed by a string over a teardrop-shaped cam, produces a torque that cancels gravity exactly through a full rotation. The cam profile isn't tuned; it's derived.

<p align="center">
  <a href="https://youtu.be/1I35prm3X8c">
    <img src="Media/Thumbnail%20Horizontal.png" width="85%">
  </a>
</p>

<p align="center"><i>Click the image for the demo video, or watch it directly at <a href="Media/Statically%20Balanced%20Mechanism%20v2.mp4">Media/Statically Balanced Mechanism v2.mp4</a>.</i></p>

## Why

Gravity compensation is one of the most useful tricks in mechanism design. If a payload's weight is exactly counterbalanced at every position, it can be moved with negligible effort and parked anywhere. Surgical robot arms, exoskeletons, articulated desk lamps, and camera rigs all rely on it — and in actuated systems it means motors only ever fight inertia and friction, never gravity.

This build balances a tape measure (0.485 kg) on a 5-inch arm through 360°.

## The derivation

The system is in equilibrium at every angle only if total potential energy is constant in the arm angle θ:

```math
\frac{\partial V}{\partial \theta} = 0, \qquad V = mgL\cos\theta + \tfrac{1}{2}kx^2
```

The string wrapped over the cam ties spring stretch to arm angle through the cam radius: dx/dθ = R(θ). Try a profile of the form

```math
R(\theta) = R_{max}\cos\!\left(\tfrac{\theta}{2}\right)
```

Integrating gives x(θ) = 2R_max·sin(θ/2), so the spring torque becomes

```math
\tau_s = k\,x(\theta)\,R(\theta) = kR_{max}^2 \cdot 2\sin\tfrac{\theta}{2}\cos\tfrac{\theta}{2} = kR_{max}^2\sin\theta
```

— a pure sine, by the double-angle identity. It cancels the gravity torque mgL·sinθ at every angle, not just at a few calibrated poses, provided

```math
R_{max} = \sqrt{\frac{mgL}{k}}
```

Plotting R(θ) traces the teardrop shape of the printed cam.

### Prototype numbers

| Parameter | Value |
|---|---|
| Payload mass m | 0.485 kg |
| Arm length L | 5 in (0.127 m) |
| Spring constant k | 974.1 N/m |
| **Cam radius R_max** | **0.980 in (24.9 mm)** |

The full write-up — potential energy formulation, torque balance, and the prototype calculation — is in **[Derivation/Tear Drop Derivation.pdf](Derivation/Tear%20Drop%20Derivation.pdf)**.

<details>
<summary>Read the derivation pages inline</summary>

<p align="center">
  <img src="Derivation/Tear%20Drop%20Derivation_page-0001.jpg" width="80%">
  <img src="Derivation/Tear%20Drop%20Derivation_page-0002.jpg" width="80%">
  <img src="Derivation/Tear%20Drop%20Derivation_page-0003.jpg" width="80%">
</p>

</details>

## Hardware

Everything structural is 3D printed: the base with an integrated spring channel, the payload arm, and the cam, whose profile is parameterized in CAD directly from the equation above. A string wraps the cam and connects to a steel extension spring hidden inside the vertical post.

<p align="center">
  <img src="Media/Assembly%20Image.png" width="60%">
</p>

Print-ready 3MF files are included:

- [`CAD/Cam Assembly/`](CAD/Cam%20Assembly) — the teardrop cam and its base (the version in the video)
- [`CAD/Crank-Gear Assembly/`](CAD/Crank-Gear%20Assembly) — crank, gear, and disk parts from the drive experiments

## Author

Gustavo Torres — [GitHub](https://github.com/gustavotorr) · [LinkedIn](https://www.linkedin.com/in/gustavo-torres111/)

MIT license.
