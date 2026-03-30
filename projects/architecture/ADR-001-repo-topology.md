# ADR-001: Repository Topology

## Status
Accepted

## Context

llamarc42 is composed of multiple concerns:
- documentation
- core logic
- user interfaces
- integrations

A single repository would couple these concerns too tightly.

## Decision

Use multiple repositories:

- llamarc42-docs
- llamarc42-core
- llamarc42-cli
- llamarc42-powershell
- llamarc42-api (optional)

## Consequences

### Positive
- clear ownership boundaries
- independent evolution
- easier testing and versioning

### Negative
- increased coordination overhead
