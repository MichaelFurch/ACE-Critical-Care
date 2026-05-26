# Replication Strategy

## Purpose

Define the planned replication strategy for ACE Critical Care.

## Scope

This document belongs to WP-M0-02 Technical Architecture.

## Core Principle

Critical medical state must replicate from the server to clients.

The server remains authoritative.

## Planned Replicated Areas

### Patient Medical State

Planned replicated state includes:

- Consciousness state
- Cardiac arrest state
- Airway state
- Breathing state
- Trauma state
- Vital signs
- Medication effects

### Interaction State

Planned replicated interaction data includes:

- Active treatments
- Defibrillation actions
- Ventilation interactions
- Medical device state

### UI State

Clients may derive UI state from replicated medical state.

UI itself should not become authoritative.

## Replication Philosophy

Only necessary state should replicate.

Avoid excessive network traffic.

Prefer compact synchronized state over excessive event spam.

## Planned Networking Goals

- Deterministic multiplayer behavior
- Stable synchronization
- Minimal desynchronization
- Scalable multiplayer performance

## Risks

### Replication Overhead

High-frequency medical state updates may create network load.

### Desynchronization

Improper authority handling may create inconsistent patient states.

### Event Flooding

Excessive RPC or event traffic may impact multiplayer stability.

## Planned Future Areas

Future technical evaluation may include:

- Optimized state replication
- Partial state replication
- Event batching
- Client prediction for UI responsiveness

## Current Status

Planning only.

No replication systems are implemented during WP-M0-02.