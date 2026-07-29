# Mass-Dependent Bouncing-Ball Physics Simulator

Four balls are thrown with identical velocity. The heavier one stops short. This simulation shows why: gravity treats all masses the same in flight, but friction during impact doesn't.

https://github.com/user-attachments/assets/9c2857ed-fe06-41ed-be3e-349a7bf6ae1a

## The result

| Ball | Mass | Friction μ | Final position |
|---|---|---|---|
| 1 (red) | 0.7 kg | 0.09 | 2.51 m |
| 2 (green) | 0.7 kg | 0.09 | 3.51 m |
| 3 (blue) | 0.7 kg | 0.09 | 4.51 m |
| 4 (magenta) | 1.5 kg | 0.15 | 4.23 m |

Despite being 114% heavier, ball 4 travels less far than ball 3 from the same throw — its higher effective friction bleeds horizontal velocity at every bounce. Each ball goes through about 12 bounces before coming to rest, retaining roughly 47–49% of its energy per impact.

## The model

Between bounces, plain projectile motion:

```math
x(t) = x_0 + v_x t, \qquad y(t) = y_0 + v_y t - \tfrac{1}{2} g t^2
```

At each impact, an impulse-based collision update. The vertical velocity reverses with restitution e = 0.7:

```math
v_y^+ = -e\, v_y^-
```

and a Coulomb friction impulse acts horizontally:

```math
J_t = \mu J_n = \mu\, m\, |v_y^-| (1+e), \qquad \Delta v_x = J_t / m
```

The mass cancels in the velocity change — which is exactly the classic textbook result. The mass-dependence enters through μ: in practice heavier objects often grip harder on impact, and the simulation models that (μ = 0.15 for the heavy ball vs 0.09 for the light ones). The impact routine also checks a sliding-versus-sticking condition: if the friction impulse is large enough to consume all horizontal velocity, the ball sticks rather than being pushed backwards.

Impact times are found exactly by root-finding rather than fixed-step collision checks, so no bounce is missed or smeared.

## Setup

All four balls launch from y = 2.5 m with vₓ = 2.5 m/s and v_y = 1.0 m/s, spaced 1 m apart horizontally. Gravity 9.81 m/s².

## Running it

Open `Code/Bouncing-Ball-Physics-Simulator.nb` in Wolfram Mathematica (12.0+, no extra packages). A PDF export of the notebook is in the same folder if you just want to read it.

## Author

Gustavo Torres — [GitHub](https://github.com/gustavotorr) · [LinkedIn](https://www.linkedin.com/in/gustavo-torres111/)

MIT license.
