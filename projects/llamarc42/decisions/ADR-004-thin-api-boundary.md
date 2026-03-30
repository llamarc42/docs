# ADR-004: Thin API Boundary

## Status
Accepted

## Context

llamarc42 may expose functionality through an API for:
- browser UI
- editor integrations
- external tools

There is a risk of duplicating business logic between:
- core engine
- API layer

## Decision

The API will act as a thin facade over the core engine.

All business logic remains in the core library.

## Rationale

- ensures a single source of truth for behavior
- prevents divergence between clients
- keeps API replaceable
- simplifies testing and maintenance

## Consequences

### Positive
- consistent behavior across surfaces
- reduced duplication
- easier refactoring

### Negative
- requires disciplined layering
- API cannot evolve independently of core
