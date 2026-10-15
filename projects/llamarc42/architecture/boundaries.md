# Architecture Boundaries

## Documentation repository

Responsible for:

- authoritative framework documentation and accepted contracts;
- architectural decisions and their lifecycle;
- current project guidance and navigation; and
- preserved proposals, milestone evidence, and superseded history.

Must not:

- depend on executable tooling to remain readable or maintainable;
- grant generated or transient output hidden authority; or
- define implementation behavior outside reviewed documentation.

## Core repository

Responsible for:

- reusable C# implementation of accepted discovery and validation rules;
- the single executable copy of versioned initialization template assets and deterministic selection of the applicable accepted template;
- initialization preflight, deterministic planning, safe application, and structured results and diagnostics; and
- deterministic, read-only inspection where required by contract.

Must not:

- infer architectural meaning or invent project facts;
- modify authoritative documentation during validation;
- own command-line presentation; or
- provide model interaction, chat, sessions, semantic retrieval, orchestration, or a required API.

## CLI repository

Responsible for:

- command invocation, argument handling, and user interaction;
- invoking Core behavior; and
- terminal and machine-readable presentation of Core results and diagnostics.

Must not:

- duplicate template assets or selection, initialization rules, documentation discovery, validation logic, authority classification, or other accepted Core behavior;
- become required for reading or manually maintaining documentation; or
- expand accepted contracts through command behavior.

## Other repositories

`llamarc42/powershell` preserves the original prototype and reference material. It is not a required wrapper, bootstrap, or runtime dependency.

`llamarc42/api` is deferred. No v0.1 framework behavior depends on an API.

`llamarc42.con` is independent. It may consume framework documentation in the future only through a separately accepted, optional integration.

The presence of a repository, checkout, remote, or sibling directory does not make it part of a llamarc42 project. v0.1 does not define cross-repository membership or federation.

## External workflows

External tools may read or produce framework-compatible documentation through visible, reviewable changes. They retain ownership of their own planning, orchestration, implementation, and review workflows. They cannot silently change documentation authority or become framework dependencies.
