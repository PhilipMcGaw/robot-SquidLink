# Testbot simulation framework

This directory reserves the vehicle-specific simulation space for Testbot, the small differential-drive Raspberry Pi robot described in [robot-NautiPi `TestBot - Main Body`](https://github.com/PhilipMcGaw/robot-NautiPi/tree/main/TestBot%20-%20Main%20Body).

This is a framework and integration boundary, not yet a digital twin. The physical design, CAD, wiring, measurements, and validation evidence must be established in NautiPi first. This directory can then receive the stable geometry and measurements needed for a ROS 2/Gazebo model.

## Planned structure

```text
vehicles/testbot/
├── README.md
├── urdf/       # Testbot Xacro and generated URDF, once the geometry is stable
├── meshes/     # Simulation meshes derived from approved NautiPi CAD
├── config/     # ROS 2 controllers and simulation parameters
├── launch/     # Testbot visualisation and simulation launch files
└── worlds/     # Testbot-specific Gazebo worlds, if required
```

## Intended first model

The first model should represent only the confirmed physical arrangement:

- `base_link` for the chassis;
- `left_wheel_link` and `right_wheel_link` for the two M1/M2 motors;
- a differential-drive controller;
- `camera_mount_link` and `camera_link` connected by the camera-tilt joint;
- a Pi Camera sensor placeholder;
- simulated battery and network-status interfaces only where they are useful to an integration test.

The ADM133 is a physical hardware adapter. Gazebo should model the resulting robot behaviour and logical CuttleOS contract, not pretend to simulate every GPIO or PCA9685 electrical detail. The physical PCA9685 reservations remain documented in CuttleOS and NautiPi.

## Source and status

- Physical source: [robot-NautiPi TestBot - Main Body](https://github.com/PhilipMcGaw/robot-NautiPi/tree/main/TestBot%20-%20Main%20Body).
- Software contract: [CuttleOS Testbot profile](https://github.com/PhilipMcGaw/robot-CuttleOS/blob/main/configs/profiles/testbot.json).
- Simulation boundary: SquidLink.
- Current status: framework planned; no Testbot digital-twin model or simulation validation is claimed.

## Entry criteria for the digital twin

Before adding a full model here, record in NautiPi:

- stable chassis CAD and mounting points;
- wheel diameter, width, and separation;
- total mass and an approximate centre of mass;
- Pi Camera position and camera-servo limits;
- motor polarity and differential-drive arrangement;
- confirmed battery and power arrangement;
- ADM133 channel reservations and tested interfaces;
- a licence decision for any imported or derivative CAD.

When those inputs are available, create the Xacro model first, validate its frames and visual geometry in RViz2, then add Gazebo physics and controllers. Keep each stage separately identified as planned, automated-test verified, bench-tested, or production-validated.