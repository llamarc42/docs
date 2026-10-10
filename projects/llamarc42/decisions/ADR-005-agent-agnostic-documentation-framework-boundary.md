# ADR-005: Agent-Agnostic Documentation Framework Product Boundary

## Status
Proposed

## Context

The current authoritative documentation defines llamarc42 as a local AI copilot. It assigns the product responsibility for model interaction, retrieval, conversations, sessions, and multiple interactive surfaces.

The proposed product direction is different: llamarc42 is to be a documentation-first, local-first, agent-agnostic framework for recording and maintaining engineering knowledge. That boundary must be established as an architectural decision before the documentation contract, templates, validation rules, Core responsibilities, or CLI behavior are designed.

Backlog approval and implementation planning do not accept this decision. While this ADR is Proposed, the existing accepted decisions and authoritative documents remain in force.

## Decision

Upon acceptance of this ADR, llamarc42 will adopt a documentation-first, local-first, agent-agnostic engineering documentation framework as its product foundation.

### Purpose and intended users

The framework serves engineers, architects, maintainers, and teams that need durable, inspectable records of a project's purpose, constraints, boundaries, decisions, and evolution.

People must be able to create, read, maintain, review, and use a conforming documentation set without an AI model or software agent. Tools and agents may assist with those activities, but human-only use is a complete and supported mode of operation.

### Framework responsibilities

The framework is responsible for:

- providing a coherent way to organize engineering documentation;
- preserving documentation as the authoritative record of declared project knowledge;
- supporting explicit, reviewable architectural decisions and their status changes;
- keeping current authority distinguishable from historical evidence;
- keeping framework artifacts human-readable and directly inspectable;
- enabling deterministic, local tooling without making that tooling a prerequisite for reading or maintaining the documentation; and
- allowing optional integrations to consume or produce framework-compatible artifacts without transferring authority to those integrations.

The exact documentation contract, authority classes, required and optional artifacts, ordering rules, and discovery rules are deferred to DOC-02. This ADR does not select a template format or define Core or CLI behavior.

### Product guarantees

The foundation:

- operates from local, human-readable files;
- remains usable with network access unavailable;
- requires no model, inference runtime, software agent, agent orchestrator, provider, GitHub account, hosted service, or other external service;
- supports a fully human workflow;
- keeps authoritative inputs and recorded decisions inspectable;
- makes changes to authority explicit and reviewable; and
- preserves superseded decisions and historical reasoning so that architectural evolution remains traceable.

### Optional integration boundary

An unfamiliar agent may consume framework documentation using the same visible artifacts available to a person. Agent-specific entry points, instructions, adapters, and integrations are optional conveniences.

Framework discovery, validity, authority, and meaning must not depend on:

- the presence of an agent-specific file;
- an agent recognizing or following such a file;
- agent-specific capabilities or hidden state; or
- access to a particular model, provider, protocol, or service.

Optional integrations must not silently replace, reinterpret, or outrank the documentation's declared authority.

### Explicit non-goals

The framework does not provide or require:

- AI inference, model hosting, model selection, or provider integration;
- chat, conversations, prompts, or session management;
- retrieval ranking, embeddings, semantic search, or context-window management;
- autonomous agents, agent orchestration, or agent-specific workflow semantics;
- token accounting or model-usage accounting;
- an API or hosted service;
- a GitHub-dependent workflow;
- automatic architectural decisions or replacement of human judgment;
- a documentation contract, template format, validation specification, or CLI design in this decision; or
- migration or compatibility guarantees for the original copilot implementation.

These exclusions define the product foundation. A future optional integration may address an excluded capability only through a separate accepted decision and without making that capability a framework dependency.

### llamarc42.con

`llamarc42.con` is retained as an independent project. It is not part of the initial framework, is not a framework dependency, and is not the reference definition of framework behavior. Any future integration is deferred and requires a separate decision after the framework contract is established.

### Authority, supersession, and history

