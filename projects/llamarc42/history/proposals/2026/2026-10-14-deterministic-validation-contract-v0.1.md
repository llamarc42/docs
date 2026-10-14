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
| `sourcePath` | Project-relative source locator using `/`, preserving the distinguishable case and Unicode code-point representation exposed by the filesystem. |
| `path` | Stable document identity derived from `sourcePath` using `/` and Unicode NFC. |
| `documentType` | `ProjectDescriptor`, `DecisionRecord`, `CurrentGuidance`, `HistoricalRecord`, `Navigation`, or `Supplemental`. |
| `authorityState` | `Current`, `NonCurrent`, `None`, or `Indeterminate`. |
| `lifecycleStatus` | An explicitly recognized status when one exists; otherwise absent. |
| `decisionId` | `ADR-NNN` for a structurally identifiable ADR; otherwise absent. |

`Indeterminate` means the explicit lifecycle or status evidence required to
derive authority for a specification or ADR cannot be evaluated uniquely. It
MUST NOT be used merely because a document's prose is incomplete or disputed,
or merely because an otherwise classifiable source identity collides. Two
source entries that collide after NFC normalization or case-folding retain
separate inventory entries and separate `sourcePath` values even when their
normalized `path` values are equal. Section 11 defines the complete authority
precedence.

### 4.3 Diagnostic

Every diagnostic MUST contain:

| Field | Requirement |
| --- | --- |
| `code` | One stable code from section 9. |
| `severity` | `Error` or `Warning` as fixed by that code. |
| `message` | Deterministic, non-empty, actionable text satisfying the code's message requirement. |
| `path` | Normalized project-relative path when the condition belongs to one document; otherwise absent. |
| `sourcePath` | Original distinguishable source locator when the condition belongs to one source entry; otherwise absent. |
| `line` | One-based source line when section 9 requires a precise content location; otherwise absent. |
| `relatedSourcePaths` | Zero or more original distinguishable source locators, de-duplicated only by exact code-point equality and ordinally ordered. |

Consumers MUST use `code`, `severity`, and `outcome` for control flow. They MUST
NOT parse `message` to determine program behavior.

A diagnostic for a collision or duplicate set MUST omit singular `sourcePath`
and retain every distinguishable member in `relatedSourcePaths`. Its normalized
`path`, when present, identifies the collided identity rather than replacing
the original locators.

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

Filesystem entry names MUST be obtained by enumeration and retained as
`sourcePath` values without host-native case correction. Framework-fixed paths
and mandatory filename patterns are recognized by ordinal, case-sensitive
matching after Unicode NFC normalization. Case-folding detects portable
identity collisions only; it never recognizes, selects, or renames an entry.
This rule does not impose lowercase styling on arbitrary project-specific
filenames.

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
6. normalize both the resolved destination and each source-entry candidate to
   Unicode NFC while preserving case;
7. select a target only by ordinal, case-sensitive equality of those
   NFC-normalized project-relative paths;
8. require exactly one matching non-link regular target; and
9. reject a target reached through an excluded link or nested-project subtree.

A local target MAY be a Markdown contract document or a non-Markdown supporting
asset. A valid link does not grant its target authority.

Resolution MUST use enumerated source-entry names rather than native
case-insensitive path lookup. Unicode default case-folding is used only to
detect portable identity collisions and to explain a likely case mismatch; it
MUST NOT select or silently correct a reference. If no exact-case NFC match
exists, `LC42V106` is emitted even when a case-folded match exists. If multiple
source entries have the same exact-case NFC path, `LC42V104` reports their
collision and the reference is non-unique under `LC42V106`.

## 6. Project descriptor rules

Root discovery applies Documentation Contract v0.1 sections 3.1 and 3.2 to
each directory from the supplied start toward the filesystem root:

1. An absent exact `project.md` entry causes discovery to continue upward.
2. An entry that can be deterministically classified as linked, reparse, or
   non-regular is not a valid root marker. Validation records `LC42V002` and
   continues upward; it does not treat that directory as a root or as an
   implicit barrier to an accepted ancestor root.
