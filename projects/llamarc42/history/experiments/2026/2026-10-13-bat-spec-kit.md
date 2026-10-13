# BAT Spec Kit v1.1.3 Experiment Record

Date: 2026-10-13 (America/Chicago)

Document type: Historical experiment record

Authority: None. This file preserves evidence and observations; it does not
amend llamarc42 requirements, accepted ADRs, or the documentation contract.

## Purpose

This record consolidates the reusable governance findings from an independent
GitHub Spec Kit v1.1.3 experiment performed against BAT's multi-repository
architecture. BAT-specific AWS, CloudFormation, package, repository, and data
model decisions are deliberately excluded from llamarc42 requirements.

The source handoff remained outside this repository at:

`D:/CODE/Experiments/bat-spec-kit/reports/bat-speckit-experiment-handoff.md`

Its SHA-256 at review time was
`9F99388C702FAD4D3D783D597ADB9BC1EA29934E7901F6E4A54313B5FF8ADFB8`.
That location and hash are provenance, not a portable dependency of this
repository.

## Evidence classification

This record uses the following labels:

- **Artifact-verified**: reproduced directly from an available experiment file.
- **Handoff-reported**: stated in the consolidated handoff but not reproduced
  from a persisted native command result.
- **Observed**: workflow behavior described by the experiment operator.
- **Human decision**: a material clarification accepted by a person.
- **Interpretation**: a possible llamarc42 implication requiring review.

## Available evidence

The following source artifacts were available and inspected without modification:

| Artifact | SHA-256 | Use in this record |
| --- | --- | --- |
| `spec.md` | `A899AA36BFA5A387AA1323FC7EB37F00D7191E5B357EF6374A192C82EEE5BBF3` | Requirement and lifecycle checks. |
| `plan.md` | `57447AC7569EB92764CF2AC14D76041AD4BCABB4B41DB176D1C7BBE7584E6F6A` | Planning gate and stale-next-step checks. |
| `research.md` | `4B54D8C8A2E6A0BEB0F213FF3538F1804F602DE3E47459A4DD1BDE6D4A093982` | R-12 resolution evidence. |
| `tasks-firstrun.md` | `FAA9ED0F8A1D59248299E2F0EC1569B79099FCEAF037F2C441D1FC277562D0F8` | Initial graph counts. |
| `tasks.md` | `A1BB9E958ACE5DCE4E11C0065DF733A6F569E9EBEA8CA8B041A600DD5CC86F6F` | Final graph counts and gates. |

No persisted native `$speckit-analyze` result was present in the inspected
artifact set. Severity totals and analysis conclusions are therefore classified
as handoff-reported even when their downstream task corrections are visible.

## Workflow and demonstrated behavior

The handoff records use of the native Constitution, Specify, Clarify, Plan,
Tasks, and Analyze workflows. No implementation workflow was run.

The available artifacts demonstrate that the workflow produced a governed
specification, research and planning material, implementation contracts, a
quickstart, and two versions of an implementation task graph. The experiment's
derived constitution was explicitly subordinate to BAT authority; it did not
amend BAT architecture.

The following results were independently reproduced:

- **Artifact-verified:** `tasks-firstrun.md` contains 72 unique sequential task
  identifiers and 51 tasks marked parallelizable.
- **Artifact-verified:** `tasks.md` contains 79 unique sequential task
  identifiers and 52 tasks marked parallelizable.
- **Artifact-verified:** `spec.md` contains 35 functional requirements and 10
  success criteria.
- **Artifact-verified:** final tasks explicitly separate source/test authoring,
  exact upstream package availability, component-local verification, and
  cross-repository integration/runtime acceptance.
- **Artifact-verified:** final package-backed execution is ordered T061 through
  T064 while earlier source and test authoring may proceed in parallel.
- **Artifact-verified:** the specification still says "Clarified — ready for
  planning," while the plan still describes `$speckit-tasks` as the next
  workflow. The lifecycle text is stale relative to the available final graph.

The following results remain reports from the native workflow rather than
independently reproduced analysis output:

- **Handoff-reported:** final analysis covered all 35 functional requirements
  and 10 success criteria, with 100 percent requirement-to-task coverage and no
  unmapped tasks.
- **Handoff-reported:** final analysis found no CRITICAL, HIGH, or MEDIUM
  findings and one LOW lifecycle-metadata finding.
- **Handoff-reported:** final analysis judged the graph ready for implementation.
  This was a planning assessment, not implementation evidence.

## Human clarification and stop behavior

During planning, the workflow identified that the accepted empty security-group
egress requirement could not be represented faithfully using the selected
CloudFormation scope because omission would produce default allow-all behavior.

- **Observed:** planning stopped instead of inventing a sentinel, deployment
  mutation, custom resource, or architecture change.
- **Human decision:** clarification narrowed the accepted feature contract to
  require at least one supported explicit egress rule and to reject omitted or
  empty egress deterministically.
- **Artifact-verified:** the resolution is reflected in `research.md`, the
  specification, plan, and final tasks.

This is evidence that an explicit clarification gate can prevent an external
workflow from silently changing an accepted requirement. It is not evidence
that llamarc42 should adopt the BAT-specific rule.

## Generated problems and later corrections

The initial 72-task graph was structured but treated downstream package-backed
tests as executable before required exact upstream packages existed. Native
analysis also reported incomplete canonical target-isolation coverage and an
adapter proof that could run before bindable models and registration existed.

The first refinement expanded the graph to 79 tasks and added package handoffs,
selector isolation, and adapter sequencing. The handoff reports that this
iteration introduced or retained circular package prerequisites and still
conflated authored tests with executable checkpoints.

The final refinement retained 79 tasks and corrected the graph by separating:

1. source and test authoring;
2. required dependency availability;
3. component-local build and test evidence; and
4. cross-repository integration and runtime acceptance.

The architectural dependency direction did not change. The correction changed
build, publication, restore, and verification order. This distinction matters:
dependency direction and delivery execution order are related but not identical.

## Remaining limitations and untested behavior

- No production code, repository, package, build, deployment, or AWS operation
  was created or executed.
- Adapter binding and distinct manifest/output identities remained
  implementation-time stop gates.
- Exact package publication and consumption, assembled runtime composition,
  and host compatibility remained unverified.
- Spec Kit did not provide a federated Git or release manager for the independent
  repositories; the task graph had to describe those handoffs explicitly.
- Starting from the umbrella did not automatically establish descendant
  repository instruction authority.
- Repeated analysis/refinement was necessary; generated task structure and
  apparent coverage were not sufficient evidence of executable ordering.
- Lifecycle metadata did not advance automatically.

## Reusable llamarc42 lessons

The experiment supports evaluating, but does not itself accept, the following
governance refinements:

- keep derived workflow instructions subordinate to declared documentation
  authority;
- distinguish authored work from verified component and integration evidence;
- require unresolved architectural conflicts to stop for human review;
- model independent repository membership without confusing membership with a
  package dependency graph;
- keep repository targeting explicit in umbrella or multi-repository work;
- make stale lifecycle metadata detectable only where a governing contract
  defines objective state and transition rules; and
- preserve analysis/refinement findings with traceability to the artifact or
  requirement they corrected.

These lessons are assessed against current llamarc42 authority in
[`../../../specifications/governance-improvement-program-v0.1.md`](../../../specifications/governance-improvement-program-v0.1.md).
