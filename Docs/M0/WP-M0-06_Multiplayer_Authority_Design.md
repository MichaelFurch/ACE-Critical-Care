# WP-M0-06 Multiplayer Authority Design

## Status

In Progress

## Purpose

Define the multiplayer ownership, authority and synchronization architecture for ACE Critical Care.

## Scope

- Ownership logic
- Replication authority
- Synchronization planning

## Deliverable

- Multiplayer authority architecture

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