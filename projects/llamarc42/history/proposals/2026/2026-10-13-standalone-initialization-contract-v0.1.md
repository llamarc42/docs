# Standalone Initialization Contract v0.1

Version: 0.1

Status: Proposed

Document type: Historical proposal

Authority: None. Its `history/` path classifies it as a historical record under
Documentation Contract v0.1. `Status: Proposed` is descriptive metadata only.

Tracks: [DOC-03](https://github.com/llamarc42/docs/issues/9)

## 1. Purpose and scope

This proposal defines review requirements for safely initializing a standalone
llamarc42 documentation project. If accepted and promoted to current guidance,
it will govern initialization planning, approved template meaning, filesystem
application, and ownership boundaries shared by Core and CLI implementations.

This proposal covers only standalone initialization equivalent to:

```text
llamarc42 init [--directory <path>]
```

CLI syntax, output envelopes, and exit codes remain owned by
[CLI-01](https://github.com/llamarc42/cli/issues/20). Validation rules and
diagnostic codes remain owned by
[DOC-04](https://github.com/llamarc42/docs/issues/10). Reconciliation of the
accepted architecture remains owned by
[DOC-05](https://github.com/llamarc42/docs/issues/11).

This proposal does not define multi-root or multi-child initialization,
repository membership, migration, implementation libraries, package formats,
or runtime code.

The requirement terms **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and
**MAY** describe the behavior proposed for later acceptance. They have no
normative effect while this file remains historical.

## 2. Governing inputs and conflicts

The accepted inputs are:

- [Documentation Contract v0.1](../../../specifications/documentation-contract-v0.1.md),
  which defines the project root, minimum valid project, portable paths,
  artifact classes, authority, and generic `AGENTS.md` role; and
- [ADR-005](../../../decisions/ADR-005-agent-agnostic-documentation-framework-boundary.md),
  which requires a documentation-first, local-first, agent-agnostic framework
  that remains usable without a model, agent, network, or hosted service.

Accepted ADR-001 through ADR-004 and older current guidance still conflict with
ADR-005. DOC-05 owns their disposition. This proposal does not resolve those
conflicts. Core implementation remains blocked on the applicable DOC-05
outcome, including the ADR-002 disposition recorded by CORE-01.

The following historical records informed review but grant no authority:

- [Governance Improvement Program](2026-10-13-governance-improvement-program-v0.1.md);
  and
- [BAT Spec Kit experiment](../../experiments/2026/2026-10-13-bat-spec-kit.md).

The historical material supports explicit preflight, honest evidence claims,
and visible stop conditions. It does not add multi-repository requirements to
DOC-03.

## 3. Initialization boundary

Initialization is a deterministic, non-interactive filesystem operation. It
has two separable stages:

1. **Plan**: inspect only the selected target and classify every approved
   artifact without changing the filesystem.
2. **Apply**: re-preflight the plan and create only artifacts still classified
   for creation.

Planning MUST NOT mutate the filesystem. Application MUST use the same approved
template-set identity and documentation-contract version recorded by the plan.

Initialization success means the approved operation completed as planned. It
does not mean that project-authored prose is complete, that DOC-04 validation
has passed, or that the resulting documentation is architecturally correct.

Initialization MUST be:

- offline and independent of source control;
- deterministic for the same target state and template set;
- non-interactive;
- confined to the selected target;
- non-destructive; and
- honest about partial results after unexpected failures.

## 4. Target selection and path safety

### 4.1 Selecting the target

When no explicit directory is supplied, the target is the process current
working directory. When an explicit directory is supplied, a relative path is
resolved against that current working directory; an absolute path identifies
the target directly.

After lexical normalization, the selected target MUST be represented by one
absolute path for the duration of the operation. User-facing reporting SHOULD
also retain the supplied path when it differs from the normalized path.

The target MUST already exist, be accessible, and be a directory. A nonexistent
target is invalid. Initialization MUST NOT create the target directory because
doing so would require mutation in its parent and make the requested boundary
ambiguous. The user must create the directory explicitly and retry.

Implicit and explicit selection MUST use the same normalization, ancestry
inspection, and classification rules. When they produce the same normalized
absolute component chain, they MUST produce equivalent plans and results.
Different lexical paths that reach the same physical directory through a link
or reparse-point alias are intentionally not equivalent inputs: the aliased
path is invalid under section 4.3. Initialization does not resolve aliases or
provide a generalized physical-filesystem identity service.

### 4.2 Root and containment rules

Every planned artifact path MUST:

- be a portable framework-defined path allowed by Documentation Contract v0.1;
- normalize beneath the selected target;
- contain no `.` or `..` segment after normalization; and
- be rejected if containment cannot be established before writing.

Initialization MUST NOT recursively scan siblings, parents, or arbitrary
descendants. It MAY inspect only the selected target, the two approved artifact
paths, and ancestor metadata needed to reject links and detect an existing
enclosing project. Ancestor inspection is read-only and MUST NOT enumerate or
read unrelated ancestor contents.

If the target has no `project.md` but root discovery finds a project in an
ancestor directory, initialization MUST report an invalid target rather than
silently create a nested project. Documentation Contract v0.1 permits explicit
nested projects, but this command has no explicit nested-project authorization.

### 4.3 Target ancestry, links, and special entries

Before project discovery or artifact inspection, initialization MUST inspect
the selected absolute path component by component, beginning with its
filesystem root and ending with the target. Each component entry MUST be
examined with no-follow metadata operations. On Windows, a symbolic link,
junction, mount-point reparse point, or any other reparse-point component is
invalid. On other platforms, a symbolic link or equivalent link-like component
is invalid.

If a component is linked, reparse, unavailable, or cannot be inspected without
following a link, the target is invalid and both approved artifacts are
`Invalid`. The diagnostic MUST identify the normalized selected target, the
first offending component that can be reported safely, and the detected entry
type or access failure. It MUST instruct the user to select a direct,
inspectable, non-link path and create a new plan.

This rule applies identically whether the selected path came from the process
current directory or an explicit argument. It is an initialization safety rule;
it does not change Documentation Contract v0.1 inventory behavior after a valid
root has been selected.

An approved artifact path is invalid when an existing entry at that path is:

- a symbolic link or file reparse point;
- a directory;
- a device, socket, pipe, or other special filesystem entry; or
- anything other than a non-link regular file.

Initialization MUST NOT follow or resolve a link to decide that a target,
ancestor, or artifact is safe or compatible. It MUST NOT use canonicalization
through links, recursively traverse ancestors, read link targets, or perform
any write, create, delete, or probe outside the selected target.

## 5. Approved standalone template set

### 5.1 Identity and version

The proposed template-set identity is:

```text
llamarc42/standalone-init/v0.1
```

The template set targets:

```text
Documentation contract: llamarc42/v0.1
```

Template-set version and documentation-contract version are distinct. The
template-set version identifies the exact approved assets and initialization
semantics. The contract version identifies the project-documentation rules.
A template revision MUST NOT silently change the selected contract version.

The template identity and contract version belong to packaged Core assets and
initialization results. Initialization MUST NOT create a second registry or a
template-metadata file inside the target merely to persist them.

### 5.2 Approved artifacts

The standalone template set contains exactly two root-level Markdown files:

| Ordered path | Contract role | Initialization reason |
| --- | --- | --- |
| `project.md` | Required project descriptor and root marker | Establish the minimum project entry point and contract declaration. |
| `AGENTS.md` | Optional navigation | Provide the generic, agent-agnostic entry point explicitly required by DOC-03. |

No directory is created eagerly. Optional contract directories are created only
when a separately approved artifact requires them. This template set requires
none.

Initialization MUST NOT create:

- `context.md`, `constraints.md`, architecture, domain, guide, specification,
  or quality content whose project meaning has not been authored;
- an ADR or `decisions/` placeholder;
- an iteration record, daily log, or `history/` placeholder;
- a README, configuration file, source-control file, registry, session file, or
  implementation artifact; or
- any empty directory added only to make the project appear complete.

### 5.3 Proposed `project.md` template meaning

Documentation Contract v0.1 requires a real project name and concise purpose.
The authorized CLI shape has no wizard and no project-name or purpose input.
Initialization therefore MUST NOT infer those facts from a directory name or
invent them.

The proposed template is an explicit authoring scaffold with this meaning:

```markdown
# TODO: Project name

Documentation contract: llamarc42/v0.1

## Purpose

TODO: Replace this line with a concise statement of project purpose or scope.

## Documentation

Add links to current documents needed to understand this project.
```

The exact accepted template bytes will be packaged by Core after this contract
is accepted. The two `TODO` markers MUST remain visibly incomplete; they MUST
NOT be presented as project facts.

Creating this scaffold does not make the project conforming. Initialization
output MUST tell the user to replace the placeholders before claiming
Documentation Contract v0.1 conformance. Whether this placeholder strategy is
accepted is an explicit human review decision in section 12.

### 5.4 Proposed `AGENTS.md` template meaning

The generic entry point MUST:

- point first to `project.md`;
- describe `project.md` and its links as the starting point for current project
  knowledge;
- identify `history/`, when present, as non-authoritative evidence;
- contain navigation and concise usage guidance only; and
- avoid vendor, model, protocol, hosted-service, or agent-workflow requirements.

It MUST NOT duplicate project architecture, constraints, decisions, or other
normative content. It MUST NOT control discovery, validity, authority, or
meaning.

## 6. Planning and artifact classifications

The plan MUST list `project.md` before `AGENTS.md` and assign exactly one of the
following anticipated classifications to each artifact. The completed result
MUST use the same four classifications to record the observed outcome:

### Created

In a plan, `Created` means the artifact is absent and is scheduled to be written
from the approved template. In the completed result, it means that planned
write succeeded. The phase of the report distinguishes an anticipated creation
from an observed creation; no separate `create` classification is introduced.

### Existing

`Existing` means a non-link regular file already occupies the approved path and
initialization will preserve it byte-for-byte.

For `project.md`, an existing file is initialization-compatible only when it
contains exactly one declaration selecting `llamarc42/v0.1` under the narrow
recognition rule in section 7.1. Initialization does not judge whether its name,
purpose, links, or prose are substantively complete; that belongs to validation
and human review.

For `AGENTS.md`, any non-link regular file is initialization-compatible and is
preserved. `Existing` is not a claim that the file conforms to every navigation
recommendation.

### Skipped

`Skipped` means initialization deliberately performed no write for an approved
artifact and reports why. Required uses include:

- an otherwise creatable artifact blocked because another artifact made the
  complete plan invalid; and
- an absent artifact whose recorded plan was refused after re-preflight found
  stale target state or a template- or contract-identity mismatch; and
- a not-yet-attempted artifact after an unexpected application failure.

`Skipped` MUST NOT conceal a conflict or be used as an overwrite substitute.

### Invalid

`Invalid` means the artifact cannot safely reach or retain an approved regular
file state. It applies both to objectively detectable preflight conditions and
to an artifact whose attempted creation fails unexpectedly. Preflight examples
include:

- missing, inaccessible, non-directory, linked, or reparse-point target;
- target nested beneath an already discovered project without explicit nested
  project authorization;
- path containment failure;
- special or linked entry at an approved artifact path;
- existing `project.md` with no contract declaration, multiple declarations,
  or a declaration selecting another contract version; and
- a known inability to create a required missing artifact.

An application-time `Invalid` result MUST identify the failed artifact, phase,
and underlying error. The aggregate `Error` outcome distinguishes that case
from a preflight-invalid plan.

These are initialization conditions, not the complete DOC-04 validation rule
catalog.

### 6.1 Aggregate outcomes

The operation has one aggregate outcome:

| Outcome | Meaning |
| --- | --- |
| Applied | Preflight was valid and every planned creation completed. At least one artifact is `Created`. |
| No change | Preflight was valid, every artifact is `Existing`, and no write occurred. |
| Invalid | Initial preflight found an invalid condition, or re-preflight found that the recorded plan was stale or inapplicable; no planned write occurred. A plan-level mismatch can produce this aggregate even when current artifact classifications are only `Existing` and `Skipped`. |
| Error | Preflight succeeded, but an unexpected failure interrupted application. Completed and unattempted artifacts are reported honestly. |

`Applied` and `No change` are successful initialization outcomes. They do not
assert full documentation-contract validation. CLI exit behavior and serialized
output remain deferred to CLI-01.

## 7. Preflight and compatibility rules

Before any planned write, initialization MUST inspect both approved paths and
produce the complete ordered plan.

The plan MUST retain enough comparison evidence to detect material change at
re-preflight: the normalized target, the link/reparse/access classification of
its component chain, the template identity, the contract version, and each
artifact's presence, entry kind, classification, and bytes when it is an
existing regular file. Core owns the internal comparison representation; the
contract does not require exposing file contents or selecting a particular hash
algorithm. Timestamp-only or other metadata-only changes are not material
unless they change access, entry type, link status, or safe application.

If any artifact or target condition is `Invalid`:

- the aggregate outcome is `Invalid`;
- no planned directory or file write may begin;
- every conflict must be reported with its target-relative path and actionable
  correction; and
- otherwise creatable artifacts must be reported as `Skipped` because the plan
  was blocked.

Preflight MUST NOT use a write-and-delete probe to test writability. A known
permission or access denial is invalid when it can be established without
mutation. Because filesystem state can change after preflight, writability
checks cannot guarantee application success.

Existing compatible files MUST be preserved byte-for-byte even when they differ
from the current template. Template version changes do not authorize rewrite,
normalization, migration, or added metadata.

### 7.1 Initialization-compatible contract declaration

Documentation Contract v0.1 defines the declaration text exactly as:

```text
Documentation contract: llamarc42/v0.1
```

DOC-03 reuses that syntax and defines only the narrow recognition needed to
decide whether an existing `project.md` can be preserved during initialization.
It does not introduce another declaration form.

Initialization examines the decoded Markdown source as logical lines, treating
LF and CRLF line endings equivalently. A file that cannot be read as Markdown
text is `Invalid`. On each top-level source line outside the excluded Markdown
contexts below, initialization removes zero through three leading ASCII spaces,
finds the first colon, removes ASCII spaces and tabs immediately before that
colon, and compares the preceding field name to `Documentation contract` using
ordinal case-insensitive comparison. A match is one candidate declaration.
Lines without that field-name match are not candidates.

An accepted candidate MUST match this grammar:

```text
^[ ]{0,3}Documentation contract: llamarc42/v0\.1[ \t]*$
```

The grammar permits zero through three leading ASCII spaces and trailing ASCII
spaces or tabs. It permits no leading tab and no other internal whitespace.
The field name and value are case-sensitive, including the lowercase
`contract` and `llamarc42/v0.1` value.

The following source does not contribute candidates:

- lines inside fenced code blocks. An opener has zero through three leading
  spaces followed by at least three matching backticks or tildes. A closer has
  zero through three leading spaces, at least the opening run length of the
  same character, and only trailing ASCII whitespace. An unclosed fence remains
  open through end of file;
- indented-code lines beginning with a tab or at least four spaces;
- blockquote lines whose first non-space character after zero through three
  spaces is `>`; and
- list items, headings, table cells, inline-code examples, quoted strings, or
  prose whose source line does not satisfy the candidate form.

Recognition produces exactly one of these compatibility results:

- exactly one candidate that matches the accepted grammar is compatible;
- no candidate is `Invalid` as a missing declaration, even if ignored examples
  contain the literal text;
- more than one candidate is `Invalid` as duplicate or ambiguous declarations;
  and
- exactly one candidate that does not match the accepted grammar is `Invalid`
  as malformed or selecting an unsupported contract value.

For one nonmatching candidate, `unsupported value` applies only when the
case-sensitive field name and required internal spacing are exactly
`Documentation contract: ` but the value, after permitted trailing whitespace
is removed, is not `llamarc42/v0.1`. Every other single nonmatching candidate is
`malformed`. When there are multiple candidates, duplicate or ambiguous takes
precedence over their individual defects.

The diagnostic MUST distinguish missing, duplicate or ambiguous, malformed,
and unsupported-value cases. This check does not validate the project name,
purpose, links, other Markdown structure, or substantive prose; those remain
DOC-04 and human review concerns.

Entries elsewhere in the selected target are outside the approved artifact set.
Initialization MUST preserve them and MUST NOT infer project compatibility or
authority from their names or contents. Such entries make the layout invalid
only when an accepted root, containment, or artifact-path rule identifies an
objective conflict.

The same target state and template set MUST produce the same ordered plan.

## 8. Application, failures, and recovery

Application MUST re-preflight immediately before writing. Re-preflight repeats
target-ancestry inspection, artifact inspection, and declaration recognition
and compares the current result with the recorded plan, template identity, and
contract version.

### 8.1 Stale-plan refusal

If any target state, artifact state, template identity, or contract version no
longer matches the recorded plan, application MUST refuse that plan before any
write. The aggregate outcome is `Invalid`, not `Error`, because application did
not begin. Every approved artifact MUST still receive a current result:

- a compatible regular file is `Existing`, including a file that appeared
  after planning or whose bytes changed while remaining initialization-compatible;
- a conflicting, linked, reparse, special, unreadable, malformed, duplicate,
  or unsupported file is `Invalid`;
- an artifact that is currently absent is `Skipped` because the stale plan is
  not authorized to create it; and
- if the target or any target-ancestor component is unavailable, linked,
  reparse, or otherwise invalid, both artifacts are `Invalid`.

A template-identity or contract-version mismatch is a plan-level invalidity.
The artifacts are classified from the current filesystem as `Existing`,
`Invalid`, or `Skipped` using the rules above; no artifact is mislabeled merely
to manufacture an `Invalid` artifact result.

The refusal diagnostic MUST identify the recorded target and the recorded and
current template identity and contract version as applicable. For each changed
artifact, it MUST report the target-relative path, recorded classification,
current classification, and a non-content-revealing reason for the change. No
write is permitted. Retry requires producing a new plan from the current state;
application MUST NOT silently adopt or update the stale plan.

When application proceeds, artifacts MUST be attempted in plan order:

1. `project.md`;
2. `AGENTS.md`.

Each creation MUST use exclusive create semantics so an entry appearing after
preflight is never overwritten.

If an unexpected failure occurs:

- the aggregate outcome is `Error`;
- every successfully created artifact remains `Created`;
- every preserved artifact remains `Existing`;
- the failed artifact is `Invalid`, and its phase and failure cause are
  identified;
- later unattempted artifacts are `Skipped` with that reason;
- no created artifact is deleted or rolled back automatically; and
- the result explains that the user may correct the cause and retry.

The contract does not promise transactional atomicity, automatic rollback, or
recovery orchestration. A retry MUST re-plan from the actual filesystem state.
When previously created files remain compatible, they are `Existing`, and only
the remaining missing approved artifacts may be created.

## 9. Scenario matrix

| Scenario | Required plan/result | Writes allowed | Verification maturity |
| --- | --- | --- | --- |
| Existing empty target | Plan both files for creation; apply in defined order; report authoring still required in `project.md`. | Both approved files only. | Specification review now; later Core and CLI filesystem tests. |
| Equivalent implicit and explicit direct paths | The same normalized, non-link component chain produces the same ordered classifications and result. | According to the shared plan. | Specification review now; later invocation-pair integration test. |
| Compatible partial target with `project.md` | Preserve `project.md` as `Existing`; create missing `AGENTS.md`. | `AGENTS.md` only. | Specification review now; later golden-plan and integration tests. |
| Compatible partial target with `AGENTS.md` | Preserve `AGENTS.md`; create scaffold `project.md` after confirming no enclosing project. | `project.md` only. | Specification review now; later tests. |
| Complete initialized target | Classify both files `Existing`; aggregate `No change`. | None. | Specification review now; later byte-for-byte snapshot test. |
| Existing customized compatible files | Preserve both files even when their bytes differ from the current templates. | None. | Specification review now; later fixture test. |
| One exact declaration with permitted surrounding spaces | Count the top-level declaration once and preserve `project.md` as `Existing`. | Missing approved artifacts only. | Specification review now; later line-ending and whitespace fixtures. |
| Declaration text only in quoted prose, blockquote, inline code, indented code, or fenced example | Ignore the example occurrences; with no candidate declaration, classify `project.md` `Invalid` as missing. | None. | Specification review now; later Markdown-source fixtures. |
| Duplicate declaration candidates | Classify `project.md` `Invalid` as duplicate or ambiguous; classify a missing approved artifact `Skipped`. | None. | Specification review now; later fixture test. |
| Malformed or unsupported declaration candidate | Classify `project.md` `Invalid` with the specific malformed or unsupported-value reason; classify a missing approved artifact `Skipped`. | None. | Specification review now; later case, spacing, and value fixtures. |
| Existing special or linked artifact path | Classify that artifact `Invalid`; do not follow the entry. | None. | Specification review now; later platform-specific tests. |
| Unwritable target known at preflight | Report `Invalid` with the affected path and corrective action. | None. | Specification review now; later permissions tests. |
| Unexpected failure after `project.md` creation | Report `project.md` `Created`, the failed artifact `Invalid`, any later artifacts `Skipped`, and aggregate `Error`. | No further writes after failure. | Requires injected implementation-time failure test. |
| Nonexistent explicit target | Report invalid target and instruct the user to create it before retrying. | None. | Specification review now; later CLI/Core test. |
| Target or target ancestor is a link or reparse point | Classify both artifacts `Invalid`, identify the first offending component, and require a direct non-link path. The same rule applies to implicit and explicit selection. | None. | Specification review now; later cross-platform component-chain tests. |
| Target lies inside an existing project | Report invalid implicit nested-project attempt. | None. | Specification review now; later discovery fixture. |
| Rerun after successful initialization | Report both artifacts `Existing` and aggregate `No change`. | None. | Specification review now; later snapshot test. |
| Retry after reported partial failure | Preserve compatible files already created and create only still-missing approved artifacts. | Missing approved files only. | Specification review now; later injected-failure recovery test. |
| Re-preflight finds a planned missing artifact now compatible | Classify the new file `Existing`; classify unchanged compatible files `Existing` and still-absent artifacts `Skipped`; aggregate `Invalid` and require a new plan. | None. | Specification review now; later between-phase mutation test. |
| Re-preflight finds a planned missing artifact now conflicting | Classify the changed artifact `Invalid`; classify unchanged compatible files `Existing` and still-absent artifacts `Skipped`; aggregate `Invalid` and require a new plan. | None. | Specification review now; later regular-file conflict test. |
| Re-preflight finds a planned artifact now linked or reparse | Classify the changed artifact `Invalid`; classify all other artifacts from current state, using `Skipped` for absent files; aggregate `Invalid` and require a new plan. | None. | Specification review now; later link-substitution test. |
| Re-preflight finds a previously compatible artifact changed | If it remains compatible, classify it `Existing`; otherwise classify it `Invalid`. Classify every other artifact from current state, aggregate `Invalid`, and require a new plan. | None. | Specification review now; later compatible- and incompatible-edit tests. |
| Re-preflight finds a template or contract identity mismatch | Classify each artifact from current state as `Existing`, `Invalid`, or `Skipped`; aggregate `Invalid`, report recorded and observed identities, and require a new plan. | None. | Specification review now; later stale-version-plan tests. |
| Target becomes unavailable before application | Classify both artifacts `Invalid`, aggregate `Invalid`, and report the target access failure. | None. | Specification review now; later between-phase removal and permissions tests. |

## 10. Ownership boundaries

### Documentation repository

The docs repository owns:

- normative meaning of the approved skeleton;
- artifact roles and template content requirements;
- planning classifications and aggregate outcome meanings;
- non-overwrite, confinement, preflight, idempotency, and recovery guarantees;
  and
- the scenario matrix used to review later implementation evidence.

### Core repository

After the required contracts and DOC-05 dispositions are accepted, Core owns:

- the single executable copy of versioned template assets;
- deterministic planning and ordering;
- path normalization, containment, and filesystem entry checks;
- re-preflight and safe application behavior;
- structured results independent of CLI presentation; and
- Core unit and filesystem integration tests.

This work is already scoped by
[CORE-03](https://github.com/llamarc42/core/issues/3) and
[CORE-04](https://github.com/llamarc42/core/issues/4).

### CLI repository

After CLI-01 is accepted, CLI owns:

- parsing current-directory and `--directory` selection;
- invoking Core planning and application APIs;
- human and JSON rendering;
- help, output, and exit behavior; and
- presenting actionable Core errors without discarding meaning.

CLI MUST NOT contain independent template bytes, compatibility rules,
filesystem mutation logic, or a competing result model. Implementation is
already scoped by
[CLI-03](https://github.com/llamarc42/cli/issues/22).

## 11. Template compatibility and evolution

A later template version MUST NOT rewrite or migrate an existing project.
Compatibility is based on the accepted documentation contract and the limited
initialization checks in sections 6 and 7.1, not byte equality with the newest
template.

A later template version MAY change assets for newly created projects only when
the corresponding normative documentation change has been accepted. Core MUST
retain enough template identity in its result to make the selected version
inspectable, but the project does not become dependent on retaining an old
template package after creation.

Changing a project's documentation-contract version is migration and is outside
DOC-03. Initialization MUST refuse an existing `project.md` that selects a
different contract version.

## 12. Human review decisions

Acceptance requires explicit maintainer decisions on:

1. whether an existing target directory is required, rather than allowing init
   to create its parent-visible target;
2. whether rejecting every link or reparse component in the selected target's
   ancestry is the appropriate v0.1 tradeoff instead of physical alias
   resolution;
3. whether implicit nested-project creation must remain prohibited;
4. whether the non-fabricating `project.md` placeholder scaffold is acceptable
   given the non-interactive CLI shape;
5. whether the generic `AGENTS.md` entry point is part of every approved
   standalone initialization despite being optional for conformance;
6. whether the proposed template identity is sufficiently stable;
7. whether the narrow declaration grammar correctly distinguishes project
   compatibility from the complete DOC-04 validation scope; and
8. whether re-preflight drift should invalidate the recorded plan even when the
   observed filesystem state is independently compatible.

These decisions are part of DOC-03 review. This proposal does not accept them
by its presence or by merge into `history/`.

## 13. Acceptance and promotion path

This historical proposal MUST NOT be treated as the normative initialization
contract. If maintainers approve its requirements and scenarios, a separate
reviewed change must promote the accepted text into a current-guidance path,
such as:

```text
projects/llamarc42/specifications/standalone-initialization-contract-v0.1.md
```

The promoted document must remove historical-proposal metadata and any wording
that implies unreviewed acceptance. Moving or copying it into `specifications/`
is the authority-changing action under Documentation Contract v0.1.

DOC-03 must remain open until that acceptance step is complete. Acceptance of
the standalone contract does not authorize multi-repository initialization,
resolve DOC-05, or begin Core or CLI implementation.
