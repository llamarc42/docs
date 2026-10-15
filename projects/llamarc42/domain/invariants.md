# Invariants

These rules must always hold for the llamarc42 framework.

## Documentation authority

- Project-owned documentation is authoritative only as classified by the accepted documentation contract.
- Accepted specifications and ADRs remain explicit and reviewable.
- Historical material remains discoverable but never becomes current authority because of proximity or tooling behavior.
- Navigation helps discovery but does not create authority.
- Transient conversations, summaries, generated output, and tool state do not override project documentation.

## Human and local operation

- A conforming documentation set remains readable and maintainable without the framework's software.
- Human-only use is a complete and supported mode.
- Framework validity does not depend on a network, hosted service, model, agent, or source-control system.

## Deterministic tooling

- Discovery, initialization, and validation outcomes follow accepted contracts and visible inputs.
- Validation is read-only and does not infer substantive architectural meaning.
- Missing, invalid, or inaccessible inputs produce explicit outcomes rather than guessed replacements.
- Core and CLI behavior cannot silently expand documentation authority.

## Boundaries

- Documentation is normative; Core is the reusable executable interpretation of accepted structural contracts; the CLI is their user-facing invocation and presentation surface.
- Optional tools and external workflows may assist but cannot become hidden dependencies or sources of authority.
- Repository existence does not establish project membership.
- Multi-repository federation is not a v0.1 invariant or capability.
