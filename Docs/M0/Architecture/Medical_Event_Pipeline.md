# Medical Event Pipeline

## Purpose

Define the planned medical event pipeline for ACE Critical Care.

## Scope

This document belongs to WP-M0-02 Technical Architecture.

## Core Principle

Medical changes should flow through explicit events instead of uncontrolled direct state changes.

The server remains authoritative for critical medical events.

## Planned Event Flow

```text
Interaction Request
        ↓
Validation
        ↓
Server-side Medical Event
        ↓
Medical State Update
        ↓
Replication
        ↓
Client UI / Feedback
```
## Event Categories
### Interaction Events

#### Examples:

- Treatment interaction requested
- Device interaction requested
- Medication action requested
- Defibrillation action requested
- Ventilation action requested
### State Change Events

#### Examples:

- Consciousness changed
- Airway state changed
- Breathing state changed
- Cardiac state changed
- Vital signs changed

### Device Events

#### Examples:

- Monitor state changed
- Defibrillator state changed
- Ventilator state changed

### Feedback Events

#### Examples:

- UI update requested
- Audio feedback requested
- Animation feedback requested
### Authority Rules

Interaction requests may originate from clients.

Validation and final medical state changes must happen on the server.

Clients may display replicated results and local feedback.

## Design Rules
- Avoid direct uncontrolled medical state mutation
- Prefer explicit event handling
- Keep event ownership clear
- Avoid excessive event spam
- Do not use UI events as medical truth
## Risks
### Event Flooding

Too many medical events may create performance or networking issues.

### Hidden State Mutation

If systems bypass the pipeline, debugging becomes difficult.

### Authority Confusion

If clients trigger final state changes directly, multiplayer desyncs may occur.

### Current Status

Planning only.

No full event pipeline implementation is performed during WP-M0-02.