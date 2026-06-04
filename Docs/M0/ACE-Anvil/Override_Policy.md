# ACE Anvil Override Policy

## Purpose

Define when ACE Critical Care may override ACE Anvil behavior.

## Default Rule

Direct ACE Anvil source modification is not allowed.

ACE Critical Care must remain a separate addon.

## Preferred Integration Methods

Preferred methods:

- external extension components
- separate ACECC configs
- separate ACECC prefabs
- event-driven integration
- documented ACE extension points
- wrapper or adapter classes owned by ACECC

## Direct Overrides

Direct overrides are avoided by default.

A direct override may only be considered if:

- no extension point exists
- the feature is required by ACECC scope
- the override is documented
- multiplayer impact is understood
- compatibility risk is accepted explicitly

## Forbidden During WP-M0-05

During WP-M0-05, the following are not performed:

- no ACE source edits
- no ACE medical logic replacement
- no runtime patient state implementation
- no replication implementation
- no ACE action remapping

## Risk

Hard overrides increase compatibility risk with future ACE Anvil updates.