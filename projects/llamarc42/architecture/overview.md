# Architecture Overview

llamarc42 v0.1 has three cooperating layers with distinct authority and implementation responsibilities.

## 1. Documentation

The project-owned documentation root contains human-readable current guidance, accepted specifications, architectural decisions, navigation, and history. The accepted documentation contract determines authority and lifecycle.

Documentation is normative. It remains readable and maintainable without executable tooling.

## 2. Core

The C# Core implements reusable behavior governed by accepted contracts:

- deterministic documentation-root discovery;
- structural validation and diagnostics;
- access to framework-controlled templates; and
- safe initialization behavior.

Core does not decide substantive project meaning or own AI, chat, session, retrieval, orchestration, API, or user-interface behavior.

## 3. CLI

The CLI provides command invocation, terminal interaction, and presentation over Core behavior. It does not maintain a separate interpretation of the documentation contracts.

## Repository context

- `llamarc42/docs` owns normative documentation and preserved history.
- `llamarc42/core` owns shared C# behavior governed by that documentation.
- `llamarc42/cli` owns the user-facing command surface.
- `llamarc42/powershell` is retained as historical prototype and reference material.
- `llamarc42/api` is deferred.
- `llamarc42.con` is an independent project with any integration deferred.

Repository existence does not establish participation in v0.1. [ADR-006](../decisions/ADR-006-framework-repository-responsibilities.md) records the authoritative responsibility boundaries.
