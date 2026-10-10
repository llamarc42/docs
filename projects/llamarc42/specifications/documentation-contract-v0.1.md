# llamarc42 Documentation Contract v0.1

Version: 0.1

Status: Accepted

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
- structural document types, authority states, and conflict behavior;
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

### 2.2 Specification acceptance

The `Status` field at the top of this file controls whether this specification
is in force. `Proposed` means the specification is under review and has no
normative effect. Its presence in a branch, pull request, default branch, or
released file set does not by itself make it accepted.

Acceptance requires explicit maintainer approval after the required scenarios
have been verified and a reviewed change of this file's `Status` to `Accepted`.
Until that event, `Authority: Normative upon acceptance` describes intended
future authority only. Before that event, this specification remains
`Proposed`.

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

For v0.1, discovery MUST NOT traverse a child directory entry that the
filesystem identifies as a symbolic link, junction, mount-point reparse point,
or other directory reparse point. The link entry and everything reachable only
through it are excluded from the inventory, even when the target is inside the
project root. If the target is also reachable through a non-link path inside
the root, its documents are inventoried only through that physical path. This
rule prevents cycles, duplicate identities, and platform-specific traversal.

A linked or reparse-point file is not a regular contract document and MUST also
be excluded. The root marker `project.md` MUST itself be a non-link regular
file.

Every contract document path MUST use a portable representation. A path segment
MUST NOT:

