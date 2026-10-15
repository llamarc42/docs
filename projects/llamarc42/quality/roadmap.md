# Roadmap

The roadmap describes the approved three-phase sequence. GitHub milestones and issues remain the operational source for individual work assignments and status.

## Phase 1 — Documentation Framework Definition

Establish and reconcile the authoritative foundation before implementation.

- DOC-01 establishes the product boundary through ADR-005.
- DOC-02 accepts Documentation Contract v0.1.
- DOC-03 accepts the Standalone Initialization Contract v0.1.
- DOC-04 accepts the Deterministic Validation Contract v0.1.
- DOC-05 reconciles current guidance, prior ADRs, repository responsibilities, and this roadmap.

Exit condition: the v0.1 documentation framework and its accepted contracts are coherent, reviewable, and free of unresolved current-authority conflicts.

## Phase 2 — CLI Foundation

Implement the accepted v0.1 contracts in dependency order.

1. **CLI Foundation — Framework Core v0.1:** implement reusable C# discovery, validation, template access, and initialization behavior with contract fixtures.
2. **CLI Foundation — CLI v0.1:** implement the user-facing command surface over verified Core behavior without duplicating contract logic.

Phase 2 must not silently expand the accepted contracts. Newly discovered architectural gaps return to the documentation governance process before dependent implementation proceeds.

## Phase 3 — Framework Validation and Adoption

Demonstrate that the framework works across supported environments and remains independently useful.

- maintain conformance and portability fixtures;
- dogfood the framework against suitable projects;
- verify documentation and tooling remain aligned;
- improve adoption guidance from observed evidence; and
- record gaps for separately governed follow-up.

PowerShell remains historical prototype and reference material. The API and `llamarc42.con` are deferred or independent. Multi-repository federation, DOC-07, and Documentation Contract v0.2 remain outside this v0.1 roadmap unless separately accepted.
