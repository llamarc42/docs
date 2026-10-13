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

Current-directory and explicit-directory selection of the same physical target
MUST produce equivalent plans and results.

### 4.2 Root and containment rules

Every planned artifact path MUST:

- be a portable framework-defined path allowed by Documentation Contract v0.1;
- normalize beneath the selected target;
- contain no `.` or `..` segment after normalization; and
- be rejected if containment cannot be established before writing.

Initialization MUST NOT recursively scan siblings, parents, or arbitrary
descendants. It MAY inspect only the selected target, the two approved artifact
paths, and ancestor information needed to detect an existing enclosing project.

If the target has no `project.md` but root discovery finds a project in an
ancestor directory, initialization MUST report an invalid target rather than
silently create a nested project. Documentation Contract v0.1 permits explicit
nested projects, but this command has no explicit nested-project authorization.

### 4.3 Links and special entries

The selected target MUST NOT itself be a symbolic link, junction, mount-point
reparse point, or other directory reparse point. An approved artifact path is
invalid when an existing entry at that path is:

- a symbolic link or file reparse point;
- a directory;
- a device, socket, pipe, or other special filesystem entry; or
- anything other than a non-link regular file.

Initialization MUST NOT follow a link to decide that an artifact is safe or
compatible.

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
contains exactly one declaration selecting `llamarc42/v0.1`. Initialization
does not judge whether its name, purpose, links, or prose are substantively
complete; that belongs to validation and human review.

For `AGENTS.md`, any non-link regular file is initialization-compatible and is
preserved. `Existing` is not a claim that the file conforms to every navigation
recommendation.

### Skipped

`Skipped` means initialization deliberately performed no write for an approved
artifact and reports why. Required uses include:

- an otherwise creatable artifact blocked because another artifact made the
  complete plan invalid; and
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
| Invalid | Preflight found one or more `Invalid` conditions; no planned write occurred. |
| Error | Preflight succeeded, but an unexpected failure interrupted application. Completed and unattempted artifacts are reported honestly. |

`Applied` and `No change` are successful initialization outcomes. They do not
assert full documentation-contract validation. CLI exit behavior and serialized
output remain deferred to CLI-01.

## 7. Preflight and compatibility rules

Before any planned write, initialization MUST inspect both approved paths and
produce the complete ordered plan.

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

Entries elsewhere in the selected target are outside the approved artifact set.
Initialization MUST preserve them and MUST NOT infer project compatibility or
authority from their names or contents. Such entries make the layout invalid
only when an accepted root, containment, or artifact-path rule identifies an
objective conflict.

The same target state and template set MUST produce the same ordered plan.

## 8. Application, failures, and recovery

Application MUST re-preflight immediately before writing. If the target state,
template identity, or contract version no longer matches the plan, application
MUST refuse the plan without performing planned writes.

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
| Compatible partial target with `project.md` | Preserve `project.md` as `Existing`; create missing `AGENTS.md`. | `AGENTS.md` only. | Specification review now; later golden-plan and integration tests. |
| Compatible partial target with `AGENTS.md` | Preserve `AGENTS.md`; create scaffold `project.md` after confirming no enclosing project. | `project.md` only. | Specification review now; later tests. |
| Complete initialized target | Classify both files `Existing`; aggregate `No change`. | None. | Specification review now; later byte-for-byte snapshot test. |
| Existing customized compatible files | Preserve both files even when their bytes differ from the current templates. | None. | Specification review now; later fixture test. |
| Conflicting contract declaration | Classify `project.md` `Invalid`; block the plan and mark missing artifacts `Skipped`. | None. | Specification review now; later fixture test. |
| Existing special or linked artifact path | Classify that artifact `Invalid`; do not follow the entry. | None. | Specification review now; later platform-specific tests. |
| Unwritable target known at preflight | Report `Invalid` with the affected path and corrective action. | None. | Specification review now; later permissions tests. |
| Unexpected failure after `project.md` creation | Report `project.md` `Created`, the failed artifact `Invalid`, any later artifacts `Skipped`, and aggregate `Error`. | No further writes after failure. | Requires injected implementation-time failure test. |
| Nonexistent explicit target | Report invalid target and instruct the user to create it before retrying. | None. | Specification review now; later CLI/Core test. |
| Target is a link or reparse point | Report invalid target without following it. | None. | Specification review now; later cross-platform tests. |
| Target lies inside an existing project | Report invalid implicit nested-project attempt. | None. | Specification review now; later discovery fixture. |
| Rerun after successful initialization | Report both artifacts `Existing` and aggregate `No change`. | None. | Specification review now; later snapshot test. |
| Retry after reported partial failure | Preserve compatible files already created and create only still-missing approved artifacts. | Missing approved files only. | Specification review now; later injected-failure recovery test. |

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
initialization checks in section 6, not byte equality with the newest template.

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
2. whether implicit nested-project creation must remain prohibited;
3. whether the non-fabricating `project.md` placeholder scaffold is acceptable
   given the non-interactive CLI shape;
4. whether the generic `AGENTS.md` entry point is part of every approved
   standalone initialization despite being optional for conformance;
5. whether the proposed template identity is sufficiently stable; and
6. whether the limited `project.md` compatibility check correctly avoids
   absorbing DOC-04 validation scope.

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
