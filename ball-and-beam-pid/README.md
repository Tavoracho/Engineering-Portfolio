# Ball-and-Beam PID Controller

<p align="center">
  <a href="https://youtu.be/9JrFDMzFvnI">
    <img src="Media/Photos/ball%20%26%20beam%20pid%20robot.png" width="85%">
  </a>
</p>

<p align="center"><i>Click the image for the demo video.</i></p>

A real-time control system that balances a ball on a beam. A VL53L0X time-of-flight sensor measures the ball's position, a PID loop computes the correction, and a servo tilts the beam to hold the setpoint. Gains are tuned live from an onboard joystick with an OLED dashboard — no computer needed once it's flashed.

## How it works

The control loop runs every 35 ms (about 28.6 Hz):

1. Read the distance sensor
2. Validate and filter the reading (7-sample moving average, jump rejection)
3. Compute the PID output
4. Constrain and apply the servo command
5. Update statistics

With default tuning it settles in 2–4 seconds and holds steady-state error under 1 cm.

A few implementation details that mattered in practice:

- **Jump rejection** — readings that change more than 6 cm between samples are discarded; the ToF sensor occasionally glitches
- **Derivative filtering** — a low-pass filter (α = 0.6) on the D term keeps sensor noise out of the servo
- **Integral windup protection** — the I term is clamped so a saturated servo doesn't wind up a huge correction
- **Range validation and timeout handling** — out-of-range readings and I2C failures are handled instead of fed into the loop

## Hardware

| Component | Part |
|---|---|
| Microcontroller | Arduino Mega (or compatible) |
| Distance sensor | VL53L0X time-of-flight |
| Actuator | standard hobby servo |
| Display | SSD1306 OLED, 128×64 |
| Input | 2-axis analog joystick + push button |

Beam length is 32.2 cm with a valid sensing range of 1.5–33.7 cm. The sensor mounts at one end of the beam pointing along it; the servo tilts the beam from below. Servo flat position is 112° (adjust for your servo), with travel limited to 25°–180°.

### Wiring

```
VL53L0X:            SDA → 20, SCL → 21, VCC → 5V, GND → GND
SSD1306 OLED:       SDA → 20, SCL → 21, VCC → 5V, GND → GND
Servo:              signal → 30, power → 5V (external supply for high-torque servos)
Joystick:           VRx → A0, VRy → A1, SW → 5 (internal pullup)
Enable button:      pin 4 (internal pullup) → GND
```

If the sensor fails intermittently, power the servo separately — servo current spikes brown out the I2C bus.

## Software

Libraries (via the Arduino Library Manager): `Servo` and `Wire` (built-in), `Adafruit_GFX`, `Adafruit_SSD1306`, and Pololu's `VL53L0X`.

Open `Code/Ball_On_Beam_PID.ino` in the Arduino IDE and upload.

## Using it

Power on, then press the enable button to start control; press it again to pause. The OLED shows status, measured and target distance, and a visual error bar.

Press the joystick button to open the menu, move up/down to navigate, and press to edit a value (left/right to adjust, press again to save):

| Setting | Step |
|---|---|
| Kp | ±0.1 |
| Ki | ±0.01 |
| Kd | ±0.1 |
| Target | ±0.5 cm |

`RESET` restores the defaults: Kp = 6.2, Ki = 0.15, Kd = 3.1, target = 18.0 cm, with a 1.0 cm deadband.

### Tuning notes

Start from the defaults and adjust one gain at a time. If the ball oscillates, Kp is too high. If it responds sluggishly, raise Kp or lower Kd. If it consistently rests off-target, raise Ki — carefully, since too much causes slow oscillation. The serial monitor (9600 baud) streams the PID terms every 5 loops and prints error statistics every 500, which makes it easy to see which term is misbehaving.

### Troubleshooting

| Symptom | Likely cause |
|---|---|
| "SENSOR FAILED!" at startup | wiring or I2C address issue |
| Wild oscillation | Kp too high |
| Slow response | Kp too low or Kd too high |
| Persistent offset | Ki too low |
| "! SAT" on the display | servo output saturated — check mechanics or reduce gains |
| Intermittent sensor failures | servo drawing down the 5V rail; use external power |

## Author

Gustavo Torres — [GitHub](https://github.com/gustavotorr) · [LinkedIn](https://www.linkedin.com/in/gustavo-torres111/)

MIT license. Thanks to Pololu for the VL53L0X library and Adafruit for the display libraries.
