# ADR-0001 Repository Structure

## Status

Accepted

## Context

ACE Critical Care requires a clean repository structure for Milestone 0 foundation work.

The project contains both:

- Enfusion Workbench mod files
- Project documentation
- Build artifacts
- Supporting tools

## Decision

The Git repository root contains the project-level structure.

The Enfusion mod project is stored under:

`Addons/`

The documentation is stored under:

`Docs/M0/`

Build outputs are stored under:

`Builds/`

Tools and helper scripts are stored under:

`Tools/`

## Reasoning

This keeps the repository root clean while separating source files, documentation, tooling and builds.

Milestone 0 documentation remains inside `Docs/M0/` because M0 is the active foundation phase.

## Consequences

- The Enfusion project can remain isolated under `Addons/`
- M0 documentation is clearly scoped
- Future milestone documentation can be added without restructuring the repository
- Build artifacts do not mix with source files