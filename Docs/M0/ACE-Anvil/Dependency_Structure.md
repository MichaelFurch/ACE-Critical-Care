# ACE Anvil Dependency Structure

## Purpose

Document the current ACE Anvil dependency structure used by ACE Critical Care.

## Current ACE Dependencies

ACE Critical Care currently depends on the following ACE Anvil development addons:

- ACE_Core_Dev
- ACE_Medical_Core_Dev
- ACE_Medical_Breathing_Dev
- ACE_Medical_Circulation_Dev

## Dependency Role

### ACE_Core_Dev

Base ACE framework dependency.

### ACE_Medical_Core_Dev

Base ACE medical framework dependency.

### ACE_Medical_Breathing_Dev

ACE medical breathing dependency.

### ACE_Medical_Circulation_Dev

ACE medical circulation dependency.

## Integration Rule

ACE Critical Care depends on these ACE modules but does not modify ACE source files directly.

## WP-M0-05 Scope

WP-M0-05 validates ACE Anvil as a technical dependency base.

Medical feature implementation is not part of this workpackage.

## Deferred Work

- Multiplayer authority validation is handled in WP-M0-06.
- ACE hook and event mapping is handled in WP-M0-07.
- Runtime medical systems are implemented in later milestones.