3. If enumeration or metadata access cannot determine whether an exact
   `project.md` entry is absent or is a non-link regular file, discovery stops
   with `LC42T002` and aggregate `Error`. It MUST NOT guess, emit `LC42V002`, or
   select an ancestor through the uninspected condition.
4. The first exact `project.md` entry deterministically established as a
   non-link regular file selects the nearest valid root. Content readability is
   evaluated after selection and does not cause discovery to skip that root.

Thus an invalid nearer entry type can coexist with a selected valid ancestor
and makes that validation result `Invalid`. An environmental inspection failure
instead prevents complete discovery and makes the result `Error`. If discovery
reaches the filesystem root after one or more `LC42V002` observations without
selecting a valid root, it also emits `LC42V001`; no project inventory or
content validation follows.

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
established while accepted. Authority-edge status and reciprocity checks apply
only when the source ADR is `Accepted` or `Superseded`. Proposed, Rejected, and
Withdrawn ADRs MAY record intended or historical relationship links for review;
those links still receive independent path, duplicate, and self-link checks but
have no authority effect. Validation MUST NOT invent a missing transition,
target path, or edge.

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
| `LC42V002` | Error | Upward discovery deterministically classifies an exact `project.md` entry as linked, reparse, or non-regular; identify its source location and observed entry kind, then continue searching for the nearest valid ancestor root. Inability to inspect entry metadata is `LC42T002`, not this code. | The nearest exact `project.md` is a non-link regular file, with no invalid marker type encountered below it. | A nearer `project.md` is a directory or symbolic link; report it and select a valid regular ancestor marker when one exists. |
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
| `LC42V100` | Error | Completely read bytes of a contract Markdown file do not decode as UTF-8 text; identify the path and encoding condition. An undecodable specification has `Indeterminate` authority because its optional lifecycle cannot be evaluated. An operating-system read or access failure is `LC42T003`, not this code. | UTF-8 Markdown, BOM optional. | A completely read UTF-16 specification. |
| `LC42V101` | Error | A document identity contains an ASCII control character or one of `<`, `>`, `:`, `"`, backslash, `?`, `*`, or ASCII vertical bar; identify the segment and prohibited character class. | `guides/api.md`. | `guides/a?b.md` on a filesystem that permits it. |
| `LC42V102` | Error | A path segment ends in a space or period; identify the segment. | `quality/risks.md`. | `quality/risks./notes.md`. |
| `LC42V103` | Error | A segment filename stem is a reserved portable name; identify the segment. | `guides/console.md`. | `guides/CON.md`. |
| `LC42V104` | Error | Two or more source paths collide after NFC normalization and default case-folding; retain every distinct original locator in `relatedSourcePaths` and report the normalized collision identity. | `guides/api.md` only. | `guides/API.md` and `guides/api.md`, or `café.md` using U+00E9 and `café.md` using U+0065 U+0301. |
| `LC42V105` | Error | A local link is malformed, absolute, uses a backslash, escapes the root after relative resolution, or has invalid UTF-8 percent-encoding; identify destination and reason. | Destination `guides/setup.md`, or `../guides/setup.md` from a nested document when it remains inside the root. | Destination `../../outside.md`, `C:\\x.md`, or `file:///x.md`. |
| `LC42V106` | Error | A syntactically valid local link has no unique ordinal case-sensitive match after NFC normalization, or its match is not an existing non-link regular file. Identify the original destination, normalized candidate, and any case-fold-only or NFC-colliding source locators without selecting them. | Destination `guides/setup.md` exactly matches that source path; composed and decomposed spellings match one unique NFC identity. | Destination `guides/Setup.md` when only `guides/setup.md` exists, a missing file, a directory, or two NFC-equivalent source entries. |
| `LC42V107` | Error | A local link resolves only through an excluded link/reparse entry or into a nested-project subtree; identify the excluded boundary. | Link within the same non-link project tree. | Parent project links into a child project's document. |
| `LC42V109` | Error | An objectively identifiable framework-controlled daily-iteration name violates its mandatory component grammar: a numeric year-directory and date digits must be ASCII, a present topic suffix must be lowercase ASCII kebab-case, and its Markdown extension must be exact lowercase `.md`. Identify the original component and required grammar. | `2026/2026-10-14-review.md`; arbitrary `guides/API.md` is outside this framework-controlled grammar and is not an error. | `2026-10-14-Review.md`, `2026-10-14_review.md`, a numeric year or date using non-ASCII digits, or `2026-10-14.MD`. |

