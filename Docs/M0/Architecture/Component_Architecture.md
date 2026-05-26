# Component Architecture

## Purpose

Define the planned component architecture for ACE Critical Care.

## Scope

This document belongs to WP-M0-02 Technical Architecture.

It defines how ACE Critical Care systems are planned to be separated into components.

## Core Principle

ACE Critical Care uses modular components.

Components should be small, focused and maintainable.

## Planned Component Layers

### Core Components

Core components provide shared foundation functionality.

Responsibilities:

- Shared base logic
- Common state access
- Cross-module coordination
- Shared constants and types

### Medical State Components

Medical state components are planned to hold or expose medical state.

Responsibilities:

- Patient-related state containers
- Vital sign state access
- Trauma state access
- Airway and breathing state access

### System Components

System components are planned to process medical gameplay systems.

Responsibilities:

- Airway system logic
- Breathing system logic
- Monitoring system logic
- Medication system logic
- Trauma system logic
- Defibrillation system logic
- Ventilator system logic

### UI Components

UI components are planned to display medical information and interaction feedback.

Responsibilities:

- Medical UI state display
- Monitor UI
- Interaction feedback
- Non-authoritative client-side presentation

## Authority Rule

Critical medical logic must remain server-authoritative.

Clients may display state but must not own authoritative medical outcomes.

## Dependency Direction

Preferred dependency direction:

```text
UI -> System Components -> Medical State Components -> Core
```

Avoid direct coupling between feature systems unless explicitly documented.

## Current Status

Planning only.

No full medical component implementation is performed in WP-M0-02.