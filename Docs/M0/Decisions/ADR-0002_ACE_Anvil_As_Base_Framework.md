# ADR-0002 ACE Anvil as Base Framework

## Status

Accepted

## Context

ACE Critical Care is planned as an advanced medical gameplay expansion for Arma Reforger.

The project document defines ACE Anvil Medical System as the base framework.

## Decision

ACE Critical Care extends ACE Anvil.

ACE Critical Care does not replace ACE Anvil.

## Reasoning

Using ACE Anvil as the base framework allows ACE Critical Care to build on existing medical systems while adding modular critical care systems.

## Consequences

- ACE Anvil compatibility must be considered during architecture work
- Direct hard-forks of ACE systems should be avoided where possible
- Upstream ACE changes are a technical risk
- Integration points must be documented before implementation