# ACE Critical Care Module Boundaries

## Purpose

Define the planned module boundaries for ACE Critical Care.

## Core Rule

Modules must be independently maintainable and must not directly overwrite each other.

## Core Module

Path:

`Scripts/Game/ACECC/Core/`

Responsibilities:

- Shared base systems
- Common utility logic
- Central initialization points
- Shared constants
- Shared base types
- Cross-module coordination

Must not contain feature-specific medical gameplay logic.

## API Module

Path:

`Scripts/Game/ACECC/API/`

Responsibilities:

- Public access layer for other ACECC modules
- Stable interfaces between systems
- Future external integration points
- Controlled access to medical state

Must not directly own simulation state.

## Feature Modules

Paths:

- `Scripts/Game/ACECC/Airway/`
- `Scripts/Game/ACECC/Breathing/`
- `Scripts/Game/ACECC/Monitor/`
- `Scripts/Game/ACECC/Medication/`
- `Scripts/Game/ACECC/Trauma/`
- `Scripts/Game/ACECC/Defib/`
- `Scripts/Game/ACECC/Ventilator/`
- `Scripts/Game/ACECC/UI/`

Responsibilities:

- System-specific logic
- System-specific data models
- System-specific UI logic
- System-specific interaction logic
- System-specific configuration bindings

## Dependency Direction

Allowed direction:

```text
Feature Module -> API -> Core
```

## Avoid:
```text
Feature Module -> Feature Module
```

## Current Status

Planning only.

No full medical module logic is implemented during WP-M0-01.