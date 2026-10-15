# ADR-006: Framework Repository Responsibilities and Architectural Reconciliation

## Status
Proposed

## Review authorization

The maintainer authorized preparation of this proposed reconciliation in [DOC-05 issue comment 6201160065](https://github.com/llamarc42/docs/issues/11#issuecomment-6201160065). That authorization explicitly did not accept this ADR or change any existing ADR status.

## Context

[ADR-005](ADR-005-agent-agnostic-documentation-framework-boundary.md) established llamarc42 as a documentation-first, local-first, agent-independent framework. Documentation Contract v0.1, the Standalone Initialization Contract v0.1, and the Deterministic Validation Contract v0.1 subsequently defined the initial documentation and tooling contracts.

ADR-001 through ADR-004 still describe the earlier copilot architecture. The multi-repository separation and C# implementation choice remain useful, but the recorded repository names and responsibilities conflict with the accepted framework boundary. Session storage and a required API facade are outside the v0.1 framework. The accepted decisions require explicit reconciliation rather than incidental edits or erasure of their history.

## Proposed decision

The current repositories have the following responsibilities:

| Repository | v0.1 responsibility |
| --- | --- |
| `llamarc42/docs` | Owns authoritative framework documentation, accepted contracts, architectural decisions, current guidance, and preserved history. |
| `llamarc42/core` | Implements reusable C# behavior for deterministic discovery, validation, template access, and initialization as governed by accepted documentation. |
| `llamarc42/cli` | Provides the user-facing command invocation and presentation layer over Core behavior without duplicating that behavior. |
| `llamarc42/powershell` | Preserves the original prototype and reference material. It is not the v0.1 wrapper or an implementation dependency. |
| `llamarc42/api` | Is deferred. It is not a v0.1 framework component or dependency. |
| `llamarc42.con` | Remains an independent project. Any future integration is optional, deferred, and separately governed. |

The documentation is normative. Core provides executable behavior for accepted contracts. The CLI is a presentation and invocation surface over Core. Repository existence does not by itself establish participation in the framework.

This decision retains the multi-repository separation. It does not define multi-repository project membership, federation, remote discovery, or cross-repository governance. Those concerns remain outside v0.1 and require separately accepted authority.

## Amends

- [ADR-001: Repository Topology](ADR-001-repo-topology.md)
- [ADR-002: Core Implementation Language](ADR-002-core-implementation-language.md)

## Amendment scope

ADR-001 remains authoritative for separating concerns across repositories. Its repository identities and responsibilities are replaced only by the table and boundaries in this decision. The amendment does not make every existing repository a framework participant and does not introduce repository federation.

ADR-002 remains authoritative for implementing shared Core behavior in C#. Core's responsibility is narrowed to deterministic discovery, validation, template access, and initialization governed by accepted documentation. Model interaction, retrieval, prompts, sessions, and API sharing are not Core responsibilities under v0.1. PowerShell is preserved as historical prototype and reference material, not as a required wrapper or bootstrap dependency.

## Supersedes

- [ADR-003: Session Storage Model](ADR-003-sessions-storage-model.md)
- [ADR-004: Thin API Boundary](ADR-004-thin-api-boundary.md)

Upon acceptance, ADR-003 would be superseded because chat and session management are outside the accepted framework boundary. Its local-first and inspectability rationale would remain historical context, but its session model would not be a v0.1 requirement.

Upon acceptance, ADR-004 would be superseded because an API is deferred and is not a framework dependency. Its separation-of-concerns rationale would remain historical context, but it would not require an API surface in v0.1.

## Consequences

### Positive

- Current repository responsibilities align with the accepted documentation-first boundary.
- Normative documentation, reusable executable behavior, and user-facing command presentation have distinct owners.
- The useful portions of ADR-001 and ADR-002 remain traceable through explicit amendments.
- ADR-003 and ADR-004 remain discoverable as historical decisions without imposing superseded requirements.
- Deferred and independent repositories cannot silently expand the v0.1 framework.

### Negative

- Existing prototype code and repository names may not match current framework responsibilities.
- Future participation by the API, `llamarc42.con`, or other repositories requires new accepted authority.
- Multi-repository project governance remains deferred beyond v0.1.
