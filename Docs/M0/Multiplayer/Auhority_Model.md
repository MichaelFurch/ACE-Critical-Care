# Multiplayer Authority Model

## Purpose

Define the multiplayer authority rules for ACE Critical Care.

## Core Principle

Critical medical gameplay state is server-authoritative.

Clients must never become authoritative owners of medical state.

## Server Responsibilities

The server owns:

- Medical state evaluation
- Vital sign calculation
- Trauma progression
- Medication processing
- Defibrillation validation
- Airway state validation
- Cardiac arrest state
- Blood volume calculations
- Oxygenation state

## Client Responsibilities

Clients may handle:

- UI display
- Local audio feedback
- Animation requests
- Interaction requests
- Temporary prediction visuals

Clients must not finalize medical outcomes locally.

## Replication Philosophy

Only necessary medical state should replicate.

Avoid excessive replication frequency.

## Performance Goals

- Minimize network overhead
- Prevent desynchronization
- Avoid unnecessary RPC usage
- Keep medical simulation deterministic

## Risks

### Desynchronization

Medical states may diverge if authority boundaries are unclear.

### Replication Overhead

High-frequency medical variables may impact multiplayer performance.

### Prediction Mismatch

Client prediction may temporarily differ from server state.

## Current Status

Planning only.

No multiplayer medical systems are implemented during WP-M0-01.