# llamarc42 Documentation Contract v0.1

Version: 0.1

Status: Proposed

Authority: Normative upon acceptance

## 1. Purpose

This specification defines the minimum documentation contract for a llamarc42
project. It makes current engineering knowledge predictable and discoverable
without requiring AI inference, an agent, network access, a hosted service, a
database, or a source-control provider.

The contract is intentionally small. A project can conform with one file and
can add current guidance, architectural decisions, and historical records only
when they are useful.

This specification uses **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and
**MAY** as normative requirement terms.

## 2. Scope and boundaries

This contract defines:

- project-root and document discovery;
- required and optional documentation artifacts;
- stable document identity and deterministic ordering;
- documentation authority classes and conflict behavior;
- ADR status and explicit historical relationships;
- iteration-history conventions;
- the role of a generic `AGENTS.md` entry point; and
- extension conventions for larger projects.

This contract does not define:

- executable initialization templates;
- validation diagnostics or diagnostic codes;
- Core, CLI, PowerShell, API, or integration behavior;
- migration of existing llamarc42 documentation;
- retrieval ranking, embeddings, or semantic interpretation;
- an agent workflow; or
- GitHub, Jira, source control, or any hosted service as a dependency.

ADR-005 defines the product boundary for this contract. Reconciliation of the
existing copilot-oriented documents and ADR-001 through ADR-004 belongs to
DOC-05 and is not performed here.

### 2.1 Contract versioning

A project selects exactly one documentation-contract version through the
declaration in `project.md`. A later contract version MUST be published as a
separate versioned specification and MUST NOT silently redefine v0.1. Moving a
project to another version requires an explicit change to its declaration and
documentation.

## 3. Project model

### 3.1 Project root

A documentation project root is a directory containing a regular file named
exactly `project.md`.

`project.md` is both the root marker and the required human entry point. It
MUST:

- contain the project name and a concise statement of purpose or scope;
- contain the declaration `Documentation contract: llamarc42/v0.1`; and
- direct a reader to any current documents needed to understand the project.

The declaration identifies the contract version; it does not depend on a
registry or network lookup.

A project MAY exist anywhere in a filesystem. A `projects/` parent directory
and a source-control repository are not required.

### 3.2 Root discovery

Given a starting path, project discovery is deterministic:

1. If the starting path names a file, discovery begins in its parent directory.
2. Examine that directory for `project.md`.
3. If it is absent, examine each parent directory in turn.
4. The first directory containing `project.md` is the project root.
5. If the filesystem root is reached without finding `project.md`, no project
   is discovered.

The nearest root wins. A directory below a project root that contains its own
`project.md` starts a separate nested project. Its subtree MUST NOT be included
in the parent project's document inventory.

Discovery MUST NOT depend on `AGENTS.md`, repository metadata, a remote service,
or interpreting document prose.

### 3.3 Minimum valid project

The minimum valid project is:

```text
example/
└── project.md
```

No ADR, iteration record, architecture hierarchy, agent entry point, or other
artifact is required.

## 4. Paths, identity, and ordering

### 4.1 Relative paths

All document paths are interpreted relative to the discovered project root.
Contract paths use `/` as their written separator on every platform. A platform
MAY use a different native separator when accessing the filesystem.

A document path MUST:

- remain within the project root after normalization;
- contain no `.` or `..` path segment in its normalized form; and
- identify a regular, human-readable Markdown file with the `.md` extension.

Directory links, junctions, or symbolic links that resolve outside the project
root are outside the project and MUST NOT be followed during discovery.

Framework-defined paths and filenames are case-sensitive and MUST use the exact
case specified here. A project MUST NOT contain document paths that differ only
by case or Unicode normalization. This prevents a project from producing a
different inventory on Windows, macOS, and Linux.

Portable project-specific names SHOULD use lowercase ASCII kebab-case. Spaces,
trailing dots or spaces, platform-reserved names, and platform-specific path
syntax SHOULD be avoided.

### 4.2 Stable document identity

The stable identity of an ordinary document is its normalized project-relative
path using `/` separators and Unicode NFC normalization.

An ADR also has a stable decision identifier taken from its filename:
`ADR-NNN`, where `NNN` is a zero-padded decimal number. Changing the descriptive
slug MUST NOT change the decision identifier. A project MUST NOT reuse an ADR
identifier.

