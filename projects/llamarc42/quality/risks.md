# Risks

## Documentation drift

Current guidance, accepted decisions, and executable behavior may diverge.

Mitigation: keep authority explicit, preserve lifecycle relationships, validate structural contracts deterministically, and reconcile conflicts through reviewed documentation changes.

## Validator divergence

Core and CLI implementations may interpret the accepted contracts differently.

Mitigation: Core owns reusable contract behavior; the CLI presents Core outcomes rather than duplicating rules; fixtures verify conformance.

## Portability failures

Case sensitivity, Unicode representation, path handling, or host-specific filesystem behavior may produce inconsistent results.

Mitigation: follow the accepted deterministic validation rules and maintain portable cross-platform fixtures before claiming conformance.

## Scope and integration creep

AI, orchestration, API, hosted, or multi-repository concerns may be introduced as framework dependencies before a demonstrated need and accepted decision.

Mitigation: enforce the ADR-005 and ADR-006 boundaries, keep integrations optional, and defer v0.2 governance work.

## Legacy ambiguity

The earlier copilot prototype and documentation may be mistaken for current framework requirements.

Mitigation: preserve that material as history or reference, identify superseded decisions explicitly, and direct navigation to the current project root.

## Implementation duplication

Initialization or validation rules may be reimplemented independently in multiple surfaces.

Mitigation: place shared behavior in Core and keep the CLI focused on invocation and presentation.

## Adoption risk

The structure may impose maintenance effort without providing clear value to project teams.

Mitigation: keep the initial framework small, human-readable, and independently useful; validate it through bounded fixtures and real project adoption before expansion.
