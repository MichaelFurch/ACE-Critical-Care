# ACE Anvil Extension Strategy

## Purpose

Define how ACE Critical Care extends ACE Anvil during later implementation milestones.

## Core Principle

ACE Critical Care extends ACE Anvil through separate ACECC-owned modules.

ACE Critical Care must not modify ACE Anvil source files.

## Extension Approach

ACE Critical Care uses ACE Anvil as the base medical framework.

ACECC functionality is added through:

- ACECC-owned components
- ACECC-owned configs
- ACECC-owned prefabs
- ACECC-owned scripts
- ACECC-owned UI layers
- documented integration points

## Current ACE Dependency Base

Current configured ACE Anvil development dependencies:

- ACE_Core_Dev
- ACE_Medical_Core_Dev
- ACE_Medical_Breathing_Dev
- ACE_Medical_Circulation_Dev

## Planned Extension Areas

ACECC may extend ACE Anvil in these areas:

- medical patient state
- airway and breathing systems
- circulation-related systems
- medical interactions
- medical devices
- UI feedback

## Boundary

WP-M0-05 defines extension architecture only.

Concrete hook analysis belongs to WP-M0-07.

Runtime medical implementation belongs to later milestones.