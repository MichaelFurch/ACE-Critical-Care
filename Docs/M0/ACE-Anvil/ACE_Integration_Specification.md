# ACE Integration Specification

## Status

Draft

## Scope

This document defines the ACE Anvil integration architecture for ACE Critical Care during WP-M0-05.

## Dependency Structure

ACE Critical Care currently depends on:

- ACE_Core_Dev
- ACE_Medical_Core_Dev
- ACE_Medical_Breathing_Dev
- ACE_Medical_Circulation_Dev

## Extension Strategy

ACE Critical Care extends ACE Anvil through ACECC-owned modules and integration layers.

ACE Critical Care remains separate from ACE Anvil and does not modify ACE Anvil source files.

## Override Policy

Direct ACE Anvil overrides are avoided by default.

Overrides require explicit documentation and validation.

## Modular Integration Planning

Planned ACECC modules remain separated by responsibility:

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

## Workpackage Boundary

WP-M0-05 validates dependency structure and integration architecture.

WP-M0-06 handles multiplayer authority planning.

WP-M0-07 handles ACE hook, event and action mapping.

## WP-M0-05 Status

DONE

