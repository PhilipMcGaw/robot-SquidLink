# SquidLink current status

## Implemented

The repository currently provides the documented project boundary and supporting documentation for an independent ROS 2/Gazebo simulation and integration-test environment.

The repository includes:

- an authoritative ROS 2 workspace under `ros2_ws/`, including the provisional `testbot_description` working example;
- simulation configuration under `configs/`;
- vehicle-specific simulation space under `vehicles/`, including the initial Testbot planning boundary;
- scenario and integration-test space under `scenarios/`;
- documentation covering the simulation architecture, NATS/ROS 2 boundary, Gazebo, testing, logging, camera integration, timing, and safety;
- ROS course material for development and training;
- documentation currency checks under `tests/`.

## Automated-test verification

`tests/test_documentation.py` provides the documentation currency audit. The repository documentation also defines a pull-request documentation policy under `tests/documentation_change_policy.py` and its associated configuration.

The existence of these checks is not evidence that the simulated vehicle or HiL path has passed an end-to-end engineering test.

## Bench-tested

No bench-test result is claimed by this status document. Bench-test evidence must be recorded explicitly in the relevant test documentation.

## Production-validated

No physical ROV or production validation is claimed.

A successful ROS 2/Gazebo simulation, automated scenario, or software integration test does not constitute physical or production validation.

## Planned or unverified

The following remain subject to implementation and explicit evidence:

- complete ROV vehicle simulation;
- complete Testbot vehicle simulation or digital-twin validation;
- production-quality NATS/ROS 2 bridge execution;
- repeatable end-to-end scenarios;
- sufficiently realistic vehicle physics for the intended verification purpose;
- complete simulated sensor and camera behaviour;
- hardware-in-the-loop integration with real components;
- physical validation against the ROV hardware.

The current repository state should therefore be treated as simulation infrastructure and development scaffolding rather than a validated digital twin of the physical ROV.

## References

- `MASTER_CONTEXT.md`
- `docs/documentation-policy.md`
- `docs/architecture.md`
- `docs/automated-scenario-testing.md`
- `ros2_ws/`
- `configs/`
- `vehicles/`
- `scenarios/`
- `tests/`


Documentation audit reference: docs/scenario-testing.md
