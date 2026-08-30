# ROV HiL and SiL

Simulation and integration-test environment for the ROV project.

This repository contains the ROS 2/Gazebo software-in-the-loop environment and the hardware-in-the-loop bridge used to test the same Cockpit and Control interfaces before connecting real hardware. It always runs independently in its own virtual machine or on dedicated physical hardware.

It does not share or require the Python runtimes, virtual environments, or Windows portable-Python support used by the Cockpit, Control, and Datalogger repositories.

## Architecture

```text
Cockpit ── NATS Core ── Control ── NATS Core ── HiL bridge ── ROS 2 ── Gazebo
                                      ▲                 │
                                      └── telemetry ────┘
```

The simulation must remain behind the same application-facing NATS Core contract as the real ROV. ROS 2 and Gazebo are implementation details of the simulation environment.

## Repository layout

- `ros2_ws/` — colcon workspace and ROS packages
- `configs/` — simulator and bridge configuration
- `scenarios/` — repeatable test scenarios and expected outcomes
- `tests/` — integration and scenario tests
- `docs/` — environment and simulation documentation
- `scripts/` — utility scripts for setup and testing
- `vehicles/` — vehicle-specific simulation content
- `ROS Course Material Notes/` — retained learning and setup notes

## Key project files

- `MASTER_CONTEXT.md` — comprehensive architectural and engineering context
- `CONTRIBUTING.md` — contributor guidance and development practices
- `LICENSES.md` — licensing information

## Current status

The repository is scaffolded with initial ROS packages and Gazebo model structure. See [docs/README.md](docs/README.md) for detailed documentation.
