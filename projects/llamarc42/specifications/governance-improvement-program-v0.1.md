# Governance Improvement Program v0.1

Version: 0.1

Status: Proposed

Authority: None until explicitly reviewed and accepted through the established
documentation and ADR process.

## 1. Purpose and scope

This proposal records the evidence, candidate requirements, and backlog mapping
for a four-milestone llamarc42 improvement program. It is a review artifact, not
an accepted architecture decision or an amendment to the accepted
[`documentation-contract-v0.1.md`](documentation-contract-v0.1.md).

Milestone 1 is documentation and planning only. This proposal does not authorize
implementation of CLI commands, registries, validators, retrieval, sessions,
agent behavior, external workflows, or llamarc42.con integration.

## 2. Governing inputs and authority

The following accepted documents govern this proposal:

- [`documentation-contract-v0.1.md`](documentation-contract-v0.1.md), accepted
  through [DOC-02](https://github.com/llamarc42/docs/issues/8) and
  [PR #16](https://github.com/llamarc42/docs/pull/16);
- [`ADR-005-agent-agnostic-documentation-framework-boundary.md`](../decisions/ADR-005-agent-agnostic-documentation-framework-boundary.md),
  accepted through [DOC-01](https://github.com/llamarc42/docs/issues/7); and
- the still-current, known-conflicting documents and ADR-001 through ADR-004,
  whose explicit reconciliation remains assigned to
  [DOC-05](https://github.com/llamarc42/docs/issues/11).

The accepted contract gives current authority to the project descriptor,
accepted non-superseded ADRs, and current-guidance documents. Known conflicts
must be named and left visible until an explicit documentation change or valid
ADR relationship resolves them. Historical records, navigation, issue state,
generated workflow artifacts, and this proposal cannot supersede current
authority.

## 3. Verified current-state baseline

Baseline date: 2026-10-13 (America/Chicago)

### 3.1 Repository state

- Repository: `llamarc42/docs`
- Remote: `git@github.com:llamarc42/docs`
- Default branch: `main`
- Verified `origin/main`: `a05404b9f139ffc11927a5f76ed9424646011fe1`
- That commit is the merge of PR #16.
- The accepted documentation contract blob at the baseline is
  `886c5100306b02c3baaddbf2d4e0b103961df7ab`.
- The accepted ADR-005 blob at the baseline is
  `ae7e27a9e348566756d16242d217bd53f3dc12c6`.
- Discovery began from a clean historical DOC-02 branch. Work was then based on
  the verified `origin/main` commit on `docs/m1-governance-foundation`.

### 3.2 Historical baseline verification

The following supplied historical facts were verified:

- DOC-02 is closed as completed.
- Documentation Contract v0.1 is on `main` with `Status: Accepted`.
- PR #16 is merged.
- The public Llamarc42 Board shows DOC-01 and DOC-02 as Done in sprint `26_21`.
- The board's current sprint view is still filtered to `26_21`; it contains no
  open items. This proposal does not change that commitment.

### 3.3 Current backlog and milestones

The docs repository has three GitHub milestones:

| GitHub milestone | State | Relevant work |
| --- | --- | --- |
| Docs Foundation | Closed | Original repository, documents, ADRs, and quality set. |
| [Documentation Framework Definition](https://github.com/llamarc42/docs/milestone/2) | Open | DOC-01 through DOC-05; DOC-03, DOC-04, and DOC-05 remain open. |
| [Framework Validation and Adoption](https://github.com/llamarc42/docs/milestone/3) | Open | VAL-01 through VAL-03, all open. |

Open framework issues already define substantial future work:

- [DOC-03](https://github.com/llamarc42/docs/issues/9): standalone
  initialization contract and template ownership;
- [DOC-04](https://github.com/llamarc42/docs/issues/10): deterministic
  validation and diagnostics;
- [DOC-05](https://github.com/llamarc42/docs/issues/11): accepted-document,
  ADR, repository-role, and roadmap reconciliation;
- [VAL-01](https://github.com/llamarc42/docs/issues/12): normative fixtures;
- [VAL-02](https://github.com/llamarc42/docs/issues/13): dogfooding; and
- [VAL-03](https://github.com/llamarc42/docs/issues/14): adoption and manual
  migration guidance.

The Core and CLI repositories already contain open implementation backlogs:

- CORE-01 through CORE-04 cover project discovery, validation, versioned
  templates/planning, and safe application.
- CLI-01 through CLI-05 cover replacement of the old chat-oriented CLI contract,
  host/output foundations, `init`, `validate`, and offline packaging.

The accepted CLI repository file remains CLI Specification v0.1 for the prior
copilot surface. CLI-01 explicitly owns its replacement. This proposal does not
silently edit or reinterpret that specification.

### 3.4 Known authoritative conflict

Accepted ADR-005 establishes an agent-agnostic documentation framework, while
the constitution, principles, project description, constraints, architecture,
invariants, quality documents, and ADR-001 through ADR-004 still contain
copilot, model, retrieval, session, and surface requirements. ADR-005 and the
documentation contract require that conflict to remain visible. DOC-05 owns its
resolution. That reconciliation must be considered before the proposed
multi-repository governance contract can be accepted during M2. This proposal
does not perform the reconciliation or select a side document-by-document.

## 4. External experiment evidence

The BAT Spec Kit evidence is preserved as the non-authoritative historical
record
[`../history/experiments/2026/2026-10-13-bat-spec-kit.md`](../history/experiments/2026/2026-10-13-bat-spec-kit.md).

Reusable conclusions are limited to governance behavior: explicit authority,
human clarification gates, evidence maturity, deterministic repository
targeting, task-graph refinement, and lifecycle traceability. BAT-specific AWS
and package designs are not candidate llamarc42 contract requirements.

## 5. Documentation-contract gap assessment

### 5.1 Summary

| Candidate concern | Current coverage | Actual gap | Governance path | Recommended owner/milestone |
| --- | --- | --- | --- | --- |
| Multi-repository project identity | v0.1 defines one filesystem root, upward nearest-root discovery, and nested-project exclusion. ADR-001 accepts multiple repositories but under the prior product model. | Independent repositories cannot be declared as members of one governed project with one authority root. | A new accepted ADR should decide the multi-repository project model. A separately versioned contract amendment must follow; v0.1 must remain unchanged. | Docs / M2 prerequisite; coordinate with DOC-03, DOC-04, and DOC-05. |
| Stable project and member identity | v0.1 gives documents stable project-relative identities but gives no project/member identifier independent of checkout path. | Movement or absence of a checkout cannot be distinguished from loss of project identity. | Contract amendment after the project-model ADR. | Docs / M2. |
| Optional source metadata | v0.1 deliberately does not depend on source control or network lookup. | There is no optional, non-authoritative locator for Git, SVN, other VCS, or unversioned directories. | Contract amendment; validation catalog for syntax and consistency only. | Docs / M2, then Core/CLI implementation. |
| Explicit membership states | v0.1 inventories only files physically inside one discovered root. | It cannot represent registered/available, registered/unavailable, authoritative-root, or broken relationship states. | Contract and validation amendments. | DOC-03/DOC-04 extensions or focused successors in M2. |
| Root/child authority | Nearest `project.md` wins locally; nested roots are separate projects. | A child repository cannot explicitly name exactly one external governance root, and conflicting declarations have no defined diagnostic. | ADR plus contract amendment. | Docs / M2. |
| Evidence and execution gates | The contract separates current authority from history and forbids inference, but does not define implementation-evidence maturity. | "Tests authored" can still be mistaken for "tests executable/passed" across unavailable dependencies. | Prefer a minimal milestone/evidence convention. Only add validation when fields and transitions are objective. This does not require a dependency engine. | Guidance in M1; normative refinement with DOC-04 or a successor before M2 execution reporting. |
| Artifact lifecycle freshness | ADR lifecycle and specification acceptance are explicit. General historical/current classes are already defined. | Other workflow artifacts can contain stale prose, but no universal lifecycle exists or should be inferred. | Reuse existing authority classes. Define lifecycle only in an artifact's governing specification; add deterministic stale-state checks only where explicit metadata allows them. | DOC-04 / M2; no second state registry. |
| External workflow/agent boundary | ADR-005 makes integrations optional and subordinate. `AGENTS.md` is navigation only. | Multi-repository context handoff and explainable root selection are not specified. | Define consumption boundaries after the project model, without adopting an external workflow. | M3. |

### 5.2 Compatibility constraints

Any accepted refinement must preserve:

- the one-file standalone project as a valid mode;
- deterministic local operation without a model, network, hosted service,
  source-control provider, or agent;
- v0.1 semantics for projects that do not opt into a later contract version;
- project-relative document identities within each governance root;
- explicit rather than inferred authority;
- no recursive workspace discovery, sibling scanning, remote fetching, or trust
  derived from a remote URL; and
- the distinction between project membership, architectural dependencies, and
  build/release execution order.

## 6. Proposed multi-repository requirements

All requirements in this section are proposed and require review.

### 6.1 Initialization surface

The candidate commands are compatible with the direction of DOC-03 and CLI-01
only if their semantics are accepted before implementation:

```text
llamarc42 init
llamarc42 init --multi root
llamarc42 init --multi child <path-to-root>
```

`llamarc42 init` must retain standalone behavior. The `--multi` forms extend
rather than replace it. CLI-01 currently specifies only standalone `init` and
`validate`; accepting these flags would require an explicit CLI contract change.

### 6.2 Identity and registry semantics

An accepted multi-repository model should require:

1. one stable project identifier at the governance root;
2. stable member identifiers that do not contain absolute local paths or depend
   on a source-control provider;
3. explicit member roles such as governance documentation, implementation,
   tooling, or tests, without making the role list an execution graph;
4. optional source-control type, optional source URL, and optional portable
   local-location hint;
5. explicit membership that remains recorded when a member is not checked out;
6. exactly one authoritative governance root reference for each child; and
7. source metadata that acts only as a locator and never grants authority,
   download permission, or trust.

This proposal intentionally does not select YAML, JSON, filenames, or a schema.
The accepted project model should first decide whether the root registry and
child reference extend an existing contract artifact or require one new
artifact. Multiple overlapping registries are prohibited.

### 6.3 Deterministic resolution states

Resolution must distinguish at least:

- registered and locally available;
- registered but locally unavailable;
- locally available but not registered;
- child with a valid authoritative-root relationship;
- child with a missing or invalid root;
- root and child with inconsistent membership data;
- duplicate project or member identifiers; and
- conflicting root references.

No state may be repaired or selected based on proximity, working directory,
Git remote, sibling discovery, or filesystem traversal. A tool must report the
conflicting declarations and require explicit correction.

### 6.4 Root and child initialization

A multi-root may contain documentation only. It must not require Git metadata,
implementation source, sibling checkouts, or network access.

The child command's supplied root path is setup input used to validate the root
and establish an explicit relationship. The permanent child identity must not
be that absolute path. Moving either checkout must not change project or member
identity; it may invalidate only an optional local-location hint.

### 6.5 Registration safety and recovery

Normative scenarios must define deterministic diagnostics, visible partial
outcomes, and explicit recovery for:

- invalid or unsupported roots;
- children that already contain documentation;
- already registered children;
- one-sided registration after an unexpected write failure;
- moved roots or children;
- registered but unavailable members;
- duplicate identifiers;
- changed repository URLs; and
- absent or unknown source-control metadata.

Known conflicts must be preflighted before planned writes. Existing files must
never be silently overwritten. Full transactional atomicity or automatic
rollback is not required; when an unexpected failure occurs, the tool must
report which side changed, which side did not, and the explicit retry or manual
repair procedure. An idempotent retry should converge only when the recorded
identities and intended relationship agree.

## 7. Evidence and milestone feedback convention

Every milestone handoff should record:

- objective and governing inputs;
- completed work;
- verification evidence;
- findings and unresolved risks;
- impact on earlier work;
- impact on future work; and
- required follow-up decisions.

Evidence claims should use these four states when implementation ordering is
material:

1. **Authored**: source or tests exist; no execution claim.
2. **Dependency available**: required declared inputs are resolvable.
3. **Component verified**: component-scoped checks executed successfully.
4. **Integration verified**: the compatible multi-component assembly executed
   its acceptance checks.

These labels describe evidence, not artifact authority and not a task scheduler.
They must not be used to infer architectural dependency direction.

Later evidence may challenge an earlier decision but cannot supersede it.
Corrections must cite the original finding and authoritative requirement,
preserve completed history, and use the existing ADR/specification change
process. Downstream work must not depend on an unaccepted upstream proposal.
An existing document or issue should be refined when it already owns the scope;
a competing decision or status registry should not be created.

## 8. External workflow and agent compatibility

The intended boundary is:

- **llamarc42:** authoritative documentation, declared project structure,
  deterministic discovery, and validation;
- **external workflows:** specification development, planning, orchestration,
  review, and implementation under their own workflow ownership; and
- **llamarc42.con:** an optional, independent local-first execution surface that
  may consume accepted governance through a separately approved integration.

Future adapters for Spec Kit, Squad, Codex, GitHub Copilot, Claude, Ollama-based
models, or other tools must consume the same visible framework artifacts, cite
the authoritative root selected, expose conflicts rather than choosing silently,
and remain optional. They must not replicate the external tool's workflow inside
llamarc42 or make `.con`, API keys, hosted inference, or cloud services a
framework prerequisite.

## 9. Reconciled four-milestone roadmap

These program labels do not replace GitHub milestones.

### M1 — Governance Foundation and Experiment Consolidation

Deliver this proposal, the experiment record, validation evidence, and a review
handoff. M1 maps to the existing Documentation Framework Definition milestone
and informs DOC-03 through DOC-05. It is tracked by
[DOC-06](https://github.com/llamarc42/docs/issues/17) and submitted for review
in draft [PR #18](https://github.com/llamarc42/docs/pull/18). DOC-06 has no
sprint assignment.

Exit requires review of this proposal, not architectural acceptance. M1 does
not close DOC-03, DOC-04, or DOC-05.

### M2 — Initialization and Validation

First accept the multi-repository project-model ADR and a separately versioned
documentation-contract amendment, with the DOC-05 reconciliation outcome
considered before either multi-repository governance artifact is accepted. Then
refine DOC-03 and DOC-04, update CLI-01, and implement through CORE-01 through
CORE-04 and CLI-02 through CLI-05. VAL-01 provides fixtures; VAL-02 and VAL-03
remain later validation/adoption work.

M2 may include standalone init, multi-root and multi-child init, explicit
membership, portable identity/source metadata, deterministic validation, and
recovery behavior. It must not begin implementation against unaccepted
contracts.

### M3 — Governance Consumption and Agent Compatibility

After M2 establishes the project model and deterministic validation, define thin
optional entry points, explainable root selection, cross-repository governance
consumption, and compatibility checks. Existing `AGENTS.md` rules remain valid:
entry points navigate but do not create authority. No existing issue currently
owns the full M3 scope; create one only after M2 contracts stabilize.

### M4 — External Workflow Evaluation and Optional Integration

Evaluate Squad, Spec Kit adapters, llamarc42.con compatibility, and local-first
feasibility/cost after M3 proves consumption boundaries. Adoption is not an exit
requirement; a documented rejection is valid. No current docs issue owns this
scope, and llamarc42.con remains independent under ADR-005.

### 9.1 Duplicate and dependency reconciliation

- Standalone initialization and validation are already owned; do not duplicate
  DOC-03, DOC-04, CORE-01 through CORE-04, or CLI-01 through CLI-05.
- DOC-05 remains the owner of accepted-document and repository-role
  reconciliation; this proposal must not absorb it.
- Multi-repository contract changes are new scope and should amend the existing
  initialization/validation issues or use focused successor issues after the
  governing ADR is accepted.
- Agent compatibility and external workflow evaluation are intentionally later
  than deterministic project membership and validation.
- Existing milestone and sprint assignments remain unchanged.

## 10. Actionable follow-up work

The minimum follow-up set is:

1. Review DOC-06 and draft PR #18 without assigning the issue to a sprint or
   treating the proposal as accepted architecture.
2. Review this proposal and decide whether to authorize a proposed ADR for the
   multi-repository project model.
3. If authorized, draft the ADR and separately versioned contract amendment;
   do not edit v0.1 in place.
4. Refine DOC-03, DOC-04, and CLI-01 acceptance criteria after that decision;
   preserve their current standalone commitments.
5. Add multi-repository fixtures to VAL-01 only after the contract is accepted.
6. Create M3 and M4 issues when their upstream contracts are stable rather than
   pre-assigning speculative implementation.

## 11. Milestone 1 verification and handoff

### Completed checks

- fetched and verified current `origin/main`;
- verified DOC-02, PR #16, accepted contract status, and sprint `26_21` history;
- reviewed accepted ADR-005, ADR-001 through ADR-004, current guidance, the old
  CLI specification, and current docs/Core/CLI backlogs;
- inspected the external handoff and available experiment artifacts;
- independently reproduced 72/51 and 79/52 task/parallel counts plus 35/10
  requirement counts;
- preserved the experiment as history and marked this program Proposed;
- left Documentation Contract v0.1 and all accepted ADRs unchanged;
- modified no runtime or implementation repository; and
- installed or integrated no external workflow.

### Validation results

- Relative Markdown links in both new files resolved successfully.
- No trailing whitespace was found.
- Portable filenames and framework-defined placement were reviewed against
  Documentation Contract v0.1.
- Accepted contract and ADR-001 through ADR-005 blob hashes matched the
  `origin/main` baseline; none was modified.
- The working-tree inventory contained only the two Milestone 1 Markdown files.
- The local `cli`, `core`, `powershell`, `api`, and `llamarc42.con` working trees
  remained clean; no implementation or unrelated repository was modified.
- Proposed and historical authority labels were reviewed manually.

The repository provides no automated documentation workflow or Markdown-lint
configuration, and no Markdown linter was installed. Link resolution, authority
labeling, naming, placement, and scope were therefore verified with bounded
manual and scripted checks. Git staged-diff and whitespace checks are repeated
before commit.

### GitHub handoff

DOC-06 is open in the Documentation Framework Definition milestone with no
sprint assignment. Draft PR #18 targets `main`, links DOC-06, and contains the
Milestone 1 documentation changes and verification evidence. Neither artifact
is closed, merged, or evidence that this proposal has been accepted.

## 12. Open architectural questions

Human review must decide:

- whether the multi-repository project model warrants a new ADR or an explicit
  amendment to an existing accepted decision;
- the minimal authoritative artifacts for root membership and child reference;
- identifier format and collision domain without tying identity to a provider;
- which member roles are closed vocabulary versus project-defined extension;
- how portable local-location hints are resolved without implicit discovery;
- whether one-sided registration recovery needs additional normative states;
- which lifecycle fields are objective enough for deterministic validation; and
- whether M3/M4 integration candidates merit adoption after bounded experiments.

The recommended next action is review of this proposal and authorization of the
multi-repository project-model decision work. M2 implementation must not begin
before those contracts are accepted.
