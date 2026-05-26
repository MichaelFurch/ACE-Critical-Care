# ACE Anvil Integration Strategy

## Purpose

Define the architectural relationship between ACE Critical Care and ACE Anvil.

## Foundation Philosophy

ACE Critical Care extends ACE Anvil.

ACE Critical Care does NOT replace ACE Anvil.

ACE Anvil remains the foundational medical framework.

## Integration Goals

- Preserve ACE compatibility where possible
- Extend gameplay depth modularly
- Avoid hard-forking ACE systems
- Minimize upstream breakage risk
- Keep systems maintainable

## Planned Integration Areas

- Medical state systems
- Damage handling
- Interaction systems
- Medical item workflows
- Multiplayer medical synchronization
- Player state transitions

## Multiplayer Philosophy

Critical medical logic must remain server-authoritative.

Clients may display predicted UI feedback but must not own authoritative medical state.

## Architectural Risks

### Upstream Dependency Risk

ACE updates may break integrations.

### Replication Risk

Improper synchronization may create medical desyncs.

### Tight Coupling Risk

Overwriting ACE systems directly increases maintenance complexity.

## Current Status

Planning only.

No runtime integration is implemented during WP-M0-01.