# llamarc42 documentation

This repository contains the authoritative documentation and preserved history for the llamarc42 documentation framework.

Start with [the llamarc42 project definition](projects/llamarc42/project.md). That file declares the applicable documentation contract and links to the current contracts, architecture, decisions, constraints, and roadmap.

## What llamarc42 is

llamarc42 is a documentation-first, local-first, agent-independent framework for organizing and maintaining authoritative project knowledge. The documentation belongs to the project and remains useful to people and tools without requiring a particular model, provider, service, or workflow.

Executable tooling may initialize the framework structure and validate accepted structural rules. It does not replace human ownership of documentation or infer substantive project decisions.

## Repository navigation

- [`projects/llamarc42/project.md`](projects/llamarc42/project.md) — project entry point and current navigation
- [`projects/llamarc42/specifications/`](projects/llamarc42/specifications/) — accepted framework contracts
- [`projects/llamarc42/decisions/`](projects/llamarc42/decisions/) — architectural decisions and lifecycle history
- [`projects/llamarc42/architecture/`](projects/llamarc42/architecture/) — current architecture and boundaries
- [`projects/llamarc42/domain/`](projects/llamarc42/domain/) — current invariants
- [`projects/llamarc42/quality/`](projects/llamarc42/quality/) — current risks, tradeoffs, and roadmap
- [`projects/llamarc42/history/`](projects/llamarc42/history/) — proposals and evidence that never carry current authority

Authority and lifecycle are defined by [Documentation Contract v0.1](projects/llamarc42/specifications/documentation-contract-v0.1.md), not by this repository README.

## Legacy material

The top-level [`global/`](global/) documents and [`tooling/config/retrieval.yml`](tooling/config/retrieval.yml) describe the earlier local-copilot prototype. They are outside the declared `projects/llamarc42/` documentation root, are retained as historical reference, and do not define current v0.1 authority.

Prototype and implementation repositories are governed by their own accepted responsibilities. Repository existence or filesystem proximity does not make a repository part of a documentation project.