Directory links and linked files elsewhere are excluded from inventory without a
diagnostic, as required by Documentation Contract v0.1. `LC42V106` or
`LC42V107` applies only when an inventoried document explicitly refers to a
non-regular or excluded target.

A daily-iteration filename candidate is a regular file below
`history/iterations/` whose basename begins with four Unicode decimal digits,
`-`, two Unicode decimal digits, `-`, and two Unicode decimal digits, with a
case-insensitive Markdown-like extension. A four-Unicode-decimal-digit parent
segment directly below `history/iterations/` is also a numeric year-directory
candidate. Candidate detection exists only to validate the accepted
framework-controlled year, date, topic, and extension components; it does not
make other historical filenames invalid or add a wrong-extension candidate to
the contract-document inventory.

ADR filenames remain governed by `LC42V200`. Framework-fixed names are
recognized only by their exact accepted spelling. Arbitrary project-specific
document names MAY use mixed case and receive no error merely for doing so;
their lowercase ASCII kebab-case style remains a recommendation rather than a
conformance requirement.

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
| `LC42V200` | Error | A regular file under `decisions/` with an `ADR-` basename or a case-insensitive Markdown-like extension does not match the exact ADR filename grammar, including lowercase `.md`; identify the required grammar. Wrong-extension candidates do not enter the document inventory. | `decisions/ADR-007-local-cache.md`. | `decisions/ADR-7-Local_Cache.md` or `decisions/ADR-007-local-cache.MD`. |
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
| `LC42V214` | Error | For an `Accepted` or `Superseded` source, a declared `Superseded by` edge has an invalid source status or lacks the reciprocal `Supersedes` link; a `Superseded` source also fails when it lacks exactly one successor target. Identify the source and only target paths that are explicitly declared; never invent a path for a missing successor. | ADR-001 is Superseded by ADR-002 with a reciprocal link; a Proposed ADR may record an intended successor without authority-edge evaluation. | ADR-001 is Superseded with no successor link, or is Accepted while declaring `Superseded by`. |
| `LC42V215` | Error | An authoritative `Supersedes` edge has invalid statuses or lacks reciprocal predecessor metadata; identify both paths and required statuses. | Accepted ADR-002 supersedes Superseded ADR-001. | Accepted ADR-002 supersedes Accepted ADR-001. |
| `LC42V216` | Error | A supersession chain cycles, branches at a predecessor, or fails to terminate at exactly one Accepted ADR; report the traversed related paths. | Superseded ADR-001 -> Accepted ADR-002. | ADR-001 and ADR-002 point to each other as successors. |
| `LC42V217` | Error | An authoritative amendment has a non-Accepted endpoint or lacks the reciprocal `Amends`/`Amended by` link; identify both paths and defect. | Two Accepted ADRs with reciprocal links. | Accepted amender targets a Proposed ADR. |
| `LC42V218` | Error | An Accepted amending ADR lacks exactly one non-empty `## Amendment scope`; identify missing, duplicate, or empty structure. | Non-empty scope on accepted amender. | Accepted ADR has `Amends` but no scope. |

Intended relationship links in Proposed, Rejected, or Withdrawn ADRs still
receive path, duplicate, and self-link checks, but do not receive authority-edge
diagnostics merely because they are non-current.

### 9.6 Iteration history and navigation recommendations

Mandatory framework-controlled filename components are errors under
`LC42V109` and `LC42V301`. The warning rules in this section make `SHOULD`
placement and navigation conventions inspectable without making an otherwise
conforming project invalid. They do not impose a universal lowercase style on
arbitrary project documents.

