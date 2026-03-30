# ADR-003: Session Storage Model

## Status
Accepted

## Context

llamarc42 requires persistent, project-scoped conversations that:
- survive across sessions and days
- are inspectable and debuggable
- remain simple and portable

## Decision

Sessions are stored locally within each project:

```powershell
projects/.sessions/
```

Each session consists of:

- `session.json` (metadata)
- `messages.jsonl` (append-only message log)

## Rationale

- JSONL supports append-only writes
- metadata remains separate and structured
- storage is human-readable and inspectable
- sessions remain project-scoped
- no external database dependency

## Consequences

### Positive
- simple implementation
- easy debugging
- portable across environments
- aligns with local-first philosophy

### Negative
- limited querying capabilities
- requires additional tooling for aggregation
