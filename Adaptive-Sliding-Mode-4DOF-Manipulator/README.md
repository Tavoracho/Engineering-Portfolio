# 🦾 Adaptive Sliding-Mode Control of a 4-DOF Robot Manipulator

<div align="center">

![System Status](https://img.shields.io/badge/status-complete-brightgreen)
![Wolfram](https://img.shields.io/badge/platform-Wolfram%20Mathematica-red)
![Robotics](https://img.shields.io/badge/robotics-serial%20manipulator-blue)
![Control](https://img.shields.io/badge/control-adaptive%20SMC-purple)
![License](https://img.shields.io/badge/license-MIT-blue)

**A nonlinear robotics control system that performs multi-target pick-and-place under extreme modeling uncertainty**

*Tracks trajectories, adapts to unknown payloads, and remains stable through discontinuous dynamics*

[Demo](#-results) • [Control](#-control-model) • [Dynamics](#-robot-dynamics) • [Why this matters](#-why-this-matters)

---

![Manipulator Demo](media/robot_pick_place.gif)

*Four sequential pick-and-place operations inside a constrained diamond workspace*

</div>

---

## 🎯 Project Overview

This project implements a **fully nonlinear adaptive sliding-mode controller** for a **4-DOF planar robotic manipulator** tasked with retrieving four payloads from a **narrow diamond-shaped workspace** and returning them to a drop-off zone.

The controller is intentionally given the **wrong model**:

* Link masses are **20% underestimated**
* Payload mass is **57% underestimated**
* Payload mass changes **instantaneously** when picked up

Yet the robot still tracks trajectories and completes all four tasks robustly .

This is exactly the kind of uncertainty encountered in **real robots** — where perfect modeling is impossible.

---

## 🔑 Key Features

```
✓ Full Euler–Lagrange robot dynamics     ✓ Adaptive sliding-mode control
✓ 4-DOF serial manipulator              ✓ Unknown link & payload masses
✓ Time-varying system dynamics          ✓ Narrow collision-constrained workspace
✓ Multi-target pick-and-place           ✓ Impact & bouncing physics
✓ Inverse kinematics + trajectory gen   ✓ High-fidelity numerical integration
```

---

## 📊 Results

### 🎥 Simulation Videos

**Multi-Target Pickup Sequence**
*(Insert your exported animation here)*

```
media/sequential_pickup.mp4
```

**Single-Mass Trajectory (with payload pickup)**

```
media/mass1_pickup.mp4
```

---

### 📈 Controller Performance

| Metric                 | Result                         |
| ---------------------- | ------------------------------ |
| Pick-and-place cycles  | **4 successful**               |
| Total operation time   | **44 seconds**                 |
| Payload mass error     | **57% underestimated**         |
| Tracking recovery time | **≈ 1–2 s after pickup**       |
| Stability              | **Maintained for all targets** |

Tracking error spikes when mass is picked up, then rapidly converges back to zero — a hallmark of sliding-mode robustness .

---

### 📉 Analysis Plots (Placeholders)

```
plots/control_torque.png
plots/tracking_error.png
plots/mass_estimation.png
plots/joint_angles.png
```

These show:

* Torque jumps at pickup
* Tracking error recovery
* True vs estimated mass
* Joint angle evolution

---

## 🧠 Control Model

### Tracking Error

```math
\tilde q = q - q_d
```

### Sliding Surface

```math
s = \dot{\tilde q} + \Lambda \tilde q
```

### Reference Trajectory

```math
\dot q_r = \dot q_d - \Lambda \tilde q
```

### Regressor Form

```math
M(q)\ddot q_r + C(q,\dot q)\dot q_r + G(q) = Y(q,\dot q,\dot q_r,\ddot q_r)\,a
```

### Control Law

```math
\tau = Y(q,\dot q,\dot q_r,\ddot q_r)\,\hat a - K\,\text{sat}(s)
```

Where:

* **Y** is the regressor matrix
* **â** are estimated mass parameters
* **K** is the sliding-mode gain
* **sat(·)** is a boundary-layer saturation function to prevent chattering

This ensures stability even when the true mass changes mid-motion .

---

## ⚙️ Robot Dynamics

The 4-DOF manipulator is modeled using **Euler–Lagrange mechanics**:

```math
M(q)\ddot q + C(q,\dot q)\dot q + G(q) = \tau
```

Where:

* **M(q)** is the mass matrix
* **C(q, q̇)** contains Coriolis & centrifugal terms
* **G(q)** is gravity

These are computed symbolically and numerically solved using Mathematica’s **DAE-aware NDSolve** engine .

---

## 💥 Impact & Bouncing Physics

When payloads are dropped, their bouncing is modeled using:

### Restitution (Normal Direction)

```math
v_n^+ = -e\,v_n^-
```

### Coulomb Friction (Tangential)

```math
J_t = \mu J_n = \mu m |v_n^-|(1+e)
```

The simulation shows:

* **13 bounces before rest**
* **~49% energy retained per bounce**
* **Sliding → sticking transitions** 

---

## 🧪 Why This Matters

This project demonstrates **real robotics problems**:

<table>
<tr>
<td width="50%">

**🤖 Industrial Robotics**

* Pick-and-place with unknown product weights
* Robot arms in factories
* Adaptive manipulation

</td>
<td width="50%">

**🚀 Space Robotics**

* Time-varying mass properties
* Fuel & payload changes
* Nonlinear dynamics

</td>
</tr>
<tr>
<td width="50%">

**🩺 Surgical Robotics**

* Tools & tissue change mass
* Precision under uncertainty

</td>
<td width="50%">

**📦 Logistics Automation**

* Sorting robots
* Uncertain object properties

</td>
</tr>
</table>

---

## 📚 Want the Full Math?

A **graduate-level technical report** is included that derives:

* Robot dynamics
* Sliding-mode stability
* Regressor-based adaptive control
* Impact mechanics
* Performance analysis

📄 `report/Adaptive_Sliding_Mode_4DOF_Report.pdf`

This is written at the level of **Slotine & Li** and **Spong & Vidyasagar**.

---

## 👤 Author

**Gustavo Torres**

[![GitHub](https://img.shields.io/badge/GitHub-gustavotorr-181717?style=flat\&logo=github)](https://github.com/gustavotorr)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Gustavo%20Torres-0077B5?style=flat\&logo=linkedin)](https://linkedin.com/in/gustavo-torres)

---

<div align="center">

**⭐ If you found this project interesting, consider giving it a star!**

*Advanced nonlinear control meets real robotic uncertainty*

</div>
