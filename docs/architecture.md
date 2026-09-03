# SquidLink architecture

SquidLink is the simulation and integration-test environment. It is independent of the robot runtime and physical hardware repositories.

The current project boundary is:

```text
                    Robot project
                         │
          ┌──────────────┼──────────────┐
          │              │              │
       CuttleOS      SquidLink       NautiPi
       runtime      simulation       hardware
          │              │              │
          └──────────────┼──────────────┘
                         │
                  documented interfaces
```

CuttleOS owns Cockpit, Control, and Datalogger. NautiPi owns physical electronics, PCB designs, embedded hardware projects, and hardware reference material. SquidLink owns ROS 2/Gazebo simulation, simulation scenarios, and the NATS/ROS 2 bridge.

## Application boundary

NATS Core is the application-facing communication boundary between the robot services and SquidLink. The bridge translates between the existing application contract and ROS 2 topics without changing the application semantics.

```text
                     CuttleOS
       ┌───────────────┼────────────────┐
       │               │                │
    Cockpit         Control        Datalogger
       │               │                │
       └───────────────┼────────────────┘
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
                       └──────► NATS telemetry
```

Cockpit, Control, and Datalogger MUST NOT become ROS 2 dependencies. The browser also MUST NOT connect directly to NATS or ROS 2; Cockpit remains the web-facing boundary.

## Simulation boundary

The simulator owns vehicle physics, simulated actuators, simulated sensors, simulated cameras, and simulation-specific state. It does not duplicate application safety logic that belongs in CuttleOS Control.

The initial objective is a simple, deterministic command-to-telemetry loop. Physical realism can be increased progressively as the simulation becomes useful for engineering verification.

## Execution modes

SquidLink supports two architectural modes:

- **SiL:** CuttleOS services and SquidLink run as software. No physical robot hardware is required.
- **HiL:** selected real hardware or software components are connected to the simulated vehicle through the existing application interfaces.

The same application-facing NATS contract MUST be used in both modes.

## Environment

The currently documented ROS environment is:

- Ubuntu 24.04 LTS, AMD64
- ROS 2 Jazzy
- Gazebo Harmonic

SquidLink may run in a virtual machine or on dedicated Ubuntu hardware. The host or virtualisation technology is not part of the robot architecture.
