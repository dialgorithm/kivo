# kivo

arduino uno r4 and grove vision ai v2 powered 6 dof pick and place robotic arm

## design

the robot is split into three functional layers:

| layer          | hardware                                  | job                                                                                 |
| -------------- | ----------------------------------------- | ----------------------------------------------------------------------------------- |
| **perception** | grove vision ai module v2 + ov5647 camera | detects/locates the target object, runs the ml model on-device                      |
| **control**    | arduino uno r4 wifi                       | reads detection results, runs inverse kinematics / motion sequencing, drives servos |
| **actuation**  | 16-ch servo driver + 4× mg996r + 3× mg90s | physically moves the 6 joints + gripper                                             |

power is handled independently of signal wiring: a 2s lipo feeds a ubec that steps down to a servo-safe voltage, isolated from the low-current logic side.

```
                     ┌─────────────────────┐
   OV5647 Camera ───►│ Grove Vision AI V2   │
   (CSI ribbon)       │ (Cortex-M55+Ethos-U55)│
                     └──────────┬───────────┘
                                │ I2C (SDA/SCL) or UART
                                ▼
                     ┌─────────────────────┐
                     │  Arduino Uno R4 WiFi │
                     │  (motion planner)    │
                     └──────────┬───────────┘
                                │ I2C (PWM commands)
                                ▼
                     ┌─────────────────────┐
                     │ 16-ch Servo Driver   │
                     └──┬──┬──┬──┬──┬──┬───┘
                        ▼  ▼  ▼  ▼  ▼  ▼
                     J1 J2 J3 J4 J5 J6/Gripper
                     (4× MG996R, 3× MG90S)

   POWER RAIL (separate from signal wiring):
   2S LiPo 7.4V → Fuse (10A) → UBEC (6V, 8-10A) → Servo Driver V+ rail
                                                  → (Arduino Vin, see §4)
```

## CAD

this is the final 3d mockup of the arm.

|                                 |                                 |                                 |
| ------------------------------- | ------------------------------- | ------------------------------- |
| ![3dview1](content/3dview1.png) | ![3dview2](content/3dview2.png) | ![3dview3](content/3dview3.png) |

stl files are in the `CAD` folder.

## BOM

click [here](bom.csv) to view the bill of materials.
