# Tradeoffs

## Project-owned documentation vs transient context

- Project-owned documentation is durable, reviewable, and tool-independent.
- Conversations and generated summaries are convenient but transient and may omit governing context.

Decision: project-owned documentation carries declared authority; transient context may assist but cannot replace it.

## Deterministic structure vs semantic inference

- Structural rules can be validated consistently from visible inputs.
- Semantic interpretation could offer richer assistance but depends on judgment and may vary by tool.

Decision: v0.1 tooling validates accepted structural contracts and leaves substantive meaning to human review.

## C# Core vs PowerShell prototype

- C# supports reusable, cross-platform contract behavior and testable domain types.
- PowerShell enabled rapid exploration and remains useful reference material.

Decision: implement shared v0.1 behavior in C# Core; preserve PowerShell as historical prototype and reference, not a dependency.

## Local files vs hosted services

- Local human-readable files preserve control, portability, offline use, and direct inspection.
- Hosted services may add collaboration conveniences but introduce external availability and account dependencies.

Decision: local files are the framework foundation; hosted services may be optional consumers but are never required authority.

## Minimal v0.1 vs early federation

- A standalone root makes the first contract bounded and testable.
- Multi-repository federation could address broader projects but adds identity, trust, availability, and lifecycle questions.

Decision: v0.1 covers standalone roots; federation remains later governance work and cannot be inferred from repository topology.

## Tooling convenience vs independent documentation

- Core and CLI can make initialization and validation repeatable.
- Requiring those tools would reduce the documentation's independent value.

Decision: tooling supports the documentation contract, while people can still read and maintain conforming documentation without it.
