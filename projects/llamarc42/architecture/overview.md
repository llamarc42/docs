# Architecture Overview

llamarc42 v0.1 has three cooperating layers with distinct authority and implementation responsibilities.

## 1. Documentation

The project-owned documentation root contains human-readable current guidance, specifications, architectural decisions, navigation, and history. Documentation Contract v0.1 determines which documents carry current authority and how lifecycle state affects that authority.

The project descriptor, current-guidance documents whose lifecycle state permits authority, and accepted, non-superseded decisions carry current authority as defined by the contract. Proposed or superseded material, history, navigation, supplemental documents, inventory membership, proximity, and links do not independently create authority. All documentation remains readable and maintainable without executable tooling.

## 2. Core

The C# Core implements reusable behavior governed by accepted contracts:

- deterministic documentation-root discovery, validation, and diagnostics;
- the single executable copy of versioned initialization template assets and deterministic template selection;
- initialization preflight, planning, safe application, and structured results and diagnostics.

Core does not decide substantive project meaning or own AI, chat, session, retrieval, orchestration, API, or user-interface behavior.

## 3. CLI

The CLI provides command invocation, argument handling, user interaction, and terminal and machine-readable presentation over Core behavior. It does not duplicate template selection, initialization rules, discovery, validation, authority classification, or the documentation contracts.

## Repository context

- `llamarc42/docs` owns normative documentation and preserved history.
- `llamarc42/core` owns shared C# behavior governed by that documentation.
- `llamarc42/cli` owns the user-facing command surface.
- `llamarc42/powershell` is retained as historical prototype and reference material.
- `llamarc42/api` is deferred.
- `llamarc42.con` is an independent project with any integration deferred.

Repository existence does not establish participation in v0.1. [ADR-006](../decisions/ADR-006-framework-repository-responsibilities.md) records the authoritative responsibility boundaries.