An iteration record is identified by its normalized project-relative path. Its
date is historical metadata, not architectural priority.

### 4.3 Deterministic inventory and ordering

The project document inventory consists of Markdown files physically contained
within the project root, excluding nested project subtrees and paths that
resolve outside the root.

Each document is assigned exactly one class using section 6. When a complete
inventory is presented, documents MUST be grouped in this order:

1. project descriptor;
2. current architectural decisions;
3. current guidance;
4. historical evidence;
5. navigation;
6. supplemental documentation.

Within a class, documents MUST be ordered by normalized document identity using
ordinal Unicode code-point order. ADRs MUST first be ordered by their numeric
`ADR-NNN` identifier, with normalized path as the tie-breaker. Iteration records
using the preferred convention naturally order by date and then by slug.

Ordering is for predictable presentation. It does not create authority within
a class and does not silently resolve conflicts.

## 5. Required and optional artifacts

| Path or pattern | Requirement | Class | Purpose |
| --- | --- | --- | --- |
| `project.md` | Required | Project descriptor | Marks the root and states version, purpose, scope, and reading path. |
| `context.md` | Optional | Current guidance | Explains current problem context and non-goals. |
| `constraints.md` | Optional | Current guidance | States current hard constraints. |
| `architecture/**/*.md` | Optional | Current guidance | Describes current architecture and boundaries. |
| `specifications/**/*.md` | Optional | Current guidance | Defines current project contracts. |
| `domain/**/*.md` | Optional | Current guidance | Describes current domain language and invariants. |
| `guides/**/*.md` | Optional | Current guidance | Provides current implementation or operating guidance. |
| `quality/**/*.md` | Optional | Current guidance | Records current risks, tradeoffs, and delivery guidance. |
| `decisions/ADR-NNN-*.md` | Optional | Architectural decision | Records a decision and its status. |
| `history/**/*.md` | Optional | Historical evidence | Preserves investigations, findings, and iteration history. |
| `AGENTS.md` | Optional | Navigation | Directs humans and unfamiliar agents to project documentation. |
| `README.md` | Optional | Navigation | Provides a general human-oriented introduction or pointer. |
| Other `*.md` files | Optional | Supplemental | Holds project-specific material with no framework-assigned authority. |

Optional paths become part of the contract only when present. A project MUST
NOT create an empty ADR, daily log, directory, or placeholder merely to appear
complete.

## 6. Documentation authority

### 6.1 Authority classes

#### Project descriptor

`project.md` is current authoritative guidance for project identity, purpose,
scope, and the documentation map. It is not an architectural decision record
and cannot silently change an accepted ADR.

#### Current architectural decisions

An ADR with status `Accepted` is current authority for the decision it records,
unless it has been explicitly superseded as defined in section 7.

ADRs with any other status are retained decision history and are not current
authority.

#### Current guidance

Current-guidance documents describe the project's present architecture,
constraints, domain rules, practices, risks, and implementation or operating
guidance. Their authority comes from their contract-defined location, not from
an agent, retrieval result, file modification time, or search ranking.

#### Historical evidence

Files under `history/`, including iteration records, preserve what was explored,
observed, proposed, or concluded at a point in time. They are evidence for why
the project evolved; they are not current architectural authority.

#### Navigation

`AGENTS.md` and `README.md` help a reader find documentation. They MUST NOT be
treated as architectural authority and SHOULD link instead of duplicating
normative content.

#### Supplemental documentation

Markdown files outside contract-defined locations remain discoverable, but the
framework assigns them no authority. A project that needs a supplemental file
to be current authority SHOULD move it into the appropriate current-guidance
location or record the governing decision in an accepted ADR.

### 6.2 Precedence and conflicts

Current authority consists of the project descriptor, accepted and
non-superseded ADRs, and current-guidance documents.

Current authority takes precedence over historical evidence, navigation files,
and supplemental documentation. A proposal or conclusion written only in an
iteration record does not change the project. A link from `AGENTS.md` does not
raise the linked file's authority.

Accepted ADRs and current guidance have different roles: an ADR records an
explicit decision, while current guidance describes the project as it now
stands. Neither role permits a reader or tool to conceal a contradiction
between them.

