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

- reusable C# implementation of accepted discovery, validation, template, and initialization rules;
- deterministic, read-only inspection where required by contract; and
- structured outcomes that user-facing tools can present consistently.

Must not:

- infer architectural meaning or invent project facts;
- modify authoritative documentation during validation;
- own command-line presentation; or
- provide model interaction, chat, sessions, semantic retrieval, orchestration, or a required API.

## CLI repository

Responsible for:

- user-facing command parsing and terminal experience;
- invoking Core behavior; and
- presenting results and diagnostics.

Must not:

- duplicate or reinterpret accepted Core rules;
- become required for reading or manually maintaining documentation; or
- expand accepted contracts through command behavior.

## Other repositories

`llamarc42/powershell` preserves the original prototype and reference material. It is not a required wrapper, bootstrap, or runtime dependency.

`llamarc42/api` is deferred. No v0.1 framework behavior depends on an API.

`llamarc42.con` is independent. It may consume framework documentation in the future only through a separately accepted, optional integration.

The presence of a repository, checkout, remote, or sibling directory does not make it part of a llamarc42 project. v0.1 does not define cross-repository membership or federation.

## External workflows

External tools may read or produce framework-compatible documentation through visible, reviewable changes. They retain ownership of their own planning, orchestration, implementation, and review workflows. They cannot silently change documentation authority or become framework dependencies.
