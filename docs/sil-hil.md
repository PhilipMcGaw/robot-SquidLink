# SquidLink SiL and HiL Architecture

> **Status:** Authoritative for SquidLink simulation and integration-test usage.
>
> Cross-repository application contracts are authoritative in CuttleOS.

## 1. Definitions

### Software-in-the-Loop (SiL)

SiL exercises CuttleOS software against a simulated vehicle without requiring
physical robot hardware.

```text
CuttleOS
   ↓
 NATS
   ↓
NATS/ROS 2 bridge
   ↓
 ROS 2
   ↓
Gazebo
   ↓
Simulated vehicle
```

### Hardware-in-the-Loop (HiL)

HiL introduces selected real hardware or software components into the simulation
loop so that their real interfaces can be exercised against the simulated
vehicle.

HiL is therefore an integration-test configuration, not merely a more detailed
Gazebo simulation.

## 2. Common application boundary

SiL and HiL MUST use the same CuttleOS application-facing NATS contracts as the
real robot.

A simulation-only application protocol MUST NOT be introduced merely to simplify
the simulation.

The boundary is:

```text
CuttleOS application contract
           ↓
         NATS
           ↓
    SquidLink bridge
           ↓
         ROS 2
           ↓
        Gazebo
```

Real hardware introduced for HiL connects through the appropriate hardware
interface rather than changing the application contract.

## 3. Safety boundary

SquidLink MUST NOT become the authority for production Control safety logic.
Simulation may model safety-relevant behaviour for testing, but production safety
policy remains in CuttleOS Control and the appropriate hardware layer.

A successful SiL or HiL scenario does not establish physical production
validation without the corresponding physical evidence.

## 4. Simulation fidelity

Simulation should become progressively more realistic only where the additional
fidelity improves a useful engineering test.

Potential model elements include:

- mass and inertia;
- buoyancy;
- drag;
- thruster characteristics;
- actuator response;
- environmental effects;
- sensor noise;
- camera placement and characteristics.

Repeatability and deterministic test behaviour are preferred over unnecessary
model complexity.

## 5. Scenario requirements

A repeatable scenario should define, where applicable:

- vehicle/model selection;
- initial conditions;
- commands or operator actions;
- expected physical response;
- expected telemetry;
- timing constraints;
- pass/fail criteria;
- software, model, and configuration revisions.

Scenarios SHOULD support headless execution where practical so they can become
part of automated integration testing.

## 6. Evidence classification

Simulation evidence should be labelled explicitly as simulation or integration
test evidence.

The project uses the following distinct states:

```text
Designed
  ↓
Implemented
  ↓
Software-tested / Simulated
  ↓
Bench-tested
  ↓
Commissioned
  ↓
Production-validated
```

The sequence is illustrative rather than mandatory for every engineering
activity. Evidence must state what was actually tested.