- contain ASCII control characters or any of `<`, `>`, `:`, `"`, `\`, `|`,
  `?`, or `*`;
- end in a space or `.`;
- have a case-insensitive filename stem (the portion before the first `.`) of
  `CON`, `PRN`, `AUX`, `NUL`, `COM1` through `COM9`, or `LPT1` through `LPT9`;
  or
- collide with another document path after Unicode NFC normalization and
  Unicode default case-folding.

Framework-defined paths and filenames are case-sensitive ASCII and MUST use the
exact spelling shown in this contract. Variable framework-controlled names
MUST also use their defined portable form: ADR slugs and iteration topic
suffixes use lowercase ASCII kebab-case, years and dates use ASCII digits, and
Markdown extensions use lowercase `.md` except for the exact navigation names
`AGENTS.md` and `README.md`.

Arbitrary project-specific document names MUST satisfy the portable
prohibitions above. They SHOULD use lowercase ASCII kebab-case for consistency,
but that style recommendation is not a conformance requirement.

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

The project document inventory consists of non-link regular Markdown files
reached without traversing excluded entries and physically contained within the
project root. Nested project subtrees are also excluded.

Each document is assigned exactly one structural document type using its path
as defined in sections 5 and 6. Authority state is determined separately; an
ADR remains a decision record whether it is Proposed, Accepted, Rejected,
Withdrawn, or Superseded.

When a complete inventory is presented, document types MUST be grouped in this
order:

1. project descriptor;
2. decision records;
3. current-guidance documents;
4. historical records;
5. navigation;
6. supplemental documentation.

Within a type, documents MUST be ordered by normalized document identity using
ordinal Unicode code-point order. ADRs MUST first be ordered by their numeric
`ADR-NNN` identifier, with normalized path as the tie-breaker. Iteration records
using the preferred convention naturally order by date and then by slug.

Every decision-record inventory entry MUST display its explicit status and
derived authority state. Ordering is for predictable presentation only. It
does not create authority, change authority state, or silently resolve
conflicts.

## 5. Required and optional artifacts

| Path or pattern | Requirement | Document type | Authority determination |
| --- | --- | --- | --- |
| `project.md` | Required | Project descriptor | Current authority by type. |
| `context.md` | Optional | Current guidance | Current authority by type. |
| `constraints.md` | Optional | Current guidance | Current authority by type. |
| `architecture/**/*.md` | Optional | Current guidance | Current authority by type. |
| `specifications/**/*.md` | Optional | Current guidance | Current unless an explicit lifecycle status says otherwise. |
| `domain/**/*.md` | Optional | Current guidance | Current authority by type. |
| `guides/**/*.md` | Optional | Current guidance | Current authority by type. |
| `quality/**/*.md` | Optional | Current guidance | Current authority by type. |
| `decisions/ADR-NNN-*.md` | Optional | Decision record | Determined by explicit status and valid relationships. |
| `history/**/*.md` | Optional | Historical record | Never current authority. |
| `AGENTS.md` | Optional | Navigation | Never current authority. |
| `README.md` | Optional | Navigation | Never current authority. |
| Other `*.md` files | Optional | Supplemental | No framework-assigned authority. |

Optional paths become part of the contract only when present. A project MUST
NOT create an empty ADR, daily log, directory, or placeholder merely to appear
complete.

## 6. Document type and authority

### 6.1 Document types

Document type is a structural classification determined by path. Authority
state is a separate determination made from the type and, where defined, an
explicit lifecycle status and structurally valid relationships. Changing an
ADR status never changes its document type or its inventory group.

#### Project descriptor

`project.md` is the project-descriptor type. It describes project identity,
purpose, scope, and the documentation map. It is not a decision record and
cannot silently change an accepted ADR.

#### Decision records

Every conforming file under `decisions/` is a decision record. Its status does
not move it to the historical-record type. Proposed, Rejected, Withdrawn, and
Superseded ADRs remain visibly grouped and ordered with all other decision
records.

#### Current-guidance documents

Current-guidance documents describe the project's present architecture,
constraints, domain rules, practices, risks, and implementation or operating
guidance.

#### Historical records

Files under `history/`, including iteration records, preserve what was explored,
observed, proposed, or concluded at a point in time. They are evidence for why
the project evolved.

#### Navigation

`AGENTS.md` and `README.md` help a reader find documentation. They MUST NOT be
treated as architectural authority and SHOULD link instead of duplicating
normative content.

#### Supplemental documentation

Markdown files outside contract-defined locations remain discoverable, but the
framework classifies them as supplemental.

### 6.2 Authority state

The project descriptor and current-guidance documents are current authority by
type. A document with an explicit lifecycle status recognized by its governing
specification is current only when that status says it is accepted. Therefore,
this documentation-contract file remains non-authoritative while its own status
is `Proposed` under section 2.2.

A decision record is current authority only when its explicit status is
`Accepted`. Relationship effects apply only when the structural rules in
section 7.3 are complete and valid. Proposed, Rejected, Withdrawn, and
Superseded decision records are non-current decision history; they remain
decision records in the inventory.

Historical records, navigation documents, and supplemental documents are not
current authority. A project that needs supplemental material to become
current authority SHOULD move it to the appropriate current-guidance location
or record the governing decision in an accepted ADR.

### 6.3 Precedence and conflicts

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

`<slug>` MUST match `[a-z0-9]+(?:-[a-z0-9]+)*`.

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

`Proposed` may transition to `Accepted`, `Rejected`, or `Withdrawn` only through
explicit review. `Rejected`, `Withdrawn`, and `Superseded` are terminal for that
decision record. Reconsidering or re-adopting their content requires a new ADR;
historical decisions MUST NOT become current automatically because a later ADR
is removed, rejected, withdrawn, or superseded.

### 7.3 Relationships and supersession

Decision relationships use relative Markdown links that resolve to decision
documents within the same project in these optional sections:

- `## Supersedes` identifies decisions wholly replaced by this ADR.
- `## Amends` identifies accepted decisions changed only in a stated, limited
  respect.
- `## Amended by` identifies accepted ADRs that amend this ADR.
- `## Amendment scope` states the limited scope changed by an amendment.
- `## Related decisions` records context without changing authority.
- `## Superseded by` identifies the accepted successor to a superseded ADR.

Only an `Accepted` ADR can establish a new supersession or amendment. A
non-Accepted ADR MAY describe an intended relationship for review, but that
relationship has no authority effect.

Supersession MUST be explicit and reciprocal when established:

1. The successor ADR MUST have status `Accepted` and link to each predecessor
   under `## Supersedes`.
2. Each predecessor MUST have status `Superseded`, which records that it was
   formerly Accepted, and link to the successor under `## Superseded by`.
3. The predecessor file and its rationale MUST be retained.

