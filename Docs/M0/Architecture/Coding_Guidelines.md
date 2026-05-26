# ACE Critical Care Coding Guidelines

## Purpose

Define baseline coding rules for ACE Critical Care development.

## Scope

These guidelines apply to project-owned Enforce Script files during and after Milestone 0.

## Global Prefix

All project-owned classes, components, configs, prefabs and UI files use:

`ACECC_`

## File Organization

Scripts are organized under:

`Scripts/Game/ACECC/`

Module folders:

- `Core`
- `API`
- `Airway`
- `Breathing`
- `Monitor`
- `Medication`
- `Trauma`
- `Defib`
- `Ventilator`
- `UI`

## Class Rules

- One primary class per file
- File name matches primary class name
- Project classes use the `ACECC_` prefix
- Components use the suffix `Component`
- Data containers use the suffix `Data`
- Enums use the prefix `ACECC_E`

## Architecture Rules

- Core contains shared foundation logic only
- API provides stable access between systems
- Feature modules should not directly depend on each other
- Critical medical state must not be client-authoritative
- Direct ACE Anvil overrides should be avoided unless explicitly documented

## Multiplayer Rules

- Server owns critical medical simulation state
- Clients may request interactions
- Clients may display UI feedback
- Clients must not finalize medical outcomes locally

## WP-M0-01 Restriction

No medical gameplay logic is implemented during WP-M0-01.