# Vehicle-specific simulation content

This directory contains vehicle-specific simulation content for different robot types.

Each vehicle subdirectory should contain:
- Hull geometry
- Mass and inertia properties
- Buoyancy characteristics
- Thruster configurations
- Actuator characteristics
- Propulsion configurations
- Sensor models and placements
- Camera placements
- Vehicle-specific ROS 2/Gazebo configuration

## Purpose

Vehicle-specific simulation content is separated from generic simulation infrastructure to support multiple simulated vehicles without requiring redesign of the generic HiL/SiL infrastructure.

## Structure

```
vehicles/
├── <vehicle-name>/
│   ├── urdf/
│   ├── meshes/
│   ├── configs/
│   └── README.md
```

## Guidelines

- Generic simulation infrastructure should remain separate from vehicle-specific implementation
- Each vehicle should be self-contained with its own configuration
- Share common components between vehicles where appropriate
- Document vehicle-specific physics parameters and limitations