When an accepted successor is later superseded, it retains its earlier
`## Supersedes` links and adds its own `## Superseded by` link. Earlier records
continue to point to their immediate successors. A multi-generation chain MUST
remain complete and MUST NOT be collapsed to point every historical ADR at the
newest decision. Only the final Accepted ADR is current; every Superseded ADR
in the chain remains non-current.

For structural evaluation, a Superseded ADR MAY retain outgoing `## Supersedes`
links established while it was Accepted. Those links preserve history but do
not give the Superseded ADR current authority or permit it to establish new
relationships.

An amendment takes effect only when the amending ADR and its target both have
status `Accepted`. The amending ADR MUST link to the target under `## Amends`,
the target MUST link back under `## Amended by`, and the amending ADR MUST state
the exact scope changed in a non-empty `## Amendment scope` section. Both ADRs
remain current: the amendment governs only that explicit scope and the target
remains current elsewhere. If the scope cannot be stated clearly, the newer ADR
SHOULD supersede the older one instead.

Decision relationships MUST NOT be inferred from prose similarity. A missing,
one-sided, or contradictory relationship is an unresolved documentation issue,
not permission to guess. It MUST be reported as a conformance failure. An
invalid relationship has no authority effect: tools MUST NOT infer a missing
status transition, reciprocal link, amendment, successor, or predecessor.

These checks are structural. They require only paths, explicit status values,
required non-empty sections, and reciprocal links. Tools verify the presence of
amendment scope, while humans review whether it is sufficiently exact. The
structural checks do not require semantic conflict detection or interpretation
of decision prose.

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
topic suffix matching `[a-z0-9]+(?:-[a-z0-9]+)*` when multiple records or
clearer topic separation are useful.

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

**Inventory:** One project-descriptor document.

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

**Inventory:** One project descriptor and three current-guidance documents,
grouped and ordered by sections 4.3 and 5.

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

**Inventory:** One project descriptor, one current-guidance document, two
historical records, and one navigation document.

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

**Inventory:** One project descriptor, one Accepted decision record, and one
historical record. The ADR remains in the decision-record group.

**Authority:** The accepted ADR is current decision authority. The remote
storage recommendation is historical evidence and does not override it.

**Discovery:** `project.md` identifies the root. Standard path classification
leads humans and tools to the accepted ADR as current authority and labels the
iteration record as history. If the project adopts remote storage, it must do
so through an explicit new or amended accepted ADR; editing or reordering the
historical record is insufficient.

### 11.5 Rejected ADR alongside an accepted ADR

```text
telemetry/
├── project.md
└── decisions/
    ├── ADR-001-local-events.md       # Accepted
    └── ADR-002-hosted-events.md      # Rejected
```

**Result:** Conforms when both ADRs have valid form and no invalid relationship.

**Inventory:** Both files are decision records and are ordered by identifier.
Each entry displays its distinct status and authority state.

**Authority:** ADR-001 is current authority. ADR-002 is non-current decision
history. Rejection does not change ADR-002's document type or remove it from the
decision-record inventory group.

**Discovery:** Path classification identifies both records; exact status values
determine authority without interpreting their prose.

### 11.6 Accepted ADR amended by another accepted ADR

```text
payments/
├── project.md
└── decisions/
    ├── ADR-001-payment-timeouts.md   # Accepted; Amended by ADR-002
    └── ADR-002-mobile-timeout.md     # Accepted; Amends ADR-001
```

Assume ADR-002 states the exact timeout scope it changes and both links are
reciprocal.

**Result:** Conforms.

**Inventory:** Both files are Accepted decision records ordered by identifier.

**Authority:** Both ADRs are current. ADR-002 governs only its explicit amended
scope; ADR-001 remains current elsewhere. A non-Accepted ADR-002 would have no
amendment effect.

**Discovery:** Status values, relationship headings, links, and the explicit
scope statement are directly inspectable. No semantic comparison is required.

### 11.7 Three-generation supersession chain

```text
storage/
├── project.md
└── decisions/
    ├── ADR-001-files.md              # Superseded by ADR-002
    ├── ADR-002-sqlite.md             # Supersedes ADR-001; Superseded by ADR-003
    └── ADR-003-postgres.md           # Accepted; Supersedes ADR-002
```

