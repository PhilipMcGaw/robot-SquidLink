# Day 10 — Testbot: physical build to ROS 2 description

## Objective

Use Testbot as the working example for turning a physical robot record into a ROS 2 robot description. Begin with simple primitive geometry, then replace provisional values with measured dimensions and approved CAD from robot-NautiPi.

This lesson creates a useful ROS 2 model for learning and visualisation. It does not yet claim a validated digital twin or a Gazebo physics model.

## Architecture boundary

- robot-NautiPi owns Testbot’s physical CAD, wiring, measurements, and hardware evidence.
- robot-CuttleOS owns the `testbot` profile and application-facing commands and telemetry.
- robot-SquidLink owns this ROS 2 description, later Gazebo behaviour, and simulation tests.

## Prerequisites

Complete Days 0–5 and have the SquidLink ROS 2 workspace available. The target environment is Ubuntu 24.04 LTS with ROS 2 Jazzy and Gazebo Harmonic. The lesson uses ZSH, as do the repository build and test scripts.

## 1. Source the ROS environment

```zsh
source /opt/ros/jazzy/setup.zsh
cd "$HOME/robots/robot-SquidLink/ros2_ws"
```

Use the actual repository path if it differs. Run `colcon` from the workspace root, not from `src/`.

## 2. Inspect the Testbot package

```zsh
colcon list | grep testbot_description
find src/testbot_description -maxdepth 3 -type f | sort
```

The package contains:

- `urdf/testbot.urdf.xacro` — the authoritative model source;
- `launch/display_testbot.launch.py` — the visualisation launch file; and
- `README.md` — the provisional-model status and boundary.

## 3. Build the package

```zsh
colcon build --symlink-install --packages-select testbot_description
source install/setup.zsh
```

Expected result: the package builds without errors and is available to ROS 2.

## 4. Expand and validate the Xacro

```zsh
ros2 run xacro xacro \
  src/testbot_description/urdf/testbot.urdf.xacro \
  > /tmp/testbot.urdf
```

Inspect the generated model:

```zsh
grep -E '<link |<joint ' /tmp/testbot.urdf
```

Expected frames and joints include `base_link`, `left_wheel`, `right_wheel`, `camera_mount_link`, `camera_link`, and `camera_tilt_joint`.

## 5. View the model in RViz2

```zsh
ros2 launch testbot_description display_testbot.launch.py
```

In RViz2, add or confirm:

- `RobotModel` with description source `/robot_description`;
- `TF`; and
- `JointState` if available in the selected display configuration.

Move the camera-tilt joint in the joint-state publisher window. Confirm that the camera link rotates about the intended axis and that the wheels are positioned on opposite sides of the chassis.

## 6. Replace provisional values carefully

Do not edit dimensions from memory. Take the measurements from [`robot-NautiPi TestBot - Main Body`](https://github.com/PhilipMcGaw/robot-NautiPi/tree/main/TestBot%20-%20Main%20Body):

- chassis length, width, and height;
- wheel radius, width, and separation;
- camera and servo pivot position;
- total mass and centre of mass; and
- camera-tilt limits.

Replace the primitive visual and collision geometry with approved simplified meshes only after the physical CAD and licence position are clear. Use separate simplified collision meshes where possible.

## 7. What this lesson proves

- the Testbot description package can be discovered and built;
- Xacro expands into a valid URDF;
- the initial Testbot frame and joint structure can be visualised; and
- the physical-to-simulation source boundary is understood.

## 8. What this lesson does not prove

- that Testbot dimensions, mass, inertia, or joint limits are accurate;
- that Gazebo physics is stable;
- that the ADM133 or future RS485 motor controller is simulated;
- that the camera stream is available; or
- that the model is a validated digital twin.

## Next lesson

The next stage should add Gazebo Harmonic physics and differential-drive control, using the same `left_wheel`, `right_wheel`, and `camera_tilt_joint` names. Add the CuttleOS NATS bridge only after the local ROS 2/Gazebo loop is repeatable.