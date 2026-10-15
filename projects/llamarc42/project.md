# llamarc42

Documentation contract: llamarc42/v0.1

llamarc42 is a documentation-first framework for establishing, organizing, maintaining, and validating authoritative project knowledge.

## Purpose

Engineering intent often exists only in conversations, prompts, or individual experience. llamarc42 gives a project a durable, inspectable place to record its purpose, architecture, constraints, decisions, conventions, and direction.

The framework is inspired by arc42's emphasis on structured architectural communication. It does not require formal arc42 conformance or prescribe how a project must design, implement, or review its software.

## Intended users

- engineers and architects maintaining project knowledge;
- maintainers reviewing decisions and change history;
- teams that need consistent documentation across changing tools and contributors; and
- software tools or AI agents that consume the same project-owned documentation available to people.

## Product boundary

The documentation belongs to the project and remains authoritative independently of any developer, model, agent, service, or workflow that consumes it. The framework is local-first, human-readable, and model-independent.

The CLI and Core may initialize the foundational structure and validate accepted structural rules. People remain responsible for the substance, maintenance, review, and acceptance of project documentation.

External workflows may use the documentation to support specification, planning, implementation, or review. They do not become part of the framework and cannot silently replace or outrank documented authority.

The framework does not provide AI inference, conversations, sessions, retrieval ranking, agent orchestration, or a required API or hosted service.

## Guiding principle

> We aren't building a framework that knows how to use every AI tool. We're building a documentation framework that any sufficiently capable AI tool should be able to use.

## Documentation navigation

- [Documentation Contract v0.1](specifications/documentation-contract-v0.1.md)
- [Standalone Initialization Contract v0.1](specifications/standalone-initialization-contract-v0.1.md)
- [Deterministic Validation Contract v0.1](specifications/deterministic-validation-contract-v0.1.md)
- [Architecture overview](architecture/overview.md)
- [Architecture boundaries](architecture/boundaries.md)
- [Constraints](constraints.md)
- [ADR-005: Agent-Agnostic Documentation Framework Product Boundary](decisions/ADR-005-agent-agnostic-documentation-framework-boundary.md)
- [Proposed ADR-006: Framework Repository Responsibilities and Architectural Reconciliation](decisions/ADR-006-framework-repository-responsibilities.md)
- [Roadmap](quality/roadmap.md)
