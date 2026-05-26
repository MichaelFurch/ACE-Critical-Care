# ACE Critical Care Naming Conventions

## Status

Milestone 0 baseline.

## Global Prefix

All project-owned scripts, components, prefabs, configs and UI files use the project prefix:

`ACECC_`

## Enforce Script Classes

| Type | Pattern | Example |
|---|---|---|
| Base class | `ACECC_Name` | `ACECC_MedicalSystem` |
| Component | `ACECC_NameComponent` | `ACECC_MonitorComponent` |
| Data class | `ACECC_NameData` | `ACECC_AirwayStateData` |
| Enum | `ACECC_EName` | `ACECC_EAirwayState` |
| Interface | `ACECC_IName` | `ACECC_IMedicalModule` |

## Files

| Asset Type | Pattern | Example |
|---|---|---|
| Script | `ACECC_Name.c` | `ACECC_MonitorComponent.c` |
| Prefab | `ACECC_Name.et` | `ACECC_Monitor.et` |
| Config | `ACECC_Name.conf` | `ACECC_MedicationConfig.conf` |
| Layout | `ACECC_Name.layout` | `ACECC_Monitor.layout` |

## Rule

No medical gameplay logic is implemented as part of WP-M0-01.