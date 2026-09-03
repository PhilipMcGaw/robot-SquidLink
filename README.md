# ROV HiL and SiL

[![Code: PolyForm Noncommercial 1.0.0](https://img.shields.io/badge/Code-PolyForm_Noncommercial_1.0.0-purple.svg)](LICENSE-POLYFORM-NonCommercial-1.0.0.txt)
[![Documentation: CC BY-NC-SA 4.0](https://img.shields.io/badge/Documentation-CC_BY--NC--SA_4.0-purple.svg)](LICENSE-CC-BY-NC-SA-4.0.txt)
[![ROS 2](https://img.shields.io/badge/Simulation-ROS_2-22314e.svg)](https://www.ros.org/)
[![Gazebo](https://img.shields.io/badge/Simulation-Gazebo-ff6f00.svg)](https://gazebosim.org/)
[![Python](https://img.shields.io/badge/Language-Python-3776ab.svg)](https://www.python.org/)

<p align="center">
  <img src="docs/images/squidlink-architecture.svg" alt="SquidLink simulation architecture" width="900">
</p>

SquidLink is the independent simulation and integration-test environment for
the ROV. It provides the ROS 2 and Gazebo software-in-the-loop environment,
together with the hardware-in-the-loop bridge used to exercise the same
application-facing interfaces before connecting real hardware.

SquidLink works alongside the other ROV projects:

- [CuttleOS](https://github.com/PhilipMcGaw/robot-CuttleOS) runs the Cockpit,
  Control and Datalogger services on the robot Raspberry Pi.
- [NautiPi](https://github.com/PhilipMcGaw/robot-NautiPi) contains the physical
  electronics, hardware and Arduino project material.

The projects remain separate. SquidLink must use the same NATS contracts as the
real ROV, but it must not become the source of truth for the physical robot or
replace the CuttleOS runtime services.

## People who have helped

- Philip 'Skippy' McGaw - <philip@mcgaw.eu> - [philipmcgaw.com](https://philipmcgaw.com)
- Tamarisk 'NotQuiteHere' McGaw - <tamarisk@mcgaw.eu> - [tamarisk.it](https://tamarisk.it)
- Bob 'thinkl33t' Clough - <bob@clough.me> - [thinkl33t.co.uk](https://thinkl33t.co.uk)

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
