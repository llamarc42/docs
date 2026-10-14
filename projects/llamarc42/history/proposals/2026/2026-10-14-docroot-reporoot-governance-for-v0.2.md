# DocRoot and RepoRoot Governance Proposal for Documentation Contract v0.2

Date: 2026-10-14 (America/Chicago)

Status: Proposed

Target: Documentation Contract v0.2

Authority: None. Its `history/` path classifies this file as a historical record
under Documentation Contract v0.1. The status and target are descriptive only.

Dependencies: Completion of the current v0.1 documentation-framework milestone
and accepted reconciliation under
[DOC-05](https://github.com/llamarc42/docs/issues/11).

Implementation authorization: None.

Tracks: [DOC-07](https://github.com/llamarc42/docs/issues/20)

## 1. Purpose and scope

This proposal preserves candidate requirements for a possible Documentation
Contract v0.2. It does not design or accept that contract.

The candidate model addresses projects that have one project-wide documentation
authority and multiple implementation repositories. It asks how an optional
repository-specific documentation root can govern only its assigned
responsibilities without creating competing authority.

The proposal records:

- project-wide DocRoot authority;
- optional, scope-limited RepoRoot governance;
- explicit delegation and scope isolation;
- shared-contract ownership and dependency semantics;
- lightweight repository documentation;
- compatibility with standalone v0.1 projects; and
- questions requiring later architectural review.

It does not authorize a v0.2 specification, initialization behavior,
validation behavior, implementation, or new CLI surface.

## 2. Governing baseline

The accepted
[Documentation Contract v0.1](../../../specifications/documentation-contract-v0.1.md)
defines one filesystem project root marked by `project.md`. Discovery selects
the nearest root, a nested `project.md` starts a separate project, and every
contract-document identity remains relative to and inside its discovered root.
V0.1 does not define authority relationships between independent roots.

[ADR-005](../../../decisions/ADR-005-agent-agnostic-documentation-framework-boundary.md)
establishes a documentation-first, local-first, agent-agnostic framework.
Documentation remains directly usable by people, and optional tools or agents
must not replace or outrank declared authority.

[ADR-001](../../../decisions/ADR-001-repo-topology.md) records a multi-repository
topology for the original product. DOC-05 owns reconciliation of that topology,
the current repository roles, and older copilot-oriented architecture. This
proposal does not perform that reconciliation.

The earlier
[Governance Improvement Program](2026-10-13-governance-improvement-program-v0.1.md)
identified multi-repository project identity, membership, portable identity,
root/child authority, and unavailable-member handling as gaps requiring a later
architectural decision and a versioned contract change. This proposal narrows
that research to the DocRoot/RepoRoot authority model and makes lightweight
documentation a primary requirement.

## 3. Problem and motivation

A software project may span several repositories while relying on one set of
project-wide purposes, architectural constraints, boundaries, and decisions.
Individual repositories may also need local conventions, component decisions,
tests, or contracts.

Without explicit ownership, a repository may accidentally:

- claim project-wide authority for a local rule;
- redefine a contract owned elsewhere;
- duplicate project-wide requirements and allow the copies to drift;
- treat an implementation dependency as governance authority; or
- force a person or agent to infer ownership from names, Git metadata, checkout
  layout, or proximity.

The opposite failure is excessive governance. A framework must not require a
large hierarchy of repository documents merely because a project uses multiple
repositories. The intended model must work for a repository whose documentation
needs are satisfied by the initial CLI-generated structure and completed
boilerplate.

## 4. Candidate governance principles

Every principle in this section is proposed and requires later review.

### 4.1 Project authority: DocRoot

DocRoot is the authoritative documentation root for the overall project. It may
govern multiple implementation repositories.

DocRoot may establish:

- project purpose and direction;
- system-wide architecture and shared constraints;
- project-level architectural decisions;
- cross-repository responsibilities and boundaries;
- ownership of shared contracts; and
- project-wide governance.

DocRoot should state project-wide requirements and delegate repository scope. It
should not prescribe implementation details that belong entirely within an
assigned repository responsibility.

### 4.2 Repository autonomy: RepoRoot

RepoRoot represents documentation specific to one implementation repository.
It operates only within responsibilities explicitly assigned by DocRoot.

Within that scope, RepoRoot may document repository-specific architecture,
implementation conventions, component behavior, tests, validation, decisions,
and contracts. These are permitted subjects, not mandatory artifact categories.

RepoRoot may refine a project-wide requirement by adding compatible detail
inside its scope. It must not contradict, weaken, or override DocRoot.

### 4.3 Explicit delegation

RepoRoot authority originates from DocRoot's documented assignment of
responsibility. A repository cannot acquire project-wide authority by asserting
it in its own documentation.

The DocRoot assignment and RepoRoot claim must agree. A mismatch is a visible
governance conflict, not permission to infer the intended relationship.

Delegation must be discoverable through documentation. It must not be inferred
from repository names, source-control remotes, directory proximity, sibling
checkouts, or AI interpretation.

### 4.4 Scope isolation

RepoRoot may govern only its assigned responsibility.

- A repository-specific rule does not automatically govern another repository.
- A repository may describe a dependency or expectation involving another
  component without acquiring authority over that component.
- A repository may propose a change outside its scope but cannot accept that
  change on behalf of the owner.
- Architectural dependency direction, governance ownership, and build or
  release execution order remain separate concepts.

### 4.5 Contract ownership and dependency

Every shared contract must have one explicitly identifiable authoritative owner
at a time. Ownership authorizes definition and evolution of the contract,
subject to DocRoot's project-wide constraints.

A repository may use, depend on, consume, or implement a contract owned
elsewhere. Those relationships do not transfer ownership or governance
authority.

A consumer may propose a change. The proposal remains non-authoritative until
the documented contract owner and applicable project governance accept it.

Documentation should distinguish:

| Relationship | Meaning |
| --- | --- |
| Owns | Governs the accepted definition and evolution of the contract. |
| Uses or consumes | Relies on behavior defined by the contract. |
| Implements | Provides an implementation intended to conform to the contract. |
| Depends on | Requires the identified contract or artifact to fulfill local responsibilities. |
| Proposes change | Requests owner review and has no authority effect by itself. |

### 4.6 Conflict behavior

RepoRoot cannot override DocRoot. A conflict must identify the documents and
declared owners involved, remain visible, and stop work that depends on an
unresolved interpretation.

No tool or agent may resolve a conflict through:

- "most specific rule wins";
- file order or recency;
- repository or directory proximity;
- retrieval rank;
- source-control metadata; or
- inferred intent.

Resolution requires an explicit change through the applicable owner and
governance process. The correction must preserve relevant decision history.

### 4.7 Minimal governance

A repository should be able to satisfy its documentation needs using only the
initial CLI-generated structure and completed boilerplate when additional
governance is unnecessary.

Participation in a multi-repository project must not by itself require:

- repository-specific ADRs;
- separate architecture, constraints, testing, or policy documents;
- an ownership matrix;
- empty directories or placeholder artifacts;
- duplicated project-wide rules; or
- a repository governance tree.

The framework should allow the minimum facts needed for a RepoRoot relationship
to appear together in one existing document, potentially `project.md`, if later
design finds that sufficient. Additional documents should be introduced only
to record a real rule, decision, dependency, or implementation responsibility.

References to authoritative material are preferred over copied normative text.

## 5. Minimum information, not mandatory files

This proposal does not select a schema or require one document per fact.

### 5.1 DocRoot information

A future model may need the DocRoot to state:

1. project identity and purpose;
2. that it is the project-wide authority;
3. participating repositories and their assigned responsibilities;
4. cross-repository boundaries;
5. owners of shared contracts; and
6. conflict and change-governance expectations.

These facts may be combined in existing current-guidance documents. A separate
registry or ownership matrix must not be presumed necessary.

### 5.2 RepoRoot information

A participating RepoRoot may need to state:

1. repository identity and purpose;
2. the governing DocRoot identity and an explicit reference;
3. its delegated responsibility and scope;
4. contracts it owns, when any;
5. externally owned contracts it consumes or implements, when any; and
6. the route for reporting conflicts or proposing changes outside its scope.

A repository with no additional local governance should not need more than
these relationship facts and the completed initial documentation. Later design
must first evaluate whether existing `project.md` can hold them before creating
a new document type.

## 6. Discoverability and availability

A developer or agent working inside RepoRoot should begin with the RepoRoot's
documented entry point. The governing DocRoot relationship should be explicit
in current authority rather than present only in `README.md` or `AGENTS.md`,
which are navigation documents under v0.1.

A DocRoot reference or source URL is a locator, not authority. Its presence must
not:

- authorize downloading or trusting remote documentation;
- import an external inventory into the RepoRoot;
- trigger repository crawling or sibling discovery;
- make an absolute checkout path the permanent identity; or
- allow a missing DocRoot to be replaced by inferred local authority.

If required DocRoot authority is unavailable or inconsistent, a person or tool
should report that limitation rather than inventing a governing relationship.
The portable identity, locator, movement, and unavailable-root semantics remain
open design questions.

## 7. Compatibility with Documentation Contract v0.1

Any future v0.2 work must preserve:

- the valid one-file standalone project;
- standalone projects with no subordinate repositories;
- deterministic local use without a model, network, hosted service,
  source-control provider, or agent;
- v0.1 behavior for projects that continue to select v0.1;
- project-relative document identities within each v0.1 root;
- nearest-root discovery and nested-project isolation under v0.1;
- explicit rather than inferred authority; and
- the rule that navigation does not create authority.

This proposal must not be used to reinterpret v0.1. A future accepted model
must decide whether DocRoot and RepoRoot are roles within a new project model,
separate project types, or another minimal construct. That decision precedes a
versioned contract specification.

Ordinary Markdown can express the candidate relationships for human review
today. V0.1 does not make cross-root delegation, ownership, discovery, or
conflict handling deterministic or machine-verifiable.

## 8. Illustrative BAT example

BAT is an example only and does not define llamarc42 requirements.

- **BAT DocRoot** owns project architecture, shared constraints, repository
  responsibilities, and contract ownership assignments.
- **Core Abstractions RepoRoot** owns implementation-independent plugin
  contracts and shared resource abstractions.
- **Cloud Provider RepoRoot** implements the Core Abstractions contract and owns
  provider-specific models, defaulting, validation, and tests.
- **Terraform Projection RepoRoot** owns mapping provider resources into
  Terraform representations.
- **Terraform Output RepoRoot** owns rendering and output-specific behavior.

Cloud Provider's dependency on Core Abstractions does not allow Cloud Provider
to redefine the shared plugin interface. Terraform Output may document an input
expectation, but it cannot establish provider defaulting or validation rules.
Either consumer may propose a change to an owner without gaining authority to
accept that change.

None of these RepoRoots necessarily requires a separate ADR, architecture
hierarchy, constraint document, testing guide, or ownership matrix. Additional
documentation is justified only by actual repository needs.

## 9. Open architectural questions

Later review must preserve rather than prematurely resolve these questions:

1. How can DocRoot delegate responsibility to RepoRoot with the least required
   documentation?
2. Does RepoRoot require a distinct document type, or can existing `project.md`
   express its relationship to DocRoot?
3. How can RepoRoot reference an external DocRoot without creating implicit
   authority or remote-fetch requirements?
4. What is the minimum information needed to establish shared-contract
   ownership?
5. What happens when DocRoot is unavailable, moved, or conflicts with RepoRoot?
6. Can repository governance be added incrementally without changing the
   initial documentation structure?
7. How can deterministic validation remain lightweight and structural without
   semantic interpretation?
8. How will v0.2 preserve compatibility for standalone projects with no
   subordinate repositories?
9. Does portable multi-root identity require one new artifact, or can existing
   artifacts express it without duplication?
10. Which relationships, if any, require machine verification rather than
    human-readable documentation alone?

## 10. Explicit non-goals

This proposal does not introduce or authorize:

- a Documentation Contract v0.2 specification;
- a v0.2 template or initialization contract;
- new CLI commands or changes to current CLI/Core backlogs;
- federated or distributed document inventory;
- repository crawling or automatic sibling discovery;
- remote authority resolution or fetching;
- a central registry, database, index, or hosted service;
- APIs, plugins, integration interfaces, model routing, or agent orchestration;
- semantic conflict detection or automatic cross-root reconciliation;
- source-control-derived identity, trust, membership, or authority;
- mandatory RepoRoot ADRs, architecture documents, guides, constraints,
  ownership matrices, or governance trees; or
- duplicated project-level policy in every repository.

## 11. Future acceptance requirements

Before any DocRoot/RepoRoot behavior becomes normative:

1. the current v0.1 documentation-framework milestone must complete without
   expansion by this proposal;
2. DOC-05 must apply accepted dispositions to the existing architecture,
   repository roles, and ADRs;
3. a separately reviewed architectural decision must define the minimal
   multi-root authority model and its compatibility boundary;
4. a versioned Documentation Contract v0.2 proposal must define any normative
   representation and finite verification scenarios;
5. reviewers must confirm that the model requires no unnecessary repository
   artifacts and preserves standalone use;
6. any later initialization or validation changes must receive their own
   accepted specifications and backlog authorization; and
7. implementation must remain blocked until those governing inputs are
   accepted.

Merging this historical proposal or closing DOC-07 would preserve research only.
Neither action accepts v0.2 architecture or authorizes implementation.

## 12. Backlog boundaries

- [DOC-03](https://github.com/llamarc42/docs/issues/9) remains limited to the
  standalone v0.1 initialization contract.
- [DOC-04](https://github.com/llamarc42/docs/issues/10) remains limited to
  deterministic validation for accepted v0.1 requirements.
- [DOC-05](https://github.com/llamarc42/docs/issues/11) remains responsible for
  reconciling current architecture and repository roles, not accepting this
  generic v0.2 model.
- [DOC-06](https://github.com/llamarc42/docs/issues/17) remains the completed M1
  experiment and multi-repository gap record.
- DOC-07 preserves this narrower future candidate and does not change a current
  milestone, sprint, Core issue, or CLI issue.
