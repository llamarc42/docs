# llamarc42 — docs

This repository is the documentation root for **llamarc42**: a local-first, documentation-driven AI copilot for software engineers.

llamarc42 does not rely on model training to understand your project. Instead, it reads structured documentation that *you* maintain and uses it as the source of truth in every conversation.

> The model does not know your system — your documentation does.

---

## How this repository works

The repository is organised into two main areas:

### `global/`

Documents in `global/` are applied to **every project**. They define the foundational rules, shared language, and design principles that govern all interactions with llamarc42.

| File | Purpose |
|---|---|
| `constitution.md` | Non-negotiable system rules |
| `principles.md` | Design principles guiding system behaviour |
| `glossary.md` | Shared terminology used across all projects |

You should treat these as stable. They rarely need to change unless the core philosophy of your setup changes.

---

### `projects/<project-name>/`

Each project gets its own folder under `projects/`. The folder name is the project identifier that llamarc42 uses to scope sessions and retrieve context.

Within a project folder you define the documentation that **governs and should be adhered to** for that project. llamarc42 retrieves these files during conversations to ground the model in your specific architecture, decisions, and constraints.

#### Recommended project structure

```
projects/
└── your-project/
    ├── project.md              # What the project is, who it is for, core use cases
    ├── context.md              # Problem statement, motivation, non-goals
    ├── constraints.md          # Hard constraints: runtime, architectural, behavioural
    ├── architecture/
    │   ├── overview.md         # High-level architecture description
    │   └── boundaries.md       # Component responsibilities and what they must not do
    ├── decisions/
    │   └── ADR-NNN-title.md    # Architecture Decision Records (one per decision)
    ├── domain/
    │   └── invariants.md       # Rules that must always hold true in the domain
    └── quality/
        ├── roadmap.md          # Planned phases or milestones
        ├── risks.md            # Known technical, product, and architectural risks
        └── tradeoffs.md        # Explicit tradeoff decisions and their rationale
```

None of these files are mandatory — start with what you have. The more you provide, the more grounded and useful the conversations become.

---

## File guidance

### `project.md`

Describe what the project is, who it is for, and what it is not. Keep it short and honest. This is often the first document retrieved and sets the framing for all conversations.

### `context.md`

Explain the problem being solved, why the approach was chosen, and what is explicitly out of scope. This prevents the model from proposing solutions that contradict your reasoning.

### `constraints.md`

List hard constraints. These are rules the system must not violate: runtime requirements, technology choices, data handling rules, interface requirements. Be explicit — the model will defer to these.

### `architecture/overview.md`

Describe the major components or layers of the system and how they relate. Diagrams are welcome but not required — prose works well here.

### `architecture/boundaries.md`

Define what each component is responsible for and, equally importantly, what it must not do. Boundary rules are especially useful for preventing architectural drift during AI-assisted development.

### `decisions/ADR-NNN-title.md`

One file per architecture decision. Use a lightweight ADR format: status, context, decision, rationale, consequences. These are retrieved when the model needs to validate a design proposal or explain a past choice.

### `domain/invariants.md`

Rules that must always hold true regardless of implementation. These form a contract the model should never suggest violating.

### `quality/roadmap.md`

Phases or milestones the project intends to move through. Helps the model give phase-appropriate advice rather than suggesting premature optimisation or out-of-sequence work.

### `quality/risks.md`

Known risks, by category. Giving the model visibility of risks encourages it to flag concerns proactively.

### `quality/tradeoffs.md`

Explicit tradeoff decisions: what was considered, what was chosen, and why. Prevents the model from re-opening settled debates or proposing alternatives that were already evaluated.

---

## The `llamarc42` project

The `projects/llamarc42/` folder in this repository documents the llamarc42 project itself. It serves as a **working example** of the documentation structure described above.

You are encouraged to explore these files to understand the format and level of detail that works well. When setting up your own project, generate your own documents tailored to your specific architecture, constraints, and decisions — do not copy the llamarc42 content directly.

---

## Tooling configuration

### `tooling/config/retrieval.yml`

This file controls the retrieval policy: which documents are loaded into context depending on the intent of the request. Intents include `planning`, `coding`, `review`, and `general`. Each intent specifies which folders and files are prioritised and how many files may be included.

You can tune this file to match your project structure and the way you work. The defaults are a reasonable starting point.

---

## Getting started

1. Create a folder under `projects/` with your project name.
2. Add a `project.md` and `context.md` to describe what you are building and why.
3. Add a `constraints.md` with any hard rules the model should respect.
4. Grow the documentation over time as decisions are made and architecture evolves.
5. Use the global documents as-is, or extend them if your setup requires additional shared rules.

The documentation does not need to be complete to be useful. Start with the essentials and let it grow alongside the project.