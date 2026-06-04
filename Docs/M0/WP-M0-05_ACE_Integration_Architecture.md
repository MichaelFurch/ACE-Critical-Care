# WP-M0-05 ACE Integration Architecture

## Status

DONE

## Purpose

Define and validate how ACE Critical Care technically depends on and extends ACE Anvil.

## Scope

- Dependency structure
- Extension strategy
- Override policy
- Modular integration planning

## Deliverable

- ACE Integration Specification

## Task Checklist

### Dependency Structure

- [x] ACE Anvil module dependencies configured in Workbench
- [x] ACE Critical Care loads alongside required ACE Anvil modules
- [x] Current ACE Anvil dependency structure documented

### Extension Strategy

- [x] ACECC extension strategy documented
- [x] ACECC-owned extension approach documented
- [x] WP-M0-05 boundary documented

### Override Policy

- [x] Direct ACE source modification prohibited
- [x] Preferred integration methods documented
- [x] Override conditions documented

### Modular Integration Planning

- [x] ACECC remains separate from ACE Anvil
- [x] ACECC module separation documented
- [x] Later WP boundaries documented

### Deliverables

- [x] ACE Integration Specification created

## Remaining Work

- Validate multiplayer ownership and replication behavior in WP-M0-06
- Map ACE hooks, events and actions in WP-M0-07
- Implement runtime medical systems in later milestones

## Practical Goal

ACE Anvil is configured as the base dependency for the ACE Critical Care addon.

## Notes

WP-M0-05 performs technical dependency integration only.

No medical gameplay feature implementation is performed during this workpackage.

