# Testbot simulation planning

Testbot is being recorded in SquidLink before a digital twin is built. This keeps the physical build record in NautiPi and gives the future ROS 2/Gazebo work a defined home.

## Repository boundary

```text
robot-NautiPi      physical CAD, wiring, measurements, and hardware evidence
robot-CuttleOS     robot profile, application contracts, and runtime services
robot-SquidLink    ROS 2/Gazebo model, simulation controllers, and SiL/HiL tests
```

The Testbot application namespace is `testbot`. SquidLink must use the subjects and semantics from the CuttleOS profile unchanged when a Testbot bridge or simulated interface is implemented.

## ADM133 simulation boundary

The physical ADM133 resource map and Control-driver contract are maintained in [robot-CuttleOS](https://github.com/PhilipMcGaw/robot-CuttleOS/blob/main/docs/adeept-adm133-control-driver.md). SquidLink should simulate the logical results of the adapter, such as wheel motion, camera tilt, battery telemetry, status state, and buzzer requests. It should not invent a second PCA9685 or GPIO allocation. The physical channel reservations remain in the Testbot profile and the robot-NautiPi wiring record.
## Model stages

1. Record the physical robot in NautiPi.
2. Import only approved, simplified CAD into `vehicles/testbot/meshes/`.
3. Create `testbot.urdf.xacro` with frames, joints, visual geometry, collision geometry, and inertial placeholders.
4. Add ROS 2 control and differential-drive behaviour.
5. Add camera and camera-tilt simulation.
6. Add optional sensors only when the corresponding physical module and wiring are known.
7. Add NATS/ROS 2 integration and repeatable scenarios.

No stage should be described as a digital twin until the geometry, physical parameters, and intended behaviours have been compared with the real Testbot.