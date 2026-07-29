# ⚖️ Statically Balanced Mechanism — Spring-Cam Gravity Compensator

<div align="center">

![System Status](https://img.shields.io/badge/status-complete-brightgreen)
![Hardware](https://img.shields.io/badge/build-3D%20printed-blue)
![Mechanics](https://img.shields.io/badge/mechanics-gravity%20compensation-purple)
![License](https://img.shields.io/badge/license-MIT-blue)

**A mechanism that holds a payload perfectly still at any arm angle — with zero holding torque and no motor.**

*A linear spring, a string, and a teardrop-shaped cam derived so the spring torque exactly cancels gravity for a full 360° sweep.*

[▶ Watch the demo on YouTube](https://youtu.be/1I35prm3X8c)

</div>

<p align="center">
  <a href="https://youtu.be/1I35prm3X8c">
    <img src="Media/Thumbnail%20Horizontal.png" width="85%">
  </a>
</p>

---

## 🎯 Project Overview

Gravity compensation is one of the most useful ideas in mechanism design: if a payload's weight is exactly counterbalanced at every position, it can be moved with negligible effort and parked anywhere — no brakes, no holding torque, no motor current. It's the same principle used in surgical robot arms, exoskeletons, desk lamps, and counterbalanced camera rigs.

This project designs, derives, 3D prints, and demonstrates a **statically balanced one-DOF arm**: a tape-measure payload on a 5-inch arm that stays wherever you put it, through a full rotation. The balancing element is a **linear extension spring** connected by a string wrapped over a **custom cam** at the pivot. The cam's radius profile is derived analytically so that the spring's restoring torque matches the gravitational torque *identically* at every angle — not approximately, not at a few poses.

The derived profile turns out to be a clean closed form:

```math
R(\theta) = R_{max}\cos\!\left(\frac{\theta}{2}\right), \qquad R_{max} = \sqrt{\frac{mgL}{k}}
```

which traces the **teardrop shape** of the printed cam.

---

## 🔑 Key Features

```
✓ Perfect static balance over 360°          ✓ Closed-form cam profile derivation
✓ Zero holding torque at any angle          ✓ Energy method (constant potential energy)
✓ Passive — no motors, no electronics       ✓ Parametric CAD cam from the math
✓ 3D-printed prototype, real payload        ✓ Verified on hardware (see video)
```

---

## 🧠 The Math (Short Version)

The system is in equilibrium at **every** angle only if total potential energy is constant:

```math
\frac{\partial V}{\partial \theta} = 0, \qquad V = \underbrace{mgL\cos\theta}_{\text{gravity}} + \underbrace{\tfrac{1}{2}kx^2}_{\text{spring}}
```

The string drawn over the cam links spring stretch to arm angle through the cam radius, `dx/dθ = R(θ)`. Assuming the profile `R(θ) = R_max cos(θ/2)` and integrating gives `x(θ) = 2R_max sin(θ/2)`, so the spring torque becomes

```math
\tau_s = k\,x(\theta)\,R(\theta) = kR_{max}^2\,\bigl(2\sin\tfrac{\theta}{2}\cos\tfrac{\theta}{2}\bigr) = kR_{max}^2\sin\theta
```

— a pure sine that cancels the gravity torque `mgL sin θ` exactly, provided

```math
R_{max} = \sqrt{\frac{mgL}{k}}
```

### Prototype numbers

| Parameter | Value |
|-----------|-------|
| Payload mass `m` | 0.485 kg (tape measure) |
| Arm length `L` | 5 in (0.127 m) |
| Spring constant `k` | 974.1 N/m |
| **Resulting cam radius `R_max`** | **0.980 in (24.9 mm)** |

📄 **[Full derivation (PDF)](Derivation/Tear%20Drop%20Derivation.pdf)** — the complete potential-energy formulation, torque balance, and prototype calculation.

<details>
<summary>📖 View the derivation pages inline</summary>

<p align="center">
  <img src="Derivation/Tear%20Drop%20Derivation_page-0001.jpg" width="80%">
  <img src="Derivation/Tear%20Drop%20Derivation_page-0002.jpg" width="80%">
  <img src="Derivation/Tear%20Drop%20Derivation_page-0003.jpg" width="80%">
</p>

</details>

---

## 🛠️ Hardware

The prototype is fully 3D printed: the base with an integrated spring channel, the payload arm, and the teardrop cam parameterized directly from the derived profile. A string wraps the cam and connects to a steel extension spring hidden inside the vertical post.

<p align="center">
  <img src="Media/Assembly%20Image.png" width="60%">
</p>

**Print files** (3MF) are included:

- [`CAD/Cam Assembly/`](CAD/Cam%20Assembly) — the teardrop cam and its base (the version in the video)
- [`CAD/Crank-Gear Assembly/`](CAD/Crank-Gear%20Assembly) — crank, gear, and disk parts for the drive/variant experiments

*(SolidWorks source files not included — the 3MF meshes are print-ready.)*

---

## 🎥 Demo

**[▶ Statically Balanced Mechanism — demo video (YouTube)](https://youtu.be/1I35prm3X8c)**

The same video is included in this repo at [`Media/Statically Balanced Mechanism v2.mp4`](Media/Statically%20Balanced%20Mechanism%20v2.mp4).

---

## 🧪 Why This Matters

Static balancing shows up anywhere a mechanism must feel weightless or hold position without power:

- **Collaborative & surgical robot arms** — gravity-compensated links reduce motor sizing and make hand-guiding safe
- **Exoskeletons & assistive devices** — support limb weight passively
- **Camera rigs and articulated lamps** — park anywhere, no drift
- **Energy-efficient actuation** — motors only fight inertia and friction, never gravity

It's also a satisfying case study in **energy-method mechanism synthesis**: instead of tuning a design iteratively, the geometry falls out of one equation — `∂V/∂θ = 0`.

---

## 👤 Author

**Gustavo Torres**

- GitHub: [@gustavotorr](https://github.com/gustavotorr)
- LinkedIn: [Gustavo Torres](https://www.linkedin.com/in/gustavo-torres111/)

---

⭐ Star this repo if you find it helpful!
