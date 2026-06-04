# Multiplayer Authority Architecture

## Status

Draft

## Purpose

Define the multiplayer ownership, authority and synchronization architecture for ACE Critical Care.

This document is created during WP-M0-06 and is based on:

- the ACE Critical Care project plan
- the configured ACE Anvil dependency setup
- initial Workbench inspection of ACE Anvil medical scripting patterns

## Scope

WP-M0-06 covers:

- Ownership logic
- Replication authority
- Synchronization planning

WP-M0-06 does not implement the final multiplayer-safe replication layer.

Replication implementation belongs to WP-M1-02.

## Core Multiplayer Rule

ACE Critical Care follows server-authoritative medical logic.

Clients may:

- display information
- request interactions
- render local effects
- update local UI feedback

Clients must not:

- finalize critical medical outcomes
- authoritatively change patient state
- directly decide treatment success
- directly apply critical physiology changes
- control other players' medical state

The server validates medical actions, controls authoritative medical state and synchronizes results.

## ACE Anvil Responsibility

ACE Anvil remains responsible for:

- baseline medical entity handling
- damage registration
- unconsciousness foundations
- bleeding state handling
- core multiplayer replication
- patient ownership logic

ACE Critical Care extends ACE Anvil through additional modular systems and does not replace the ACE Anvil medical foundation.

## Verified ACE Anvil Multiplayer Patterns

### Server-Side Medical Processing

Initial inspection of ACE Medical Core showed repeated use of:

`Replication.IsServer()`

in medical damage and hit zone logic.

Verified examples include:

* damage processing in `SCR_CharacterDamageManagerComponent`
* killed-state handling in `SCR_CharacterDamageManagerComponent`
* unconsciousness-related blood hit zone handling
* resilience damage and regeneration scaling

Observed pattern:

Medical state event
→ server-side validation / processing
→ authoritative state change
→ replication through existing systems or explicit replicated properties

### Replicated Medical Properties

ACE Medical Core uses replicated properties for selected medical configuration or state data.

Verified example:

`[RplProp()] protected float m_fACE_Medical_MinHealthScaledForEpinephrine;`

Observed pattern:

Server or authoritative component changes value
→ Replication.BumpMe()
→ replicated value becomes available to relevant network instances

### Owner-Only Medical Feedback

ACE Medical Pain uses owner-only replication for subjective medical effects.

Verified examples:

`[RplProp(condition: RplCondition.OwnerOnly)] protected float m_fACE_Medical_PainSuppression;`

and:

`[RplProp(condition: RplCondition.OwnerOnly, onRplName: "ACE_Medical_OnPainEffectTypeChanged")] protected ACE_Medical_EPainEffectType m_eACE_Medical_PainEffectType;`

Observed pattern:

Server determines or synchronizes value
→ value is replicated only to the owning player
→ owner-side callback updates local UI or screen effect

## ACE Critical Care Authority Model

### Authoritative Server State

| System Area | Server Authority |
| :--- | :--- |
| Airway state | Yes |
| Oxygenation state | Yes |
| Ventilation state | Yes |
| Consciousness state | Yes |
| Medication effects | Yes |
| Trauma progression | Yes |
| Cardiac rhythm state | Yes |
| CPR state | Yes |
| Active monitor values | Yes |
| Treatment result | Yes |

### Client Responsibilities

Clients are responsible for:

| Area | Client Role |
| :--- | :--- |
| Interaction input | Request only |
| Medical UI | Display only |
| Monitor UI | Display synchronized values |
| Audio feedback | Local playback |
| Screen effects | Local rendering |
| Animation feedback | Local or synchronized presentation |
| Action preview | Non-authoritative feedback only |

## Replication Visibility Model

### Global / Shared Replication
Use for information multiple players need to understand the patient state.

### Owner-Only Replication
Use for information only the affected player should receive (e.g., pain, screen effects).

### Server-Only State
Use for values required for internal calculation only.

## Synchronization Principles

1. **Server Owns Medical Truth**: All critical medical outcomes originate from the server.
2. **Clients Request, Server Validates**: Clients do not directly mutate medical state.
3. **Replicate Only What Is Needed**: Minimize traffic.
4. **Subjective Effects Are Owner-Only**: Pain/impairment should be local.
5. **Shared Devices Need Shared Visibility**: Monitor/devices require broad sync.
6. **Keep UI Separate From Gameplay Logic**: UI reads state but does not own logic.

## WP-M0-06 Result

ACE Critical Care will use a server-authoritative multiplayer model. ACE Anvil remains responsible for core medical ownership. ACE Critical Care adds multiplayer-safe extension logic through modular systems, owner-aware replication and explicit synchronization planning.