| Code | Severity | Condition and required message content | Valid example | Failure example |
| --- | --- | --- | --- | --- |
| `LC42V300` | Warning | An iteration record does not use the preferred full placement `history/iterations/YYYY/YYYY-MM-DD.md` or `history/iterations/YYYY/YYYY-MM-DD-<topic>.md`; identify the preferred placement without converting an otherwise arbitrary historical filename into a mandatory lowercase name. | `history/iterations/2026/2026-10-14-review.md`. | `history/iterations/2026-10-14.md` or `history/iterations/by-topic/design-notes.md`. |
| `LC42V301` | Error | A daily-iteration filename with otherwise valid mandatory component grammar contains an invalid Gregorian calendar date; identify the date. | `2024-02-29.md`. | `2026-02-29.md`. |
| `LC42V302` | Warning | The filename year differs from its immediate `YYYY` directory; identify both years. | `2026/2026-10-14.md`. | `2025/2026-10-14.md`. |
| `LC42V400` | Warning | A present root `AGENTS.md` has local links but its first local Markdown-document link is not `project.md`, or it has no local link to `project.md`; state the navigation recommendation. | First local document link targets `project.md`. | First local document link targets `architecture/overview.md`. |

Missing `AGENTS.md`, missing iteration dates, and absence of the entire
`history/iterations/` tree never produce diagnostics.

### 9.7 Tool-error codes

Tool errors are not conformance diagnostics and always select outcome `Error`.

| Code | Condition | Required message content |
| --- | --- | --- |
| `LC42T001` | The supplied start path does not exist, is not an accessible filesystem entry, or its parent cannot be determined. | Supplied path, observed condition, and retry action. |
| `LC42T002` | Enumeration or entry-metadata inspection fails before a complete root or document inventory can be established, including inability to classify a candidate root marker. | Safely reportable path, operation phase, and underlying failure category; during root discovery, also state that no ancestor was selected through the uninspected condition. |
| `LC42T003` | Opening or completely reading a file fails after metadata established it as a non-link regular file. | Document path, read phase, underlying failure category, and retry action without exposing content. |
| `LC42T999` | An internal validator failure prevents a trustworthy result. | Stable code, failed phase, and statement that conformance was not determined. |

Unsupported UTF-8 encoding after a complete byte read is `LC42V100`. Failure to
obtain the complete bytes is `LC42T003`, regardless of whether the operating
system reports access denial, sharing conflict, device error, or another I/O
condition. This boundary depends on the completed operation, not on retry count
or an implementation's judgment about whether a failure is expected.

## 10. Deterministic aggregation and ordering

Validation SHOULD continue after a diagnostic when later rules can be evaluated
safely and independently. It MUST suppress derivative diagnostics when a
prerequisite is unavailable:

- `LC42T002` during root discovery ends evaluation with `Error` because the
  nearest valid root cannot be determined safely;
- `LC42T003` ends content-dependent evaluation for the affected document and
  selects aggregate `Error`; any already established inventory entry remains in
  the partial result;
- `LC42V002` does not stop upward discovery; each deterministically invalid
  marker type is retained, and `LC42V001` ends conformance evaluation only when
  the filesystem root is reached without selecting a valid root;
- `LC42V100` suppresses content, link, lifecycle, and relationship rules for
  that unreadable document;
- `LC42V004` suppresses `LC42V005` and `LC42V006` for the ambiguous declaration;
- `LC42V150` suppresses `LC42V151` for the ambiguous specification status;
- `LC42V204` suppresses `LC42V205` for the ambiguous ADR status;
- `LC42V106` suppresses `LC42V211` for the same missing relationship target;
- `LC42V109` suppresses `LC42V301` when the same filename lacks parseable exact
  ASCII date components and suppresses `LC42V302` when either year cannot be
  determined; and
- an invalid ADR identifier or status suppresses authority-edge evaluation that
  depends on it, while independent form and link rules still run.

One diagnostic is emitted per source occurrence except where a code explicitly
defines one diagnostic for a collision or duplicate set.

Every conformance code has exactly one ordering phase:

