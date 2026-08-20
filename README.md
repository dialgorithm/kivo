# kivo

![kivo 3d view](content/3dview1.png)

**kivo** is a vision-guided robotic arm designed to autonomously identify objects, calculate where they are, and move a gripper to pick and place them. The system combines on-device YOLO object detection, inverse kinematics, and multi-channel servo control into a compact three-layer architecture.

it features:

- 6-DOF robotic arm for autonomous pick-and-place tasks
- On-device YOLO object detection using the Grove Vision AI Module V2
- OV5647 camera for visual target detection
- Arduino UNO R4 WiFi for inverse kinematics and motion sequencing

## how it works

see ==> calculate ==> move

### perception (see)

the **ov5647** captures the space and feeds frames to the **grove vision ai module v2**, where yolo runs on-device to detect the target and estimate its location

### control (calculate)

the **arduino uno r4 wifi** receives the target position, solves the arm's inverse kinematics, and converts the result into joint angles and a motion sequence

### actuation (move)

the **16-channel servo driver** generates pwm signals for the arm's 6 joints and gripper, using **4× mg996r** and **3× mg90s** servos

### wiring

![wiring](content/wiring.png)

## cad

final 3d mockup in fusion

|                                   |                                   |                                   |
| --------------------------------- | --------------------------------- | --------------------------------- |
| ![3d view 1](content/3dview1.png) | ![3d view 2](content/3dview2.png) | ![3d view 3](content/3dview3.png) |

stl files are in [`cad/stl`](cad/stl)

## bom

full parts list is in [`bom.csv`](bom.csv)
