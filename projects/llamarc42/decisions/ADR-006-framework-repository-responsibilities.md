# ADR-006: Framework Repository Responsibilities and Architectural Reconciliation

## Status
Accepted

## Acceptance record

The maintainer first authorized preparation of the proposed reconciliation in [DOC-05 issue comment 6201160065](https://github.com/llamarc42/docs/issues/11#issuecomment-6201160065). That authorization did not accept the ADR or change any existing ADR status.

After reviewing the proposal and remediation findings, the maintainer accepted ADR-006 and authorized its coordinated amendment and supersession relationships in [DOC-05 issue comment 6202337469](https://github.com/llamarc42/docs/issues/11#issuecomment-6202337469).

## Context

[ADR-005](ADR-005-agent-agnostic-documentation-framework-boundary.md) established llamarc42 as a documentation-first, local-first, agent-independent framework. Documentation Contract v0.1, the Standalone Initialization Contract v0.1, and the Deterministic Validation Contract v0.1 subsequently defined the initial documentation and tooling contracts.

Before this reconciliation, ADR-001 through ADR-004 described the earlier copilot architecture. The multi-repository separation and C# implementation choice remained useful, but the recorded repository names and responsibilities conflicted with the accepted framework boundary. Session storage and a required API facade were outside the v0.1 framework. The decisions required explicit reconciliation rather than incidental edits or erasure of their history.

## Decision

The current repositories have the following responsibilities:

| Repository | v0.1 responsibility |
| --- | --- |
| `llamarc42/docs` | Owns authoritative framework documentation, accepted contracts, architectural decisions, current guidance, and preserved history. |
| `llamarc42/core` | Owns the reusable C# implementation of accepted discovery and validation behavior, the single executable copy of versioned initialization template assets, deterministic template selection, initialization preflight and planning, safe application, and structured results and diagnostics. |
| `llamarc42/cli` | Owns command invocation, argument handling, user interaction, and terminal and machine-readable presentation over Core behavior without duplicating template selection, initialization, discovery, validation, or authority-classification logic. |
| `llamarc42/powershell` | Preserves the original prototype and reference material. It is not the v0.1 wrapper or an implementation dependency. |
| `llamarc42/api` | Is deferred. It is not a v0.1 framework component or dependency. |
| `llamarc42.con` | Remains an independent project. Any future integration is optional, deferred, and separately governed. |

The documentation contract determines which project-owned documents carry current authority. Core implements structural behavior defined by accepted contracts, while the CLI invokes and presents that behavior without redefining document authority. Repository existence does not by itself establish participation in the framework.

This decision retains the multi-repository separation. It does not define multi-repository project membership, federation, remote discovery, or cross-repository governance. Those concerns remain outside v0.1 and require separately accepted authority.

## Amends

- [ADR-001: Repository Topology](ADR-001-repo-topology.md)
- [ADR-002: Core Implementation Language](ADR-002-core-implementation-language.md)

## Amendment scope

ADR-001 remains authoritative for separating concerns across repositories. Its repository identities and responsibilities are replaced only by the table and boundaries in this decision. The amendment does not make every existing repository a framework participant and does not introduce repository federation.

ADR-002 remains authoritative for implementing shared Core behavior in C#. Core's responsibility is narrowed to the reusable implementation of accepted discovery and validation behavior, versioned initialization template assets and their deterministic selection, initialization preflight and planning, safe application, and structured results and diagnostics. Core implements these accepted contracts; it does not establish or revise their normative meaning. Model interaction, retrieval, prompts, sessions, and API sharing are not Core responsibilities under v0.1. PowerShell is preserved as historical prototype and reference material, not as a required wrapper or bootstrap dependency.

## Supersedes

- [ADR-003: Session Storage Model](ADR-003-sessions-storage-model.md)
- [ADR-004: Thin API Boundary](ADR-004-thin-api-boundary.md)

ADR-003 is superseded because chat and session management are outside the accepted framework boundary. Its local-first and inspectability rationale remains historical context, but its session model is not a v0.1 requirement.

ADR-004 is superseded because an API is deferred and is not a framework dependency. Its separation-of-concerns rationale remains historical context, but it does not require an API surface in v0.1.

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
