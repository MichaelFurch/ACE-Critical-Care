# Build and Release Strategy

## Purpose

Define the initial build and release structure for ACE Critical Care.

## Milestone Scope

WP-M0-01 defines only the structural foundation.

No automated build pipeline is implemented during this workpackage.

## Build Philosophy

Builds must be reproducible and clearly versioned.

Development artifacts and release artifacts must remain separated.

## Repository Build Structure

```text
Builds/
├── Dev/
├── Internal/
├── Release/
└── Archive/
```

## Planned Build Types

### Dev Builds

Purpose:

- Local testing
- Rapid iteration
- Experimental validation

Characteristics:

- Frequent changes
- Unstable
- Debug-friendly

### Internal Builds

Purpose:

- Team testing
- Multiplayer testing
- Integration validation

Characteristics:

- Semi-stable
- Shared internally
- Used for milestone validation

### Release Builds

Purpose:

- Public release
- Stable milestone delivery

Characteristics:

- Version tagged
- Validated
- Documented

## Versioning Philosophy

Planned format:

```text
Major.Minor.Patch-Stage
```

Example:
```text
0.1.0-prealpha
```

## Packaging Goals

 Future packaging should:

- Keep module structure intact
- Avoid unnecessary dependencies
- Preserve compatibility with ACE Anvil
- Support multiplayer-safe deployment
## CI/CD Status

Not implemented during WP-M0-01.

Feasibility evaluation planned for later milestones.

## Current Status

Foundation planning only.