When two current-authority documents conflict, a human or tool MUST:

- identify the conflicting documents by normalized path;
- present the conflict without inventing a reconciliation;
- avoid choosing a statement based only on file order, recency, specificity,
  retrieval rank, or agent judgment; and
- require an explicit documentation change or ADR relationship to resolve it.

The framework MUST NOT use semantic inference to discover or settle prose
conflicts. Authors and reviewers remain responsible for identifying semantic
contradictions. Once known, a conflict MUST remain visible until explicitly
resolved.

## 7. ADR status and decision history

### 7.1 ADR form

An ADR filename MUST match `decisions/ADR-NNN-<slug>.md`. It MUST contain:

- a title containing the same `ADR-NNN` identifier;
- a `## Status` section containing exactly one status from section 7.2;
- context;
- the decision or proposed decision; and
- consequences.

Rationale and explicit relationship sections are optional unless required by a
status transition.

### 7.2 Statuses

- `Proposed`: under review; not current authority.
- `Accepted`: approved and current unless explicitly superseded.
- `Rejected`: considered and declined; historical evidence only.
- `Withdrawn`: removed from consideration before acceptance; historical
  evidence only.
- `Superseded`: formerly accepted but replaced by another accepted ADR;
  historical evidence only.

Status is explicit. It MUST NOT be inferred from ADR number, filename, file
date, commit history, issue state, or the existence of a newer ADR.

### 7.3 Relationships and supersession

Decision relationships use relative Markdown links that resolve to decision
documents within the same project in these optional sections:

- `## Supersedes` identifies decisions wholly replaced by this ADR.
- `## Amends` identifies accepted decisions changed only in a stated, limited
  respect.
- `## Related decisions` records context without changing authority.
- `## Superseded by` identifies the accepted successor to a superseded ADR.

Supersession MUST be explicit and reciprocal:

1. The successor ADR MUST have status `Accepted` and link to each predecessor
   under `## Supersedes`.
2. Each predecessor MUST have status `Superseded` and link to the successor
   under `## Superseded by`.
3. The predecessor file and its rationale MUST be retained.

An amendment MUST state the exact scope changed. Both ADRs remain current for
their non-conflicting scopes. If that scope cannot be stated clearly, the newer
ADR SHOULD supersede the older one instead.

Decision relationships MUST NOT be inferred from prose similarity. A missing,
one-sided, or contradictory relationship is an unresolved documentation issue,
not permission to guess.

## 8. Iteration history

### 8.1 Preferred daily convention

Daily iteration records SHOULD use:

```text
history/
└── iterations/
    └── YYYY/
        ├── YYYY-MM-DD.md
        └── YYYY-MM-DD-short-topic.md
```

Use `YYYY-MM-DD.md` when one record covers the day. Use a lowercase kebab-case
topic suffix when multiple records or clearer topic separation are useful.

The date is the local working date of the record. A project MUST NOT require a
record for every calendar day. Missing dates are normal. Empty or fabricated
records MUST NOT be created for continuity or validation.

### 8.2 Recommended content

An iteration record SHOULD capture useful reasoning such as:

- problems investigated;
- options considered;
- experiments and findings;
- decisions proposed or reached during the iteration; and
- references to relevant ADRs, current guidance, issues, or implementation
  work.

These headings are recommended, not mandatory. The record remains historical
evidence regardless of wording.

A decision described in an iteration record does not become current authority.
If it changes architecture, constraints, or guidance, the project MUST update
the appropriate current-authority document or accept an ADR through its normal
review process.

### 8.3 Additional historical organization

Projects MAY add other structures under `history/`, including investigations,
experiments, meeting notes, migration notes, or imported legacy records. Such
organization MUST NOT create a competing authority class. Everything under
`history/` remains historical evidence.

## 9. Generic agent entry point

A project MAY provide one root-level `AGENTS.md`. Its audience includes humans,
automation, and unfamiliar agents.

`AGENTS.md` SHOULD:

- point first to `project.md`;
- link to relevant constraints, architecture, guides, and accepted ADRs;
- identify `history/` as non-authoritative evidence; and
- use generic language that does not require a specific model, vendor, agent,
  protocol, or service.

