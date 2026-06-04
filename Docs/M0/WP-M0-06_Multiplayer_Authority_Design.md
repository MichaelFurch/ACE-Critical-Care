# WP-M0-06 Multiplayer Authority Design

## Status

DONE

## Purpose

Define the multiplayer ownership, authority and synchronization architecture for ACE Critical Care.

## Scope

- Ownership logic
- Replication authority
- Synchronization planning

## Deliverable

- Multiplayer authority architecture

## Task Checklist

### Ownership Logic

- [x] ACE Anvil medical module behavior inspected
- [x] Server-authoritative medical processing pattern identified
- [x] Owner-only feedback pattern identified
- [x] Client request / server validation model defined

### Replication Authority

- [x] Critical medical state authority assigned to server
- [x] Client responsibilities limited to requests, UI and local feedback
- [x] Owner-only replication use case documented
- [x] Shared replication use case documented
- [x] Server-only state category documented

### Synchronization Planning

- [x] Medical interaction synchronization flow defined
- [x] Treatment request model defined
- [x] Replication visibility model defined
- [x] Authority matrix created
- [x] Risks and mitigations documented

### Deliverables

- [x] Multiplayer Authority Architecture created

## Completion Review

### Verified ACE Anvil Patterns

Initial ACE Anvil medical code inspection showed these multiplayer patterns:

- server-side medical processing through `Replication.IsServer()`
- replicated medical properties through `RplProp`
- owner-only subjective feedback through `RplCondition.OwnerOnly`
- replication updates through `Replication.BumpMe()`

### ACE Critical Care Authority Decision

ACE Critical Care uses server-authoritative medical logic.

Clients may request actions, display synchronized state and render local feedback, but they must not finalize critical medical outcomes.

### Workpackage Boundary

WP-M0-06 defines multiplayer authority architecture.

It does not implement the final replication layer.

The final multiplayer-safe replication layer belongs to WP-M1-02.

### Deliverable

- `Docs/M0/Multiplayer/Multiplayer_Authority_Architecture.md`

## WP-M0-06 Status

Completed


## Existing Project Rules

ACE Critical Care uses server-authoritative medical logic.

Clients display information, request actions and play local effects.

The server validates actions, controls authoritative medical state and synchronizes results.

## Technical Approach

WP-M0-06 combines architecture planning with inspection of the currently loaded ACE Anvil development modules.

Authority and synchronization decisions must be based on verified ACE Anvil and Enfusion behavior.

## Workpackage Boundary

WP-M0-06 defines multiplayer authority architecture.

It does not implement the final multiplayer-safe replication layer.

Replication implementation belongs to WP-M1-02.