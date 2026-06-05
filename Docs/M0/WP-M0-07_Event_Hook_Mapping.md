# WP-M0-07 Event & Hook Mapping

## Status

In Progress

## Purpose

Identify and document the ACE Anvil hooks, events and action extension points required by ACE Critical Care.

## Scope

- ACE hook analysis
- Event integration planning
- Action extension mapping

## Deliverable

- ACE event integration document

## Technical Goal

Create a verified integration map showing where ACE Critical Care can extend ACE Anvil without modifying ACE Anvil source files.

## Analysis Rules

- Only verified ACE classes, methods, events and actions are documented as confirmed integration points.
- Unverified possibilities are marked as candidates.
- Missing extension points are documented explicitly.
- Direct ACE source modification is not allowed.
- Hard overrides are avoided unless no supported extension path exists.

## Workpackage Boundary

WP-M0-07 maps integration points.

It does not implement production medical systems, final actions or the final replication layer.