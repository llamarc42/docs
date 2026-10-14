# Deterministic Validation Contract v0.1

Version: 0.1

Status: Proposed

Document type: Historical proposal

Authority: None. Its `history/` path classifies it as a historical record under
Documentation Contract v0.1. `Status: Proposed` is descriptive metadata only.

Tracks: [DOC-04](https://github.com/llamarc42/docs/issues/10)

## 1. Purpose and scope

This proposal defines a finite, read-only validation contract for projects that
select Documentation Contract `llamarc42/v0.1`. If accepted and promoted to
current guidance, it will govern the validation result model, diagnostic
schema, stable diagnostic codes, rule evaluation, authority derivation, and
deterministic ordering shared by Core and CLI implementations.

The proposal validates declared structure and metadata. It does not decide
whether prose is true, complete, well written, architecturally correct, or
semantically consistent.

The requirement terms **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and
**MAY** describe behavior proposed for later acceptance. They have no normative
effect while this file remains historical.

## 2. Governing inputs and ownership

The accepted inputs are:

- [Documentation Contract v0.1](../../../specifications/documentation-contract-v0.1.md),
  which defines project discovery, document identity, paths, document types,
  authority, ADR structure and relationships, iteration history, and generic
  navigation; and
- [Standalone Initialization Contract v0.1](../../../specifications/standalone-initialization-contract-v0.1.md),
  which supplies the accepted `project.md` scaffold, the narrow
  documentation-contract declaration grammar, target-safety precedent, and the
  boundary between incomplete initialized content and a conforming project.

[ADR-005](../../../decisions/ADR-005-agent-agnostic-documentation-framework-boundary.md)
requires validation to remain local-first, human-inspectable, model-independent,
and usable without a network, hosted service, source-control provider, or
agent.

DOC-04 owns the finite validation rules and the implementation-neutral result
model. Core later owns their single executable implementation under
[CORE-02](https://github.com/llamarc42/core/issues/2). CLI later owns invocation,
rendering, and exit-code mapping under
[CLI-01](https://github.com/llamarc42/cli/issues/20) and
[CLI-04](https://github.com/llamarc42/cli/issues/23).

This proposal does not resolve the accepted architecture conflicts assigned to
[DOC-05](https://github.com/llamarc42/docs/issues/11), alter accepted ADRs, or
introduce the v0.2 DocRoot/RepoRoot model preserved by
[DOC-07](https://github.com/llamarc42/docs/issues/20).

## 3. Validation boundary

Validation is a deterministic, read-only operation over one discovered v0.1
project. It MUST:

1. discover the project root using Documentation Contract v0.1;
2. build the complete contract-document inventory without traversing excluded
   links or nested projects;
3. derive structural document type and authority state;
4. evaluate every applicable rule in the catalog;
5. return all non-suppressed diagnostics in the ordering defined by section 10;
   and
6. select an aggregate outcome without interpreting diagnostic prose.

Validation MUST NOT:

- create, modify, rename, delete, normalize, or repair any filesystem entry;
- follow a symbolic link, junction, mount-point reparse point, or other
  link-like entry while discovering or inventorying documents;
- access a remote URL to test availability or trust;
- inspect source-control metadata;
- compare prose for meaning, conflict, quality, or architectural correctness;
- use a model, embedding, agent, retrieval rank, or hidden state;
- infer missing metadata, relationships, authority, or lifecycle state; or
- validate repository federation, cross-root relationships, or multi-repository
  governance.

The same accessible filesystem state and validation-contract version MUST
produce the same inventory, authority states, outcome, diagnostic values, and
diagnostic order.

## 4. Validation result model

The Core-level result is implementation-neutral. It is not a JSON or terminal
output contract.

### 4.1 Result fields

A completed result MUST contain:

| Field | Meaning |
| --- | --- |
| `contractVersion` | Exact value `llamarc42/v0.1`. |
| `outcome` | Exactly one of `Valid`, `Invalid`, or `Error`. |
| `projectRoot` | Normalized absolute root when discovery established one; otherwise absent. |
| `documents` | Complete ordered inventory entries available before any tool failure. |
| `diagnostics` | Complete ordered conformance diagnostics available before any tool failure. |
| `toolError` | Present only for `Error`; a stable tool-error code and actionable message. |

The model MAY retain the supplied starting path separately from the normalized
root. It MUST NOT expose document contents merely to explain a result.

### 4.2 Inventory entry

Each inventory entry MUST contain:

| Field | Meaning |
| --- | --- |
| `path` | Normalized project-relative identity using `/` and Unicode NFC. |
| `documentType` | `ProjectDescriptor`, `DecisionRecord`, `CurrentGuidance`, `HistoricalRecord`, `Navigation`, or `Supplemental`. |
| `authorityState` | `Current`, `NonCurrent`, `None`, or `Indeterminate`. |
| `lifecycleStatus` | An explicitly recognized status when one exists; otherwise absent. |
| `decisionId` | `ADR-NNN` for a structurally identifiable ADR; otherwise absent. |

`Indeterminate` means structural defects prevent a unique authority result. It
MUST NOT be used merely because a document's prose is incomplete or disputed.

### 4.3 Diagnostic

Every diagnostic MUST contain:

| Field | Requirement |
| --- | --- |
| `code` | One stable code from section 9. |
| `severity` | `Error` or `Warning` as fixed by that code. |
| `message` | Deterministic, non-empty, actionable text satisfying the code's message requirement. |
| `path` | Normalized project-relative path when the condition belongs to one document; otherwise absent. |
| `line` | One-based source line when section 9 requires a precise content location; otherwise absent. |
| `relatedPaths` | Zero or more normalized related paths, de-duplicated and ordinally ordered. |

Consumers MUST use `code`, `severity`, and `outcome` for control flow. They MUST
NOT parse `message` to determine program behavior.

### 4.4 Aggregate outcomes

| Outcome | Meaning |
| --- | --- |
| `Valid` | Validation completed and produced no `Error` diagnostic. Warnings MAY be present. |
| `Invalid` | Validation completed and produced at least one `Error` diagnostic. |
| `Error` | A tool or environmental failure prevented complete validation. The result MUST NOT claim conformance or non-conformance. |

CLI process exit codes are deferred to CLI-01. `Error` is not a stronger form of
`Invalid`; it means the conformance question was not completely evaluated.

## 5. Source and parsing conventions

### 5.1 Text and line model

Contract Markdown MUST be readable as UTF-8, with or without a UTF-8 byte-order
mark. LF and CRLF are equivalent line endings. A decoded source line is numbered
from one. Other encodings are invalid under `LC42V100` rather than decoded using
platform defaults.

Markdown blocks, ATX headings, inline links, and reference-style links are
recognized using CommonMark 0.31.2 semantics. An implementation MAY use any
parser that produces equivalent results and MUST NOT require network access at
runtime.

Raw HTML links, URLs written only as prose, image destinations, and content
inside code blocks do not create contract references. Heading fragments are
not validated because fragment generation is renderer-specific.

### 5.2 Header metadata

A header metadata field is a source line of the form `Name: value` after the
document's first level-one heading and before its first level-two or deeper
heading. Blank lines may appear between fields. Field names and values are
case-sensitive unless a rule explicitly says otherwise.

Metadata-like text in blockquotes, lists, tables, code blocks, or later sections
is not header metadata.

### 5.3 Non-empty sections

A required section is non-empty when it contains, before the next heading of
the same or higher level, at least one non-whitespace Markdown source line that
is not only an HTML comment. Validation checks presence, not semantic adequacy.

### 5.4 Local link resolution

A parsed link destination is external when it begins with a URI scheme matching
`[A-Za-z][A-Za-z0-9+.-]*:` or with `//`, except that `file:` destinations and
Windows drive-absolute forms are always invalid local locators. External
destinations are not fetched or validated.

A destination consisting only of `#fragment` is permitted without fragment
validation. Every other destination without an external marker is local. For a
local destination, validation MUST:

1. remove its query and fragment for filesystem resolution;
2. require valid percent escapes and UTF-8 percent-decoding;
3. reject a filesystem-absolute path, `file:` URI, or backslash separator;
4. resolve and normalize `.` and `..` segments relative to the referring
   document;
5. require the result to remain physically within the discovered project root;
6. require a non-link regular target; and
7. reject a target reached through an excluded link or nested-project subtree.

A local target MAY be a Markdown contract document or a non-Markdown supporting
asset. A valid link does not grant its target authority.

## 6. Project descriptor rules

The declaration scanner is the scanner accepted in Standalone Initialization
Contract v0.1 section 7.1. It examines top-level source lines outside fenced
code, indented code, blockquotes, list items, headings, tables, inline-code
examples, quoted strings, and prose that does not satisfy the candidate form.

An accepted declaration matches exactly:

```text
^[ ]{0,3}Documentation contract: llamarc42/v0\.1[ \t]*$
```

For complete structural validation, `project.md` also MUST contain at least one
non-empty level-one ATX heading capable of identifying the project and at least
one non-empty authored body line outside headings, header metadata, code blocks,
and HTML comments capable of carrying its purpose or scope. The exact
initialization placeholders
`# TODO: Project name` and
`TODO: Replace this line with a concise statement of project purpose or scope.`
are objectively incomplete and MUST be replaced before conformance is claimed.

This minimum does not require a particular purpose heading and does not prove
that the chosen name or body text actually states an accurate, concise purpose.
That remains human review.

## 7. Specification lifecycle and authority

This proposal uses the lifecycle hook already present in Documentation Contract
v0.1 for files under `specifications/`.

A specification MAY contain exactly one header metadata field:

```text
Status: Proposed
```

or:

```text
Status: Accepted
```

When absent, the specification is `Current` by structural type for backward
compatibility with Documentation Contract v0.1. When present, `Proposed` is
`NonCurrent` and `Accepted` is `Current`. A duplicate or any other value is an
error and makes authority `Indeterminate`.

This lifecycle applies only to `specifications/**/*.md`. A `Status` field in
another current-guidance document, a historical record, navigation, or
supplemental documentation has no framework authority effect. Everything under
`history/` remains `NonCurrent` regardless of descriptive metadata.

## 8. ADR evaluation rules

### 8.1 Form and identity

A decision-record filename MUST match
`decisions/ADR-NNN-<slug>.md`, where `NNN` is exactly three ASCII digits and the
slug matches `[a-z0-9]+(?:-[a-z0-9]+)*`.

The document MUST contain:

- exactly one level-one heading whose text contains the same `ADR-NNN`
  identifier as a case-sensitive token bounded by the start or end of the
  heading or a character outside `[A-Za-z0-9]`;
- exactly one `## Status` section containing only one of `Proposed`, `Accepted`,
  `Rejected`, `Withdrawn`, or `Superseded` as its non-comment content;
- one non-empty `## Context` section;
- exactly one non-empty `## Decision` or `## Proposed decision` section; and
- one non-empty `## Consequences` section.

The five status spellings are case-sensitive. Filename number, file date,
commit history, issue state, and the presence of a newer ADR never supply a
status.

### 8.2 Relationship extraction

The recognized relationship headings are `## Supersedes`, `## Superseded by`,
`## Amends`, `## Amended by`, and `## Related decisions`. Each may occur at most
once. Every Markdown link within one of those sections is a relationship-target
candidate and therefore must be a local link to a decision record.

A relationship target MUST resolve to a different decision record inside the
same project. A repeated target within one relationship section is invalid.
External, absolute, missing, supplemental, historical, cross-project, and
self-targets are not decision relationships.

### 8.3 Supersession

An authoritative supersession edge is valid only when:

- an `Accepted` or `Superseded` successor links to its immediate predecessor
  under `## Supersedes`;
- that predecessor has status `Superseded` and links back to the immediate
  successor under `## Superseded by`;
- a predecessor has exactly one immediate successor; and
- following immediate successors reaches exactly one `Accepted` terminal ADR
  without a cycle.

A `Superseded` intermediate ADR MAY retain the outgoing `## Supersedes` edge it
established while accepted. Non-accepted ADRs MAY record intended relationship
links for review, but those links have no authority effect. Validation MUST NOT
invent a missing transition or edge.

### 8.4 Amendments

An authoritative amendment edge is valid only when:

- the amending ADR and target both have status `Accepted`;
- the amending ADR links to the target under `## Amends`;
- the target links back under `## Amended by`; and
- the amending ADR has one non-empty `## Amendment scope` section.

The validator confirms only that scope text exists. A human decides whether it
is sufficiently exact. Non-accepted ADRs MAY preserve intended or historical
links, but those links have no amendment effect.

An invalid relationship has no authority effect. In particular, a one-sided
attempted supersession between two `Accepted` ADRs leaves both current and
reports the relationship defect.

## 9. Finite diagnostic catalog

Codes are permanent identifiers for v0.1. Later implementations MAY improve a
message's wording only while preserving the code, severity, required content,
and meaning.

In the examples below, “valid example” means the rule does not emit its code.
“Failure example” means the objectively detectable condition that emits it.

### 9.1 Root and project descriptor

| Code | Severity | Condition and required message content | Valid example | Failure example |
| --- | --- | --- | --- | --- |
| `LC42V001` | Error | Upward discovery reaches the filesystem root without a valid `project.md`; name the supplied start and say a non-link regular `project.md` is required. | Start below a root containing regular `project.md`. | Start in a tree with no `project.md`. |
| `LC42V002` | Error | Upward discovery encounters an entry named `project.md` that is linked, reparse, non-regular, or inaccessible; stop rather than silently selecting an ancestor root, and identify entry kind or access condition. | Regular readable root marker. | `project.md` is a directory or symbolic link. |
| `LC42V003` | Error | No declaration candidate exists; require `Documentation contract: llamarc42/v0.1`. | One exact declaration. | Declaration appears only inside a code fence. |
| `LC42V004` | Error | More than one declaration candidate exists; report candidate lines. | One exact declaration. | Two top-level declaration fields. |
| `LC42V005` | Error | The single candidate is malformed; identify its line and exact required syntax without echoing unrelated content. | Exact case and spacing. | `Documentation Contract: llamarc42/v0.1`. |
| `LC42V006` | Error | The single well-formed field selects a value other than `llamarc42/v0.1`; report the unsupported value and this validator's version. | `llamarc42/v0.1`. | `Documentation contract: llamarc42/v0.2`. |
| `LC42V007` | Error | `project.md` lacks a non-empty level-one ATX heading capable of carrying the project name; state the required minimum. | `# Weather station`. | No H1 or only an empty H1. |
| `LC42V008` | Error | `project.md` has no non-empty authored body line outside headings, header metadata, code blocks, and HTML comments capable of carrying purpose or scope; state that substantive adequacy remains human review. | A prose paragraph or list item describes the project. | File contains only its H1, declaration, and otherwise empty headings. |
| `LC42V009` | Error | An exact initialization placeholder remains; identify the placeholder line and require authored content. | Authored name and purpose. | `# TODO: Project name`. |

### 9.2 Documents, paths, and local references

| Code | Severity | Condition and required message content | Valid example | Failure example |
| --- | --- | --- | --- | --- |
| `LC42V100` | Error | A contract Markdown file is not readable UTF-8 text; identify the path and access or encoding condition. | UTF-8 Markdown, BOM optional. | UTF-16 file or persistent read denial. |
| `LC42V101` | Error | A document identity contains an ASCII control character or one of `<`, `>`, `:`, `"`, backslash, `?`, `*`, or ASCII vertical bar; identify the segment and prohibited character class. | `guides/api.md`. | `guides/a?b.md` on a filesystem that permits it. |
| `LC42V102` | Error | A path segment ends in a space or period; identify the segment. | `quality/risks.md`. | `quality/risks./notes.md`. |
| `LC42V103` | Error | A segment filename stem is a reserved portable name; identify the segment. | `guides/console.md`. | `guides/CON.md`. |
| `LC42V104` | Error | Two or more source paths collide after NFC normalization and default case-folding; report one diagnostic with all colliding paths. | `guides/api.md` only. | `guides/API.md` and `guides/api.md`. |
| `LC42V105` | Error | A local link is malformed, absolute, uses a backslash, escapes the root after relative resolution, or has invalid UTF-8 percent-encoding; identify destination and reason. | Destination `guides/setup.md`, or `../guides/setup.md` from a nested document when it remains inside the root. | Destination `../../outside.md`, `C:\\x.md`, or `file:///x.md`. |
| `LC42V106` | Error | A syntactically valid local link does not resolve to an existing non-link regular file; identify destination and resolved project-relative path. | Link to an existing file or asset. | Link to `missing.md` or a directory. |
| `LC42V107` | Error | A local link resolves only through an excluded link/reparse entry or into a nested-project subtree; identify the excluded boundary. | Link within the same non-link project tree. | Parent project links into a child project's document. |

Directory links and linked files elsewhere are excluded from inventory without a
diagnostic, as required by Documentation Contract v0.1. `LC42V106` or
`LC42V107` applies only when an inventoried document explicitly refers to a
non-regular or excluded target.

### 9.3 Specification lifecycle

| Code | Severity | Condition and required message content | Valid example | Failure example |
| --- | --- | --- | --- | --- |
| `LC42V150` | Error | A specification contains more than one header `Status` field; report candidate lines and make authority indeterminate. | No field or one field. | Both `Status: Proposed` and `Status: Accepted`. |
| `LC42V151` | Error | A single specification status is neither exact `Proposed` nor `Accepted`; report the value and allowed set. | `Status: Accepted`. | `Status: Draft`. |

Absence of a specification status is intentionally valid and means current by
type. No diagnostic inspects lifecycle-looking prose outside header metadata.

### 9.4 ADR form and identity

| Code | Severity | Condition and required message content | Valid example | Failure example |
| --- | --- | --- | --- | --- |
| `LC42V200` | Error | A Markdown file under `decisions/` does not match the ADR filename grammar; identify the required grammar. | `decisions/ADR-007-local-cache.md`. | `decisions/ADR-7-Local_Cache.md`. |
| `LC42V201` | Error | More than one decision path uses the same `ADR-NNN`; emit one diagnostic with all paths. | Unique ADR numbers. | Two `ADR-007-*.md` files. |
| `LC42V202` | Error | The ADR lacks exactly one H1 containing its filename identifier as a case-sensitive token; identify expected identifier. | `# ADR-007: Local cache`. | `# ADR-008: Local cache` in ADR-007. |
| `LC42V203` | Error | The ADR lacks a `## Status` section or that section has no status value; identify the required section. | One non-empty Status section. | No Status heading. |
| `LC42V204` | Error | The ADR has multiple `## Status` sections or multiple non-comment status lines; report their lines. | One section and one value. | Two Status headings or `Accepted` plus `Proposed`. |
| `LC42V205` | Error | The single status value is not an exact allowed status; report value and allowed set. | `Accepted`. | `Draft` or `accepted`. |
| `LC42V206` | Error | `## Context` is missing, duplicated, or empty; identify condition. | One non-empty Context. | Empty Context. |
| `LC42V207` | Error | There is not exactly one non-empty `## Decision` or `## Proposed decision`; identify condition. | One non-empty Decision. | Both headings or neither. |
| `LC42V208` | Error | `## Consequences` is missing, duplicated, or empty; identify condition. | One non-empty Consequences. | Missing Consequences. |

### 9.5 ADR relationships

| Code | Severity | Condition and required message content | Valid example | Failure example |
| --- | --- | --- | --- | --- |
| `LC42V210` | Error | A recognized relationship heading occurs more than once; identify heading and lines. | One `## Supersedes`. | Two `## Supersedes` sections. |
| `LC42V211` | Error | A relationship link is external or resolves to something other than a decision record in this project; identify source, heading, and target. General missing-link failures use `LC42V106` instead. | Local link to `ADR-001-x.md`. | External link or local link to `../architecture/overview.md`. |
| `LC42V212` | Error | An ADR relationship targets itself; identify relationship heading. | ADR-002 targets ADR-001. | ADR-002 Amends ADR-002. |
| `LC42V213` | Error | The same target occurs more than once in one relationship section; identify duplicate target and lines. | One target occurrence. | ADR-001 listed twice under Supersedes. |
| `LC42V214` | Error | A `Superseded` predecessor lacks exactly one `Superseded by` target, or any declared `Superseded by` edge has a non-Superseded source or lacks the reciprocal `Supersedes` link; identify both paths and missing or contradictory element. | ADR-001 Superseded by ADR-002, reciprocal. | ADR-001 is Superseded with no successor link, or is Accepted while declaring `Superseded by`. |
| `LC42V215` | Error | An authoritative `Supersedes` edge has invalid statuses or lacks reciprocal predecessor metadata; identify both paths and required statuses. | Accepted ADR-002 supersedes Superseded ADR-001. | Accepted ADR-002 supersedes Accepted ADR-001. |
| `LC42V216` | Error | A supersession chain cycles, branches at a predecessor, or fails to terminate at exactly one Accepted ADR; report the traversed related paths. | Superseded ADR-001 -> Accepted ADR-002. | ADR-001 and ADR-002 point to each other as successors. |
| `LC42V217` | Error | An authoritative amendment has a non-Accepted endpoint or lacks the reciprocal `Amends`/`Amended by` link; identify both paths and defect. | Two Accepted ADRs with reciprocal links. | Accepted amender targets a Proposed ADR. |
| `LC42V218` | Error | An Accepted amending ADR lacks exactly one non-empty `## Amendment scope`; identify missing, duplicate, or empty structure. | Non-empty scope on accepted amender. | Accepted ADR has `Amends` but no scope. |

Intended relationship links in Proposed, Rejected, or Withdrawn ADRs still
receive path, duplicate, and self-link checks, but do not receive authority-edge
diagnostics merely because they are non-current.

### 9.6 Iteration history and navigation recommendations

These warnings make explicit `SHOULD` conventions inspectable. They do not make
an otherwise conforming project invalid.

| Code | Severity | Condition and required message content | Valid example | Failure example |
| --- | --- | --- | --- | --- |
| `LC42V300` | Warning | A Markdown file anywhere below `history/iterations/` does not match the preferred full placement `history/iterations/YYYY/YYYY-MM-DD.md` or `history/iterations/YYYY/YYYY-MM-DD-<lowercase-kebab-topic>.md`; identify preferred form. | `history/iterations/2026/2026-10-14-review.md`. | `history/iterations/2026-10-14.md` or `history/iterations/2026/2026-10-14_Review.md`. |
| `LC42V301` | Warning | A convention-shaped iteration filename contains an invalid Gregorian calendar date; identify the date. | `2024-02-29.md`. | `2026-02-29.md`. |
| `LC42V302` | Warning | The filename year differs from its immediate `YYYY` directory; identify both years. | `2026/2026-10-14.md`. | `2025/2026-10-14.md`. |
| `LC42V400` | Warning | A present root `AGENTS.md` has local links but its first local Markdown-document link is not `project.md`, or it has no local link to `project.md`; state the navigation recommendation. | First local document link targets `project.md`. | First local document link targets `architecture/overview.md`. |

Missing `AGENTS.md`, missing iteration dates, and absence of the entire
`history/iterations/` tree never produce diagnostics.

### 9.7 Tool-error codes

Tool errors are not conformance diagnostics and always select outcome `Error`.

| Code | Condition | Required message content |
| --- | --- | --- |
| `LC42T001` | The supplied start path does not exist, is not an accessible filesystem entry, or its parent cannot be determined. | Supplied path, observed condition, and retry action. |
| `LC42T002` | An unexpected enumeration or metadata failure prevents a complete root or document inventory. | Safely reportable path, operation phase, and underlying failure category. |
| `LC42T003` | An unexpected read failure occurs after a file was established as an accessible regular file. | Document path, read phase, and retry action without exposing content. |
| `LC42T999` | An internal validator failure prevents a trustworthy result. | Stable code, failed phase, and statement that conformance was not determined. |

Known persistent unreadability or unsupported text encoding is `LC42V100`.
Unexpected I/O after a successful accessibility check is `LC42T003`.

## 10. Deterministic aggregation and ordering

Validation SHOULD continue after a diagnostic when later rules can be evaluated
safely and independently. It MUST suppress derivative diagnostics when a
prerequisite is unavailable:

- `LC42V001` or `LC42V002` ends conformance evaluation because no valid root
  exists;
- `LC42V100` suppresses content, link, lifecycle, and relationship rules for
  that unreadable document;
- `LC42V004` suppresses `LC42V005` and `LC42V006` for the ambiguous declaration;
- `LC42V150` suppresses `LC42V151` for the ambiguous specification status;
- `LC42V204` suppresses `LC42V205` for the ambiguous ADR status;
- `LC42V106` suppresses `LC42V211` for the same missing relationship target;
  and
- an invalid ADR identifier or status suppresses authority-edge evaluation that
  depends on it, while independent form and link rules still run.

One diagnostic is emitted per source occurrence except where a code explicitly
defines one diagnostic for a collision or duplicate set.

Diagnostics MUST be ordered by the following keys in order:

1. phase: root/project, path/identity, document structure, local reference,
   ADR relationship, recommendation;
2. normalized `path` using ordinal Unicode code-point order, with absent path
   first;
3. `line`, with a present line before an absent line for the same path and
   phase;
4. numeric diagnostic code;
5. normalized related path using ordinal Unicode code-point order; and
6. normalized destination text when all preceding keys are equal.

Inventory entries use Documentation Contract v0.1 section 4.3 ordering. Tool
errors preserve whatever complete inventory and diagnostics were established
before failure, but the aggregate outcome remains `Error`.

## 11. Authority derivation

Authority MUST be derived after structural classification and before result
ordering:

| Document condition | Authority state |
| --- | --- |
| Discovered regular `project.md` | `Current`; content diagnostics affect conformance but not its type-assigned authority |
| Current-guidance document outside `specifications/` | `Current` |
| Specification with no lifecycle field | `Current` |
| Specification with valid `Accepted` lifecycle | `Current` |
| Specification with valid `Proposed` lifecycle | `NonCurrent` |
| ADR with status `Accepted` | `Current`, except only relationship effects that are structurally valid apply |
| ADR with status `Proposed`, `Rejected`, `Withdrawn`, or `Superseded` | `NonCurrent` |
| Historical record | `NonCurrent` |
| Navigation or supplemental document | `None` |
| Colliding identity, invalid specification lifecycle, or ADR whose status cannot be determined | `Indeterminate` |

An invalid supersession edge has no authority effect. An accepted predecessor
does not become non-current unless its explicit status is `Superseded`; an
invalid status transition is reported rather than inferred. A historical file
never becomes current because it contains `Status: Accepted`.

Known semantic conflicts between current documents remain human-review issues.
This validator neither discovers them from prose nor selects a winner.

## 12. Verification obligations

An implementation claiming this contract MUST provide fixture evidence that:

- every `LC42V` code has at least one fixture that emits it and one nearby
  valid fixture that does not;
- every `LC42T` code has a controlled failure test where practical;
- repeated validation of each unchanged fixture produces byte-equivalent
  ordered structured results after excluding an explicitly non-normative
  elapsed-time field, if any;
- LF and CRLF versions of the same source produce equivalent results and line
  numbers;
- Windows, macOS, and Linux produce the same codes, normalized paths, authority
  states, and ordering for representable fixture trees;
- directory links, file links, nested projects, reserved names, NFC/case-fold
  collisions, and path-boundary attempts are covered;
- before-and-after filesystem snapshots prove that validation is read-only;
- external URLs are never fetched;
- missing optional ADRs, iteration records, `AGENTS.md`, and optional current
  guidance remain valid; and
- no fixture requires semantic interpretation, model inference, source-control
  state, or network access.

The tables in section 9 are the specification-level valid and failure examples.
CORE-02 owns executable fixtures after this proposal is accepted and promoted.

## 13. Human review decisions

Acceptance requires explicit maintainer decisions on:

1. whether UTF-8 with optional BOM is the sole portable Markdown encoding for
   deterministic v0.1 validation;
2. whether CommonMark 0.31.2 provides the stable source model for headings and
   links without selecting an implementation library;
3. whether a non-empty H1 plus at least one authored body line is the narrowest
   objective structural proxy for the project name and purpose required by
   Documentation Contract v0.1 without mandating an unaccepted heading layout;
4. whether the two accepted initialization placeholders must be explicit
   conformance errors;
5. whether DOC-04 should recognize optional `Proposed` and `Accepted`
   specification lifecycle metadata through the existing lifecycle hook;
6. whether all parsed local links should be checked while fragments and remote
   availability remain outside scope;
7. whether the ADR relationship and suppression rules faithfully encode the
   accepted contract without interpreting prose; and
8. whether warnings for iteration naming and `AGENTS.md` navigation should
   preserve a `Valid` aggregate outcome.

These decisions are part of DOC-04 review. This proposal does not accept them by
its presence or by merge into `history/`.

## 14. Acceptance and promotion path

This historical proposal MUST NOT be treated as the normative validation
contract. If maintainers approve its result model, rule catalog, examples, and
review decisions, a separate reviewed change must promote the accepted text to:

```text
projects/llamarc42/specifications/deterministic-validation-contract-v0.1.md
```

The promoted document must remove historical-proposal metadata and wording that
implies unreviewed acceptance. Moving or copying it into `specifications/` is
the authority-changing action under Documentation Contract v0.1.

DOC-04 remains open until that acceptance step is complete. Acceptance of this
validation contract does not authorize Core or CLI implementation, resolve
DOC-05, alter Documentation Contract v0.1, or introduce v0.2 governance.
