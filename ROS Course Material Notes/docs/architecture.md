# HiL/SiL course architecture

This document explains the current repository boundaries for the ROS 2/Gazebo
course exercises. The maintained architectural source for SquidLink is
[`docs/architecture.md`](../../docs/architecture.md); this note is instructional
material and must not be treated as the application or hardware source of truth.

## Repository boundaries

```text
CuttleOS
└── Robot-side application services: Cockpit, Control, and Datalogger

SquidLink
├── ROS 2/Gazebo simulation
├── NATS/ROS 2 bridge
├── vehicle models and simulated sensors
├── scenarios and simulation tests
└── course material

NautiPi
└── Physical electronics, PCB designs, embedded hardware, and hardware records
```

A repository may document an interface to another repository, but it must not
become the source of truth for that repository's implementation.

## Application and simulation boundary

```text
CuttleOS services ── NATS Core ── NATS/ROS 2 bridge ── ROS 2 ── Gazebo
       ▲                                                        │
       └────────────── NATS telemetry and state ────────────────┘

CuttleOS Cockpit ── approved camera/video interface ── simulated camera
```

NATS Core is the application-facing communication boundary. The bridge adapts
existing application contracts to ROS 2 topics and Gazebo interfaces without
changing their semantics. ROS 2 and Gazebo remain internal to SquidLink.

Camera and video are separate from NATS application messaging. The simulated
camera must be exposed through the same approved presentation boundary used by
the Cockpit integration; the browser must not connect directly to NATS or ROS 2.

## SiL

Software-in-the-Loop runs CuttleOS services and SquidLink as software. No
physical robot or Raspberry Pi is required. The SquidLink workspace and Gazebo
simulation must remain usable offline and independently of physical hardware.

CuttleOS services may run on separate development hosts or virtual machines.
The course material should keep the basic ROS 2/Gazebo exercises local until the
simulation loop is repeatable, then introduce NATS and the bridge.

## HiL

Hardware-in-the-Loop connects selected real hardware or software components to
the simulated vehicle through the existing application-facing NATS contracts.
HiL does not justify introducing a simulation-only application protocol, and it
does not constitute physical or production validation.

```text
Selected real component ── existing application interface ── NATS/ROS 2 bridge
                                                               │
                                                           ROS 2/Gazebo
```

A Raspberry Pi is a later HiL endpoint, not a prerequisite for SiL or for the
course exercises.

## Communication rules

- Use NATS Core for live application commands, telemetry, state, and status.
- Do not introduce NATS JetStream into the robot architecture.
- Keep camera/video transport outside NATS application messaging.
- Keep ROS 2 and Gazebo behind the SquidLink bridge boundary.
- Do not make Cockpit, Control, or Datalogger directly dependent on ROS 2.
- Keep persistent operational and black-box recording under the CuttleOS
  Datalogger responsibility; SquidLink may create simulation-specific evidence
  such as ROS bags and test output.
- Use the current CuttleOS subjects, payloads, units, ranges, and timing rules;
  do not create a conflicting course-specific contract.

## Simulation philosophy

Start with simple, deterministic models and a measurable command-to-telemetry
loop. Add vehicle dynamics, buoyancy, drag, thruster response, sensor noise,
camera fidelity, and environmental effects progressively. Simulation success
must be reported separately from automated-test, bench-test, and
production-validation evidence.

## Status

- Implemented: current SquidLink/CuttleOS/NautiPi boundaries and SiL/HiL
  teaching boundary documented.
- Automated-test verification: this course note is covered by documentation
  checks; the architecture it describes is not fully automated-test verified.
- Bench-tested: not applicable to this course note.
- Production-validated: not applicable to this course note.
- Planned or unverified: complete production-quality bridge, vehicle scenarios,
  and physical HiL commissioning remain dependent on explicit evidence.