| Phase rank and name | Codes |
| --- | --- |
| 1 — Root/project | `LC42V001`–`LC42V009` |
| 2 — Path/identity | `LC42V100`, `LC42V101`, `LC42V102`, `LC42V103`, `LC42V104`, `LC42V109`, `LC42V200`, `LC42V201`, `LC42V301` |
| 3 — Document structure | `LC42V150`, `LC42V151`, `LC42V202`, `LC42V203`, `LC42V204`, `LC42V205`, `LC42V206`, `LC42V207`, `LC42V208` |
| 4 — Local reference | `LC42V105`, `LC42V106`, `LC42V107` |
| 5 — ADR relationship | `LC42V210`, `LC42V211`, `LC42V212`, `LC42V213`, `LC42V214`, `LC42V215`, `LC42V216`, `LC42V217`, `LC42V218` |
| 6 — Recommendation | `LC42V300`, `LC42V302`, `LC42V400` |

Diagnostics MUST be ordered by the following keys in order:

1. phase rank from the table above;
2. normalized `path` using ordinal Unicode code-point order, with absent path
   first;
3. original `sourcePath` using ordinal Unicode code-point order, with absent
   source path first;
4. `line`, with a present line before an absent line for the same path, source,
   and phase;
5. numeric diagnostic code;
6. the ordinally ordered sequence of original `relatedSourcePaths`; and
7. NFC-normalized destination text followed by original destination text when
   all preceding keys are equal.

Inventory entries use Documentation Contract v0.1 section 4.3 grouping. ADRs
first use numeric decision identifier and then normalized `path`; other entries
use normalized `path`. Original `sourcePath` is the final ordinal tie-breaker
for every group, including invalid inventories whose NFC-normalized identities
are equal. Tool errors preserve whatever complete inventory and diagnostics
were established before failure, but the aggregate outcome remains `Error`.

## 11. Authority derivation

Authority MUST be derived after structural classification and before result
ordering. The following precedence is exhaustive and applies independently to
each retained `sourcePath`:

1. Determine structural document type from the exact source path under
   Documentation Contract v0.1. NFC and case-fold collision detection does not
   merge source entries or change their structural type.
2. Assign type-derived authority to types that do not depend on document
   lifecycle content: the project descriptor and current-guidance documents
   outside `specifications/` are `Current`; historical records are
   `NonCurrent`; navigation and supplemental documents are `None`.
3. For a specification, no lifecycle field means `Current`, exact `Accepted`
   means `Current`, and exact `Proposed` means `NonCurrent`. The specification
   is `Indeterminate` only when its lifecycle cannot be evaluated because its
   complete bytes are unavailable or not valid UTF-8, or because `LC42V150` or
   `LC42V151` establishes duplicate or invalid lifecycle metadata.
4. For an ADR, exact `Accepted` means `Current`; exact `Proposed`, `Rejected`,
   `Withdrawn`, or `Superseded` means `NonCurrent`. The ADR is `Indeterminate`
   only when its status cannot be evaluated because its complete bytes are
   unavailable or not valid UTF-8, or because `LC42V203`, `LC42V204`, or
   `LC42V205` establishes missing, ambiguous, or invalid status content.
5. Identity, portable-path, form, and relationship diagnostics do not otherwise
   replace an authority state already determined above. In particular,
   `LC42V104` and `LC42V201` make the project invalid but do not by themselves
   make a retained source entry's authority `Indeterminate`.

When a tool error prevents a complete read, an already inventoried
specification or ADR uses `Indeterminate` in the partial result and the aggregate
outcome remains `Error`. Other already classified document types retain their
type-derived state in that partial result.

Collision examples make the precedence explicit:

| Colliding sources | Diagnostics | Authority states |
| --- | --- | --- |
| `history/API.md` and `history/api.md` | `LC42V104` | Both remain `NonCurrent`; a collision cannot make historical evidence current or indeterminate. |
| `README.md` and `readme.md` | `LC42V104` | Exact `README.md` is navigation and the other source is supplemental; both are `None`. |
| `guides/API.md` and `guides/api.md` | `LC42V104` | Both remain `Current` by current-guidance type, while the project is `Invalid`. |
| NFC-equivalent specification source paths with exact `Accepted` and `Proposed` lifecycle values | `LC42V104` | The retained sources are respectively `Current` and `NonCurrent`; either source becomes `Indeterminate` only for its own unreadable, duplicate, or invalid lifecycle evidence. |
| `ADR-007-alpha.md` and `ADR-007-beta.md` with exact `Accepted` and `Proposed` statuses | `LC42V201` | The retained ADR sources are respectively `Current` and `NonCurrent`; duplicate decision identity is still an error, and an ADR becomes `Indeterminate` only when its own status cannot be evaluated. |

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
  states, and ordering for representable fixture trees, including a link whose
  case differs from its source target;
