# NATS contract

NATS Core is the live application communication layer used by CuttleOS and by SquidLink when it integrates with the robot application boundary.

SquidLink MUST use the same application-facing contract as the real robot. It MUST NOT invent a second subject taxonomy simply because the consumer or producer is simulated.

## Subject naming

Subjects are namespaced and are defined by the current CuttleOS application contract and robot profiles. Examples of the current logical form include:

```text
<namespace>.sensor.water.depth
<namespace>.command.sound.play
```

These examples illustrate the current namespaced structure. The exact subject set, payload schema, units, ranges, and update rates are authoritative in the CuttleOS implementation and profile documentation. This document is not a second source of truth for those definitions.

Where a CuttleOS profile or service defines a subject, SquidLink shall consume or publish that subject unchanged when implementing the corresponding simulation interface.

## Payloads and units

Every simulated value MUST use the same meaning, unit, scale, and expected update behaviour as its real application counterpart. Simulation-specific representations may exist internally within ROS 2, but the NATS boundary MUST preserve the application contract.

For example, a simulated depth value exposed through the application interface must represent the same physical quantity and unit as the corresponding real-robot telemetry value.

## Recording

NATS Core is a live transport. NATS JetStream is not part of the robot architecture.

Persistent operational and black-box recording belongs to the Datalogger. SquidLink may record simulation-specific evidence, such as ROS bags or test output, where required for validation, but this does not replace the robot's application-level recording path.

## Contract changes

A change to an application-facing subject, payload, unit, range, or update rate is an interface change. The change MUST be reflected in the authoritative CuttleOS documentation and in every affected consumer or producer, including SquidLink where applicable.

SquidLink documentation MUST NOT preserve an obsolete example merely because an older simulation used it.
