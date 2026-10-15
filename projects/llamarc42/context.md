# Context

## Problem

Project knowledge and engineering intent are frequently distributed across conversations, prompts, source code, issue trackers, and individual experience. That fragmentation makes decisions difficult to discover, review, preserve, and apply consistently as people and tools change.

AI-assisted development increases the cost of this problem when a tool acts without the project's actual constraints or treats transient output as authoritative knowledge.

## Approach

llamarc42 provides a project-owned, documentation-first foundation for recording purpose, context, constraints, architecture, decisions, domain rules, risks, tradeoffs, and direction.

The structure is inspired by arc42 and lightweight ADR practice. The accepted documentation contract defines which artifacts carry current authority, which are navigational, and which preserve history. Deterministic tooling may initialize and validate that structure without interpreting the documentation's substantive meaning.

## Why local-first and model-independent

- Project knowledge remains available without a network, hosted service, or model.
- Human-readable files can be reviewed with ordinary tools.
- A project can change development workflows without migrating its source of authority.
- People and unfamiliar tools can consume the same visible artifacts.

## Goals

- keep authoritative project knowledge explicit, durable, and inspectable;
- make current guidance distinguishable from proposals and history;
- support deterministic initialization, discovery, and structural validation;
- preserve decisions and their evolution; and
- remain independently useful to people and optional external tools.

## Non-goals

- prescribing how software is designed, implemented, or reviewed;
- replacing human judgment or approval;
- providing model inference, conversations, sessions, semantic retrieval, or orchestration;
- requiring Git, a source-control host, a cloud service, or an AI provider; and
- defining multi-repository federation or cross-repository project membership in v0.1.
