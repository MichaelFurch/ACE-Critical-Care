# State Machine Planning

## Purpose

Define how ACE Critical Care plans to use state machines for medical system behavior.

## Scope

This document belongs to WP-M0-02 Technical Architecture.

## Core Principle

Medical gameplay systems should use explicit states instead of unclear boolean chains.

State transitions must be predictable, debuggable and multiplayer-safe.

## Planned State Machine Areas

### Consciousness State

Planned states may include:

- Conscious
- Unconscious
- Recovering

### Cardiac State

Planned states may include:

- Stable circulation
- Unstable circulation
- Cardiac arrest

### Airway State

Planned states may include:

- Patent airway
- Compromised airway
- Secured airway

### Breathing State

Planned states may include:

- Spontaneous breathing
- Assisted ventilation
- Apnea

### Treatment State

Planned states may include:

- No active treatment
- Treatment in progress
- Treatment completed
- Treatment failed

## Transition Rules

State transitions must be:

- Server-authoritative
- Explicitly validated
- Logged where useful
- Replication-safe
- Avoided on clients unless purely visual

## Design Rule

Avoid hidden implicit state transitions.

Avoid duplicated state ownership across multiple systems.

## Risks

### State Explosion

Too many states may make systems difficult to debug.

### Conflicting Ownership

Multiple modules changing the same state may create inconsistent behavior.

### Multiplayer Mismatch

Client-side state transitions may desynchronize from server truth.

## Current Status

Planning only.

No full state machine implementation is performed during WP-M0-02.