`AGENTS.md` SHOULD contain navigation and concise usage instructions only. It
SHOULD NOT duplicate architectural rules, decisions, or constraints because
copies can drift.

Project discovery, conformance, authority, and meaning MUST remain unchanged if
`AGENTS.md` is absent, unread, ignored, or misunderstood. Recognizing the file
is never a prerequisite for using a llamarc42 project.

## 10. Extension conventions

A larger project MAY add any number of documents beneath the optional standard
directories. Subdirectories MAY reflect components, domains, teams, platforms,
or releases.

Extensions MUST preserve the meanings of the top-level classes:

- current material belongs under a current-guidance directory;
- architectural decisions belong under `decisions/`;
- historical material belongs under `history/`;
- navigation files do not become authority; and
- unclassified material remains supplemental.

A project-specific directory MUST NOT redefine `history/` as current authority,
turn `AGENTS.md` into a hidden policy layer, or create a second implicit
precedence system.

Non-Markdown assets MAY accompany documents and be linked from them. They do
not become independently authoritative contract documents unless a later
contract version defines them.

## 11. Verification scenarios

### 11.1 Minimal project with no ADRs or iteration entries

```text
weather-station/
└── project.md
```

**Result:** Conforms when `project.md` contains the required purpose and
`Documentation contract: llamarc42/v0.1` declaration.

**Authority:** `project.md` is the only current authority. There are no ADRs and
no historical documents.

**Discovery:** A human opens `project.md`. A tool or unfamiliar agent searches
upward for `project.md`, discovers the root, and reads the same file. No
`AGENTS.md` is needed.

### 11.2 Current guidance with no ADRs

```text
inventory-service/
├── project.md
├── constraints.md
├── architecture/
│   └── overview.md
└── guides/
    └── development.md
```

**Result:** Conforms. ADRs are optional.

**Authority:** All four files are current authority in their defined roles.
There is no decision history or iteration history.

**Discovery:** `project.md` marks the root and provides the human entry point.
The standard paths classify and deterministically order the remaining current
guidance.

### 11.3 Project with daily iteration history

```text
desktop-client/
├── project.md
├── architecture/
│   └── overview.md
├── history/
│   └── iterations/
│       └── 2026/
│           ├── 2026-10-09.md
│           └── 2026-10-10-window-lifecycle.md
└── AGENTS.md
```

**Result:** Conforms. Two useful dates are recorded; no file is required for
other dates.

**Authority:** `project.md` and `architecture/overview.md` are current
authority. Both iteration files are historical evidence. `AGENTS.md` is
navigation only.

**Discovery:** Humans may begin with `project.md` or `AGENTS.md`. An unfamiliar
agent can discover the project from `project.md`, classify the standard paths,
and reach the same authority result even if it ignores `AGENTS.md`.

### 11.4 Historical investigation conflicts with an accepted ADR

```text
orders/
├── project.md
├── decisions/
│   └── ADR-001-local-storage.md
└── history/
    └── iterations/
        └── 2026/
            └── 2026-10-10-remote-storage.md
```

Assume `ADR-001-local-storage.md` has status `Accepted` and selects local
storage, while the iteration record recommends remote storage after an
experiment.

**Result:** Conforms. A historical disagreement is allowed and preserves useful
reasoning.

**Authority:** The accepted ADR is current decision authority. The remote
storage recommendation is historical evidence and does not override it.

**Discovery:** `project.md` identifies the root. Standard path classification
leads humans and tools to the accepted ADR as current authority and labels the
iteration record as history. If the project adopts remote storage, it must do
so through an explicit new or amended accepted ADR; editing or reordering the
historical record is insufficient.

## 12. Conformance summary

A project conforms to v0.1 when:

- it has a discoverable `project.md` satisfying section 3;
- its contract documents remain inside the project root and obey section 4;
- every present document has the class assigned by its path;
- ADRs, when present, follow section 7;
- historical records never claim current authority;
- optional entry points never control discovery or meaning; and
- extensions preserve the standard authority classes.

Conformance does not imply that the documentation is complete, correct, or
free of semantic conflict. The contract makes structure and declared authority
explicit. Human review remains responsible for content quality and for
resolving known contradictions through visible documentation changes.