- composed and decomposed Unicode reference spellings resolve to one unique NFC
  identity, while representable NFC/case-fold collision fixtures retain every
  distinct original `sourcePath` and produce a deterministic tied order;
- collision fixtures for historical, navigation, current-guidance,
  specification, and ADR sources produce the authority states in section 11;
  collision diagnostics alone never select `Indeterminate`;
- directory links, file links, nested projects, reserved names, exact-case
  reference mismatches, NFC/case-fold collisions, and path-boundary attempts
  are covered;
- every conformance code occurs exactly once in the phase mapping and phase
  ordering is verified with multiple diagnostics on the same source;
- daily iteration fixtures distinguish mandatory ASCII/lowercase filename
  errors from preferred-placement warnings, and arbitrary mixed-case project
  filenames do not become errors;
- Proposed, Rejected, and Withdrawn ADRs with intended supersession links avoid
  authority-edge diagnostics while Accepted and Superseded sources receive the
  required status and reciprocity checks;
- an undecodable specification receives `LC42V100` and `Indeterminate`
  authority;
- a nearer linked, reparse, or non-regular `project.md` emits `LC42V002` and
  discovery continues to a valid ancestor; when no valid ancestor exists,
  `LC42V001` is also emitted;
- inability to enumerate an ancestor or inspect candidate-marker metadata emits
  `LC42T002`, stops discovery without selecting through the unknown condition,
  and produces `Error` rather than `Invalid`;
- a selected non-link regular marker with completely readable non-UTF-8 bytes
  emits `LC42V100`, while failure to obtain its complete bytes emits
  `LC42T003`; fixtures cover access denial, interrupted reads, and invalid
  encoding without allowing the same observation to select both outcomes;
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
   deterministic v0.1 validation, with invalid encoding treated as conformance
   failure only after a complete byte read and I/O failure treated as a tool
   error;
2. whether CommonMark 0.31.2 provides the stable source model for headings and
   links without selecting an implementation library;
3. whether a non-empty H1 plus at least one authored body line is the narrowest
   objective structural proxy for the project name and purpose required by
   Documentation Contract v0.1 without mandating an unaccepted heading layout;
4. whether the two accepted initialization placeholders must be explicit
   conformance errors;
5. whether DOC-04 should recognize optional `Proposed` and `Accepted`
   specification lifecycle metadata through the existing lifecycle hook, with
   unreadable specifications assigned `Indeterminate` authority;
6. whether all parsed local links should be checked while fragments and remote
   availability remain outside scope, using exact-case matching after Unicode
   NFC normalization rather than host-native case behavior;
7. whether original `sourcePath` and `relatedSourcePaths` values plus the final
   source-path tie-breaker preserve distinguishable invalid collision evidence,
   and whether deriving authority independently per retained source correctly
   avoids blanket `Indeterminate` classification for identity collisions;
8. whether mandatory framework-controlled ADR and iteration filename rules
   should be errors while arbitrary-document lowercase style and preferred
   iteration placement remain non-invalidating recommendations;
9. whether ADR authority-edge checks should apply only to `Accepted` and
   `Superseded` sources while other statuses receive independent structural
   link checks;
10. whether the explicit one-phase-per-code mapping and ordering keys are
    sufficient for cross-implementation determinism;
11. whether warnings for preferred iteration placement and `AGENTS.md`
    navigation should preserve a `Valid` aggregate outcome; and
12. whether nearest-root discovery should continue past deterministically
    linked, reparse, or non-regular marker entries while treating inability to
    inspect a candidate marker as `Error` without selecting an ancestor through
    that unknown condition.

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
