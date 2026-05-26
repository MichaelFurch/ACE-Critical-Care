# ACE Critical Care Technical Design Document

## Status

Draft

## Scope

This document summarizes the technical foundation defined during WP-M0-02.

## Foundation Philosophy

ACE Critical Care extends ACE Anvil through modular systems.

The project prioritizes:

- Multiplayer-safe architecture
- Server-authoritative medical logic
- Modular maintainability
- Explicit system boundaries
- Deterministic medical state handling

## Technical Pillars

### Component Architecture

ACE Critical Care uses modular component-oriented architecture.

Primary planned layers:

- Core
- API
- Medical State Components
- System Components
- UI Components

### Multiplayer Authority

Critical medical gameplay logic remains server-authoritative.

Clients may:

- Request interactions
- Display UI
- Display local feedback

Clients must not finalize authoritative medical outcomes.

### Replication Strategy

Only necessary medical state should replicate.

Replication goals:

- Stable synchronization
- Minimal desynchronization
- Low replication overhead
- Multiplayer scalability

### State Machine Design

Medical systems are planned around explicit states.

Planned state-machine-driven systems include:

- Consciousness
- Cardiac state
- Airway state
- Breathing state
- Treatment progression

### Medical Event Pipeline

Medical systems are planned around explicit event flow.

Planned event flow:

```text
Interaction Request
→ Validation
→ Medical Event
→ Medical State Update
→ Replication
→ Client Feedback
```

### Planned Module Structure
- ACECC_Core
- ACECC_API
- ACECC_Airway
- ACECC_Breathing
- ACECC_Monitor
- ACECC_Medication
- ACECC_Trauma
- ACECC_Defib
- ACECC_Ventilator
- ACECC_UI

### Multiplayer Philosophy

Medical simulation consistency has higher priority than client-side autonomy.

Critical gameplay state must remain deterministic and synchronized.

### ACE Anvil Integration

ACE Critical Care extends ACE Anvil instead of replacing it.

Direct hard-forks should be avoided where possible.

### Risks
#### Upstream Dependency Risk

ACE updates may impact integration compatibility.

#### Replication Risk

High-frequency medical state replication may impact multiplayer stability.

#### State Complexity Risk

Medical state machines may become difficult to maintain if ownership boundaries are unclear.

#### Coupling Risk

Direct feature-module dependencies may reduce maintainability.

### WP-M0-02 Status

In Progress