# Constraints

## Authority constraints

- Project documentation is the authoritative record of declared project knowledge.
- Authority is determined by the accepted documentation contract, not by a tool, model, conversation, generated summary, or repository proximity.
- Proposed, superseded, and historical material cannot silently become current guidance.
- Changes to accepted contracts and decisions require the documented review and lifecycle process.

## Product constraints

- The framework must remain usable by people without an AI model or software agent.
- Documentation must remain local, human-readable, and directly inspectable.
- Reading and maintaining documentation must not require the CLI, Core, Git, network access, an account, or a hosted service.
- Optional external workflows must not reinterpret, replace, or outrank declared authority.

## Tooling constraints

- Discovery and structural validation must be deterministic and local.
- Validation must be read-only and must not infer substantive meaning.
- Initialization must not overwrite substantive project documentation silently.
- Core owns reusable executable behavior defined by accepted contracts.
- The CLI invokes and presents Core behavior without duplicating its rules.
- Diagnostics must be explainable from visible inputs.

## Scope constraints

- v0.1 covers a standalone documentation root.
- Multi-repository membership, federation, remote discovery, and remote trust are outside v0.1.
- PowerShell is historical prototype and reference material, not a v0.1 dependency.
- The API is deferred and is not a v0.1 component or dependency.
- `llamarc42.con` is independent and is not a framework dependency.
- Model integration, chat, sessions, retrieval ranking, and agent orchestration are outside the framework foundation.
