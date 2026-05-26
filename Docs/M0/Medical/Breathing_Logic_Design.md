# Breathing Logic Design

## Purpose

Define the planned breathing gameplay logic for ACE Critical Care.

## Scope

This document belongs to WP-M0-03 Medical System Design.

## Design Philosophy

Breathing gameplay should connect airway state, ventilation and oxygenation into readable gameplay states.

The system prioritizes:

- Clear respiratory states
- Understandable intervention effects
- Multiplayer-safe state handling
- Gameplay-first medical logic

## Planned Breathing Areas

### Breathing State

The system plans to represent whether the patient is:

- Breathing spontaneously
- Breathing inadequately
- Not breathing
- Assisted by ventilation

### Ventilation Effectiveness

Breathing logic may be influenced by:

- Airway state
- Ventilation support
- Trauma state
- Consciousness state

### Oxygenation

Breathing state may influence:

- Oxygen saturation
- Consciousness progression
- Cardiac deterioration
- Need for ventilation support

## Planned Intervention Categories

Planned breathing-related interventions include:

- Oxygen support
- Assisted ventilation
- Mechanical ventilation support

## Planned Gameplay Priorities

The breathing system should support:

- Recognition of respiratory compromise
- Escalation from basic support to advanced support
- Team-based ventilation gameplay
- Monitoring-driven treatment decisions

## Multiplayer Considerations

Critical breathing and oxygenation state remains server-authoritative.

Clients may display respiratory UI feedback.

## Risks

### State Complexity

Breathing, airway and ventilation states may overlap if ownership is unclear.

### Replication Overhead

High-frequency oxygenation updates may increase network traffic.

### Gameplay Overload

Overly detailed respiratory simulation may slow gameplay too much.

## Current Status

Planning only.

No runtime breathing implementation is performed during WP-M0-03.