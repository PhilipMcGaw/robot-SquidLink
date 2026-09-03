# SquidLink Master Context

Interactive ROS command examples in this repository assume Zsh. Use the ROS `setup.zsh` and workspace `local_setup.zsh` files where available. Executable scripts may use another interpreter only when declared by their shebang and documented accordingly.

## 1. Purpose

SquidLink is the independent simulation and integration-test environment for the robot projects. Its primary purpose is to provide a Software-in-the-Loop (SiL) environment using ROS 2 and Gazebo, together with a Hardware-in-the-Loop (HiL) capability for exercising real components against a simulated vehicle.

SquidLink is not the robot runtime, and it is not the physical hardware repository. It must remain usable without the physical robot.

## 2. Repository boundaries

The current project is organised around three repositories:

| Repository | Responsibility |
|---|---|
| **CuttleOS** | Robot-side software, including Cockpit, Control, and Datalogger |
| **SquidLink** | ROS 2/Gazebo simulation, HiL/SiL integration, scenarios, and simulation-side testing |
| **NautiPi** | Physical electronics, PCB designs, embedded hardware projects, and hardware reference material |

The repositories are separate. A repository may describe an interface to another repository, but it must not become the source of truth for another repository's implementation.

CuttleOS owns the application-facing software contracts. NautiPi owns physical hardware design information. SquidLink owns simulation-specific implementation and test infrastructure.

## 3. Software-in-the-Loop

SiL operates the application against a simulated vehicle without requiring physical robot hardware.

The intended application boundary is:

```text
CuttleOS services
      │
   NATS Core
      │
NATS/ROS 2 bridge
      │
    ROS 2
      │
   Gazebo
      │
Simulated vehicle
      │
Simulated sensors
      │
   NATS Core
```

CuttleOS services may be run separately in the development environment. SquidLink provides the simulated vehicle and ROS 2/Gazebo environment.

SiL is intended to support repeatable, offline development and integration testing.

## 4. Hardware-in-the-Loop

HiL allows selected real hardware or software components to operate against the simulated vehicle.

HiL MUST use the same application-facing NATS contracts as the real robot. A simulation-specific application protocol must not be introduced merely to simplify integration.

HiL capability is not evidence of physical or production validation. Any physical validation must have explicit test evidence.

## 5. Application responsibilities

### CuttleOS

CuttleOS owns:

- Cockpit and the operator-facing web interface;
- Control and safety-critical vehicle control;
- Datalogger and persistent operational recording;
- robot profiles and application-facing contracts.

SquidLink must not duplicate Control safety logic merely to make a simulation work.

### SquidLink

SquidLink owns:

- ROS 2 packages used by the simulation;
- Gazebo vehicle simulation;
- the NATS/ROS 2 bridge;
- simulated sensors and actuators;
- simulated camera sources where required;
- repeatable scenarios;
- simulation and integration-test evidence.

### NautiPi

NautiPi owns the physical electronics and hardware design record. SquidLink may use hardware information from NautiPi when constructing a simulation, but the simulation does not replace physical design verification or commissioning.

## 6. NATS

NATS Core is the internal live communication mechanism at the application boundary.

NATS is used for commands, telemetry, sensor data, actuator state, status, and other inter-component messages as defined by CuttleOS.

NATS JetStream is **not part of the robot architecture**.

Persistent operational and black-box recording belongs to the Datalogger. SquidLink may produce simulation-specific recordings, such as ROS bags or test output, as engineering evidence.

### Subject naming

NATS subjects are namespaced and defined by the current CuttleOS application contract and robot profiles. SquidLink MUST use those subjects unchanged at the application boundary.

Examples of the current logical form include:

```text
<namespace>.sensor.water.depth
<namespace>.command.sound.play
```

The exact subject set, payload schema, units, ranges, and update rates are authoritative in CuttleOS. This repository must not maintain a conflicting independent subject catalogue.

## 7. ROS 2 and Gazebo boundary

ROS 2 and Gazebo are internal implementation technologies of SquidLink.

CuttleOS services MUST NOT become directly dependent on ROS 2 merely because they are tested through SquidLink. The bridge adapts the application contract to ROS 2 topics and simulation interfaces.

The currently documented environment is:

