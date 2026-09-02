Audit the current branch and its pull request for materially useful simplifications in the code introduced or materially affected by this PR, focusing on data structures, state representation, control flow, algorithms, and ownership.

This is an audit-only exercise. Do not edit files, run tests, implement recommendations, commit, or push. Read-only inspection commands are allowed.

You are the coordinator. Continue until the complete PR scope has been reviewed and the final audit is validated.

1. Establish the PR coverage contract

First determine the PR's effective scope.

Inspect:

- the current branch relative to its merge base with the target branch;
- every added, modified, deleted, or renamed file;
- the changed functions, classes, modules, components, schemas, and interfaces;
- directly affected call sites, consumers, tests, and public interfaces where needed to understand the change;
- surrounding implementation only when necessary to establish semantics, ownership, or regression risk.

Do not audit unrelated areas of the repository merely because they are nearby or share infrastructure.

Inventory every identifiable change area in the PR.

Give each change area:

- a stable ID and descriptive name;
- an exact ownership boundary;
- the PR files and changed regions it covers;
- relevant surrounding implementation, public interfaces, major call sites, and tests;
- a status: queued, in review, recommend, or skip.

Create one canonical scratchpad or report containing:

- the PR change-area inventory;
- confirmed opportunities;
- explicit skip decisions;
- cross-cutting patterns introduced or exposed by the PR;
- duplicates and superseded findings;
- final priorities and dependencies;
- an audit log.

Treat this inventory as the coverage contract. Every material part of the PR must belong to an explicit change area. Do not use broad catch-all rows to claim coverage.

2. Run bounded change-area reviews

Use fresh, read-only agents where available. Give every worker one distinct change area with an exact, non-overlapping ownership boundary.

Keep concurrency bounded to the number of lanes you can actively coordinate. Use one consolidated wait mechanism, do not interrupt productive workers merely because they are slow, and close completed workers after harvesting their results.

Each worker receives this brief:

Review the assigned portion of the current PR for at most two materially useful simplifications in its data structures, state representation, control flow, algorithms, or organizing model.

Start from the PR diff. Inspect surrounding implementation, interfaces, call sites, and existing tests only as needed to understand the changed behavior and determine whether the PR introduces, preserves, or unnecessarily increases complexity.

Stay within the assigned ownership boundary. You may identify cross-area concerns, but do not expand the scope to audit unrelated existing code.

Look especially for:

- new or modified scattered booleans or nullable fields that permit invalid combinations and should become a state machine or discriminated union;
- repeated assumptions about object shape introduced or reinforced by the PR that need a shared typed model;
- duplicated branching added or modified by the PR that a small map, registry, reducer, or command model would remove;
- unclear state or behavior ownership created or worsened by the change that a small module boundary would clarify;
- repeated scans, transformations, or lookups introduced by the PR where a more appropriate collection or index would materially simplify behavior;
- lifecycle, concurrency, or async states touched by the PR whose representation permits stale or contradictory state;
- compatibility or migration logic whose control flow can be simplified without obscuring required transitional behavior;
- newly duplicated concepts across files that should have one authoritative representation.

Distinguish carefully between:

1. complexity introduced by this PR;
2. pre-existing complexity that this PR materially expands or depends on;
3. unrelated pre-existing complexity.

Prioritize categories 1 and 2. Do not recommend cleanup of category 3 unless the PR cannot be simplified safely without addressing it.

Do not force an abstraction. Prefer boring local code when it is already clear.

Do not recommend changes solely for stylistic consistency, hypothetical extensibility, minor line-count reduction, or moving existing branching behind a new type.

Return at most two opportunities. If nothing clearly meets the threshold, return `skip`.

For every recommendation, provide:

1. Verdict: recommend or skip.
2. Evidence with exact file and line references, identifying changed lines where applicable.
3. Relationship to the PR: introduced, expanded, exposed, or required by the change.
4. Current complexity or invalid states.
5. Proposed representation and why it is simpler.
6. Smallest credible implementation scope, including affected files and interfaces.
7. Regression risks and migration concerns.
8. Existing and additional validation required.
9. Confidence: high, medium, or low.

10. Validate and synthesize

The coordinator must independently verify every finding against the current branch and its diff before accepting it.

For each finding, verify that:

- the cited behavior actually exists on the current branch;
- the recommendation is relevant to the PR rather than unrelated repository cleanup;
- the proposed simplification preserves intended semantics;
- the complexity is genuinely removed rather than moved elsewhere;
- the implementation scope is proportional to the PR.

Reject, narrow, or demote recommendations that are vague, duplicate another finding, misunderstand intentional semantics, exceed the PR's reasonable scope, or merely relocate complexity.

Record skips as completed coverage.

Deduplicate overlapping findings and assign each accepted recommendation to one authoritative change area.

Continue opening bounded review batches until every inventory row is complete.

4. Audit the audit

Before finishing, run fresh independent passes for:

- PR coverage and missing change-area boundaries;
- changed files or changed behaviors that were not reviewed;
- duplication and ownership overlap between findings;
- materiality and over-abstraction;
- whether findings are genuinely attributable or relevant to this PR;
- schema completeness;
- dependency-aware priority ranking.

If the coverage pass finds a real omission, add an explicit change-area row and audit it. Do not hide it by broadening a previously completed boundary.

Rank the final recommendations by:

- concrete impact on this PR;
- confidence;
- implementation effort;
- blast radius;
- regression risk;
- prerequisites.

Identify the best first implementation slices, favoring changes that can be made cleanly within this PR without unnecessary expansion of scope.

The audit is complete only when:

- every material part of the PR has been reviewed;
- every change area has a recommendation or explicit skip;
- every finding has complete evidence, PR relationship, scope, risk, and validation fields;
- unrelated pre-existing cleanup has been excluded;
- duplicates and weak abstractions have been removed;
- priorities and dependencies are internally consistent;
- the repository remains unchanged.