**Result:** Conforms when every adjacent relationship is reciprocal and the
former rationale remains in each record.

**Inventory:** All three files remain decision records ordered by identifier.
The inventory shows ADR-001 and ADR-002 as Superseded and ADR-003 as Accepted.

**Authority:** Only ADR-003 is current. ADR-001 and ADR-002 remain non-current
decision history. ADR-001 continues to point to ADR-002, and ADR-002 retains
its link to ADR-001 while also pointing to ADR-003; the chain is not collapsed.

**Discovery:** The complete chain is reconstructed from explicit adjacent links
and statuses. No date, file order, or prose inference is used.

### 11.8 Invalid one-sided supersession

```text
cache/
├── project.md
└── decisions/
    ├── ADR-001-memory-cache.md        # Accepted; no Superseded by link
    └── ADR-002-disk-cache.md          # Accepted; Supersedes ADR-001
```

**Result:** Does not conform. The relationship is one-sided and MUST be
reported. A tool must not repair or complete it implicitly.

**Inventory:** Both files remain Accepted decision records in deterministic
identifier order. The invalid relationship is reported alongside them.

**Authority:** The attempted supersession has no effect. ADR-001 remains
Accepted because its status was not explicitly and validly changed; ADR-002 is
also Accepted for its own recorded decision. Any resulting current-authority
conflict remains visible until the documents are corrected.

**Discovery:** The failure is found by comparing explicit statuses and links,
not by interpreting whether the two decisions conflict semantically.

### 11.9 Directory symlink or junction inside the project

```text
desktop/
├── project.md
└── architecture/
    ├── shared/
    │   └── overview.md
    └── shared-alias -> shared/        # Directory link or junction
```

**Result:** Conforms if the physical documents otherwise conform. Directory
links may exist, but v0.1 never traverses them. A linked `project.md` would not
identify a valid root because the root marker must be a non-link regular file.

**Inventory:** `architecture/shared/overview.md` is included once through its
non-link path. `shared-alias` and its apparent subtree are excluded.

**Authority:** The physical `overview.md` is current guidance. The link creates
no second document identity or authority entry.

**Discovery:** Filesystem link metadata alone determines non-traversal, avoiding
cycles and duplicate enumeration without examining content.

### 11.10 Portability-sensitive filenames and case differences

```text
portable-app/
├── project.md
└── guides/
    ├── CON.md
    ├── API.md
    └── api.md
```

**Result:** Does not conform. `CON.md` uses a platform-reserved basename, and
the other two paths collide after case-folding.

**Inventory:** On a filesystem that can represent all three entries, discovery
records all source paths as current-guidance documents and reports the reserved
name and collision. It MUST NOT merge the case variants or choose one. On a
filesystem that cannot represent the tree, the project cannot claim portable
v0.1 conformance.

**Authority:** `project.md` remains current authority. The invalid guide paths
cannot form a deterministic portable authority inventory until renamed; a
consumer reports them rather than inventing precedence.

**Discovery:** Character checks, reserved-basename checks, NFC normalization,
and case-folding are sufficient. No AI or semantic interpretation is involved.

### 11.11 Deterministic implementation check

All ten scenarios can be evaluated using filesystem entry type, normalized
relative paths, filename patterns, exact status values, and explicit Markdown
relationship links. Content-quality review and semantic conflict detection are
outside the algorithm. A conforming implementation therefore needs no AI,
embedding, database, network service, or hidden state.

## 12. Conformance summary

A project conforms to v0.1 when:

- it has a discoverable `project.md` satisfying section 3;
- its contract documents remain inside the project root and obey section 4;
- every present document has the structural type assigned by its path;
- authority state is derived separately from type and explicit lifecycle state;
- ADRs, when present, have a valid status and follow the structural relationship
  rules in section 7;
- directory links and linked files are excluded without traversal;
- document paths satisfy the portable requirements in section 4.1;
- historical records never claim current authority;
- optional entry points never control discovery or meaning; and
- extensions preserve the standard document types and authority rules.

Conformance does not imply that the documentation is complete, correct, or
free of semantic conflict. The contract makes structure and declared authority
explicit. Human review remains responsible for content quality and for
resolving known contradictions through visible documentation changes.