- Ubuntu 24.04 LTS, AMD64
- ROS 2 Jazzy
- Gazebo Harmonic

The environment may run in a virtual machine or on dedicated Ubuntu hardware. The host virtualisation technology is not part of the robot architecture.

## 8. Simulation model

The initial objective is a simple, understandable, deterministic, and repeatable command-to-telemetry loop:

```text
Application command
        ↓
     NATS Core
        ↓
   HiL/SiL bridge
        ↓
       ROS 2
        ↓
      Gazebo
        ↓
Simulated vehicle response
        ↓
 Simulated sensors
        ↓
     NATS Core
        ↓
    Telemetry
```

Physical realism should be increased progressively. Future simulation fidelity may include mass and inertia, buoyancy, drag, thruster characteristics, actuator response, environmental effects, sensor noise, and other vehicle dynamics.

Simulation complexity must not be increased at the expense of a reliable and repeatable integration loop.

## 9. Vehicle organisation

Vehicle-specific simulation content belongs under:

```text
vehicles/
```

Generic simulation infrastructure must remain separate from vehicle-specific models.

A vehicle model may include hull geometry, mass and inertia, buoyancy, thrusters, actuator characteristics, sensors, camera placement, and vehicle-specific ROS 2/Gazebo configuration.

## 10. Repository layout

```text
SquidLink/
├── configs/
├── docs/
├── vehicles/
├── ros2_ws/
│   └── src/
├── scenarios/
├── scripts/
├── tests/
├── ROS Course Material Notes/
├── CONTRIBUTING.md
├── LICENSES.md
├── MASTER_CONTEXT.md
└── README.md
```

`ros2_ws/` is the authoritative ROS 2 colcon workspace. ROS 2 packages belong under `ros2_ws/src/`.

Generated colcon directories belong under `ros2_ws/build/`, `ros2_ws/install/`, and `ros2_ws/log/`, and MUST NOT be committed.

## 11. Scenarios and testing

Scenarios should be reproducible and, where practical, executable headlessly. A scenario may define initial conditions, vehicle selection, commands, expected vehicle response, expected telemetry, and pass/fail checks.

Simulation, automated-test, bench-test, and production-validation evidence are distinct. A passing simulation test does not constitute physical validation.

## 12. Headless operation

The simulation infrastructure SHOULD support headless operation so that repeatable scenarios can eventually run without the Gazebo graphical interface.

Gazebo and RViz2 remain useful for development, debugging, and visual inspection.

## 13. Current status

SquidLink currently provides the repository structure, ROS 2/Gazebo integration boundary, documentation framework, course material, and initial simulation scaffolding. The complete ROV simulation, production-quality NATS/ROS 2 bridge, repeatable vehicle scenarios, and physical HiL validation remain unverified unless explicit evidence is recorded elsewhere in the repository.

The ROS course material is learning and setup material, not authoritative architecture documentation.

## 14. Documentation policy

The enforceable documentation policy is:

```text
docs/documentation-policy.md
```

Documentation is an engineering deliverable. Behaviour-affecting changes MUST update the relevant documentation in the same change. `MASTER_CONTEXT.md` MUST be updated when architecture, repository boundaries, operating conventions, or validation status changes.

Documentation MUST distinguish implemented, automated-test verified, bench-tested, production-validated, and planned or unverified behaviour.

Technical writing follows the repository documentation policy, including formal British English, Oxford commas, NIST SI conventions, non-breaking spaces between numerical values and units, correct Unicode symbols, precise section references using `§`, and deliberate RFC 2119 normative language.

## 15. Engineering principles

1. **Separate application intent from hardware implementation.**
2. **Keep the application-facing NATS contract common to real and simulated systems.**
3. **Keep ROS 2 and Gazebo behind the SquidLink boundary.**
4. **Do not duplicate safety-critical Control logic in the simulator.**
5. **Use simple, repeatable simulation before adding physical complexity.**
6. **Use explicit units and scales for every physical quantity.**
7. **Record sufficient software, configuration, model, and test information to reproduce results.**
8. **Never describe simulation as physical or production validation.**

## 16. Maintainer

**Name:** Philip McGaw

**Email:** philip@mcgaw.eu

**Website:** https://philipmcgaw.com
