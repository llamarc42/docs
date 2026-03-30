# ADR-002: Core Implementation Language

## Status
Accepted

## Context

llamarc42 requires a core engine responsible for:
- document resolution
- session management
- retrieval logic
- prompt construction
- model interaction

The initial prototype is implemented in PowerShell for speed of iteration.

However, long-term requirements include:
- strong domain modeling
- testability
- cross-platform support
- reuse across CLI, API, and integrations

## Decision

The core engine will be implemented in C#.

PowerShell will be retained as a wrapper and bootstrap surface during early development and for automation scenarios.

## Consequences

### Positive
- strong typing and domain modeling
- improved testability
- cross-platform compatibility
- shared logic across CLI and API
- long-term maintainability

### Negative
- increased initial complexity
- migration effort from PowerShell prototype
