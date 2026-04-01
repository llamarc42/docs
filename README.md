# llamarc42 — docs

This repository is the **source-of-truth documentation workspace*- for **llamarc42**: a local-first, documentation-driven AI copilot for software engineers.

llamarc42 does not rely on model training to understand your project. Instead, it reads structured documentation that *you- maintain and uses it as the source of truth in every conversation.

> The model does not know your system — your documentation does.

---

## What this repository is for

This repository holds the documentation that llamarc42 uses to ground conversations and project-specific reasoning.

It is organized into:

- **global documents*- that apply to every project
- **project documents*- that define the truth for a specific project
- **tooling configuration*- that controls how documentation is retrieved

This repository is not just reference material. It is part of the system.

---

## How this repository works

The repository is organized into two main content areas plus tooling configuration.

### `global/`

Documents in `global/` are applied to **every project**. They define the foundational rules, shared language, and design principles that govern all interactions with llamarc42.

| File | Purpose |
| --- | --- |
| `constitution.md` | Non-negotiable system rules |
| `principles.md` | Design principles guiding system behavior |
| `glossary.md` | Shared terminology used across all projects |

You should treat these as relatively stable. They rarely need to change unless the core philosophy of your setup changes.

---

### `projects/<project-name>/`

Each project gets its own folder under `projects/`. The folder name is the project identifier that llamarc42 uses to scope conversations, sessions, and retrieval.

Within a project folder you define the documentation that **governs*- that project. llamarc42 retrieves these files during conversations to ground the model in your specific architecture, decisions, constraints, and terminology.

#### Recommended project structure

```text
projects/
└── your-project/
    ├── project.md              # What the project is, who it is for, core use cases
    ├── context.md              # Problem statement, motivation, non-goals
    ├── constraints.md          # Hard constraints: runtime, architectural, behavioral
    ├── architecture/
    │   ├── overview.md         # High-level architecture description
    │   └── boundaries.md       # Component responsibilities and what they must not do
    ├── decisions/
    │   └── ADR-001-title.md    # Architecture Decision Records (one per decision)
    ├── domain/
    │   └── invariants.md       # Rules that must always hold true in the domain
    └── quality/
        ├── roadmap.md          # Planned phases or milestones
        ├── risks.md            # Known technical, product, and architectural risks
        └── tradeoffs.md        # Explicit tradeoff decisions and rationale
````

None of these files are strictly mandatory, but the following are **strongly recommended*- as a starting point:

- `project.md`
- `context.md`
- `constraints.md`

The more you provide, the more grounded and useful the conversations become.

---

## File guidance

### `project.md`

Describe what the project is, who it is for, and what it is not. Keep it short and honest. This is often one of the first documents retrieved and helps frame the conversation.

### `context.md`

Explain the problem being solved, why the approach was chosen, and what is explicitly out of scope. This reduces the chance of the model proposing solutions that contradict your reasoning.

### `constraints.md`

List hard constraints. These are rules the system must not violate: runtime requirements, technology choices, data handling rules, interface requirements, compliance limits, operational realities. Be explicit — the model should defer to these.

### `architecture/overview.md`

Describe the major components or layers of the system and how they relate. Diagrams are welcome but not required. Clear prose works well here.

### `architecture/boundaries.md`

Define what each component is responsible for and, equally importantly, what it must not do. Boundary rules are especially useful for preventing architectural drift during AI-assisted development.

### `decisions/ADR-NNN-title.md`

One file per architecture decision. Use a lightweight ADR format such as:

- status
- context
- decision
- rationale
- consequences

These are retrieved when the model needs to validate a design proposal, explain a past choice, or avoid reopening settled decisions.

### `domain/invariants.md`

Rules that must always hold true regardless of implementation. These form a contract the model should never suggest violating.

### `quality/roadmap.md`

Phases or milestones the project intends to move through. This helps the model give phase-appropriate advice rather than suggesting premature optimization or out-of-sequence work.

### `quality/risks.md`

Known risks, by category. Giving the model visibility into risks encourages it to surface concerns proactively.

### `quality/tradeoffs.md`

Explicit tradeoff decisions: what was considered, what was chosen, and why. This helps prevent the model from re-litigating already-settled debates.

---

## Writing guidance

These documents work best when they are:

- explicit rather than implied
- concise but concrete
- updated when decisions change
- written for both humans and retrieval

A good rule of thumb:

- put durable truth in docs
- put temporary discussion in sessions
- put final decisions in ADRs

Documentation does not need to be polished to be useful. It does need to be clear.

---

## The `llamarc42` project

The `projects/llamarc42/` folder in this repository documents the llamarc42 project itself. It serves as a **working example*- of the structure described above.

A typical structure looks like:

```text
projects/
└── llamarc42/
    ├── project.md
    ├── context.md
    ├── constraints.md
    ├── architecture/
    ├── decisions/
    ├── domain/
    └── quality/
```

You are encouraged to explore these files to understand the format and level of detail that works well.

When setting up your own project, write documents that reflect **your-architecture**, **your-constraints**, and **your-decisions**. Do not copy llamarc42 documents verbatim.

---

## Tooling configuration

### `tooling/config/retrieval.yml`

This file controls the retrieval policy: which documents are loaded into context depending on the intent of the request.

Current intents include:

- `planning`
- `coding`
- `review`
- `general`

Each intent specifies which folders and files are prioritized and how many files may be included.

You can tune this file to match your project structure and the way you work. The defaults are a reasonable starting point.

> Current retrieval is policy-driven and file-based. It does not yet use embeddings or semantic vector search.

---

## Sessions and runtime behavior

The docs in this repository are used at runtime by the llamarc42 engine.

In a typical project workflow:

- conversations are scoped to a project
- sessions are stored under that project
- retrieved documentation is injected into each request

Project sessions are typically stored under:

```text
projects/<project-name>/.sessions/
```

The `.sessions/` folder is runtime state, not durable project documentation, and should be treated differently from the documentation files themselves.

---

## Getting started

1. Create a folder under `projects/` with your project name.
2. Add a `project.md` and `context.md` to describe what you are building and why.
3. Add a `constraints.md` with any hard rules the model should respect.
4. Add architecture docs and ADRs as the system evolves.
5. Use the global documents as-is, or extend them if your setup requires additional shared rules.
6. Tune `tooling/config/retrieval.yml` once your project structure stabilizes.

The documentation does not need to be complete to be useful. Start with the essentials and let it grow alongside the project.