This ADR does not silently rewrite or erase the original copilot architecture. The existing constitution, principles, project definition, architecture documents, invariants, quality documents, and ADR-001 through ADR-004 remain readable as the record of that design.

The original copilot and the proposed documentation framework are distinct product scopes. Accepting this ADR does not relabel the copilot implementation or its historical decisions as framework behavior.

If this ADR is accepted, its product boundary becomes the basis for subsequent reconciliation. Conflicting accepted decisions and authoritative documents require explicit, reviewable dispositions under DOC-05. Until those dispositions are accepted, their conflicts remain recorded rather than being concealed by incidental edits.

## Architectural Conflicts Identified

The following current authoritative statements conflict with this proposed boundary:

- `README.md`, `projects/llamarc42/project.md`, and `projects/llamarc42/context.md` define llamarc42 as an AI copilot or platform centered on architecture-aware conversations.
- `global/constitution.md` and `global/principles.md` contain model, retrieval, conversation, session, and multi-surface requirements. Their documentation-authority, local-first, transparency, and simplicity principles remain aligned with this decision, but their copilot-specific requirements do not.
- `projects/llamarc42/constraints.md`, `architecture/overview.md`, `architecture/boundaries.md`, and `domain/invariants.md` require model runtimes, retrieval, sessions, prompts, or shared interactive surfaces that are excluded from the framework foundation.
- ADR-002 assigns model interaction to a C# Core engine, ADR-003 adopts a conversation-session storage model, and ADR-004 adopts an API facade over the copilot Core. Those responsibilities are outside this product boundary.
- ADR-001 reflects the original repository naming and role assumptions. The multi-repository separation may remain useful, but repository names and responsibilities require explicit reconciliation.
- The current roadmap, risks, and tradeoffs are organized around delivery of the original copilot and therefore do not describe the framework direction.

DOC-05 will apply explicit dispositions to the prior ADRs and reconcile the authoritative documentation. No disposition is made here beyond establishing the proposed product boundary.

## Rationale

Engineering documentation remains useful when no automation is available and when tools change. Making local, readable documentation the product foundation protects that value, preserves user control, and prevents any agent or provider from becoming an architectural dependency.

An optional-integration boundary also allows people and unfamiliar tools to benefit from the same documented knowledge without granting those tools hidden authority. Explicit supersession and retained history prevent the new direction from obscuring why the original copilot architecture existed.

## Consequences

### Positive

- The framework remains useful offline and without specialized software.
- Human workflows and agent-assisted workflows share visible source material.
- Model, provider, network, account, and hosted-service dependencies are excluded from the foundation.
- Documentation authority and architectural change remain inspectable and traceable.
- Future tools can evolve independently behind an explicit optional boundary.

### Negative

- Existing accepted documentation will remain internally inconsistent until DOC-05 is completed.
- Copilot capabilities are no longer part of the framework foundation and would require separately governed integrations.
- The documentation contract and executable behavior remain intentionally unspecified until follow-up decisions are accepted.

## Deferred Decisions

- DOC-02 will define the versioned documentation contract, authority classes, required and optional artifacts, generic entry-point role, deterministic discovery, and historical iteration records.
- DOC-05 will dispose of ADR-001 through ADR-004 and reconcile the constitution, principles, project definition, architecture, constraints, terminology, repository roles, risks, tradeoffs, and roadmap.
- Template packaging, validation rules, Core implementation, CLI behavior, migration, and optional integrations require later work and are not decided by this ADR.

## Verification

The normative statements in this ADR were reviewed against DOC-01's exclusions:

- No model, agent, network, GitHub account, or external service is required.
- Human-only use is explicitly complete and supported.
- Agent-specific files and integrations are explicitly optional.
- Excluded capabilities are listed explicitly.
- `llamarc42.con` integration is explicitly deferred.
- The ADR does not define the documentation contract, template format, Core design, CLI design, or implementation.
- Existing decisions and authoritative documents are identified rather than silently rewritten.
