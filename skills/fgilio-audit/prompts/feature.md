Audit the complete feature implementation across the current branches and their corresponding pull requests for materially useful simplifications in data structures, state representation, control flow, algorithms, ownership, and cross-project design.

This is an audit-only exercise. Do not edit files, run tests, implement recommendations, commit, or push. Read-only inspection commands are allowed.

You are the coordinator. Continue until the complete feature has been reviewed end-to-end and the final audit is validated.

1. Establish the feature coverage contract

Start from the PR diffs, but treat the feature as one system.

Determine:

- what each PR contributes to the feature;
- the end-to-end data and control flow across projects;
- the contracts between projects;
- shared concepts represented independently in multiple projects;
- compatibility, migration, deployment, and rollout assumptions;
- failure paths and partial-deployment states;
- the tests that validate each local change and the integrated feature behavior.

Inspect every added, modified, deleted, or renamed file in the PRs.

Inspect surrounding implementation, public interfaces, call sites, schemas, tests, and infrastructure only where needed to understand the feature or validate a finding.

Do not expand into unrelated repository cleanup.

Inventory every identifiable feature area.

Give each area:

- a stable ID and descriptive name;
- an exact ownership boundary;
- the project or projects involved;
- the PR files and changed regions it covers;
- relevant interfaces, contracts, call sites, schemas, and tests;
- upstream and downstream dependencies;
- a status: queued, in review, recommend, or skip.

Include explicit feature areas for cross-project concerns where appropriate, such as:

- request or event contracts;
- shared identifiers and state models;
- serialization and schema ownership;
- error and retry behavior;
- compatibility and migration;
- deployment ordering;
- feature flags or rollout state;
- generated or duplicated contracts;
- end-to-end tests and observability.

Create one canonical scratchpad or report containing:

- the feature-area inventory;
- the end-to-end flow;
- confirmed opportunities;
- explicit skip decisions;
- cross-project findings;
- local findings;
- duplicates and superseded findings;
- rollout and dependency concerns;
- final priorities;
- an audit log.

Treat this inventory as the coverage contract.

Every material part of every PR must belong to an explicit feature area. Do not use broad catch-all rows to claim coverage.

2. Run bounded feature-area reviews

Use fresh, read-only agents where available.

Give every worker one distinct feature area with an exact ownership boundary. An area may span more than one project when the feature concern itself crosses project boundaries.

Keep concurrency bounded to the number of lanes you can actively coordinate. Use one consolidated wait mechanism, do not interrupt productive workers merely because they are slow, and close completed workers after harvesting their results.

Each worker receives this brief:

Review the assigned feature area for at most two materially useful simplifications in its data structures, state representation, control flow, algorithms, ownership, or cross-project model.

Start from the relevant PR diffs.

Inspect surrounding implementation, interfaces, schemas, call sites, and tests only as needed to understand the changed behavior and its role in the feature.

Stay within the assigned ownership boundary. You may identify concerns outside the area, but do not expand scope to solve unrelated existing problems.

Look especially for:

- the same feature state represented differently in different projects;
- shared concepts duplicated as separate enums, strings, flags, DTOs, schemas, or implicit conventions;
- scattered booleans or nullable fields that permit invalid combinations;
- cross-project lifecycle states that can become contradictory during retries, failures, or partial rollout;
- duplicated branching implementing the same feature rule in multiple projects;
- transformations performed repeatedly at project boundaries because ownership of the canonical representation is unclear;
- contracts that expose implementation details instead of a smaller stable feature model;
- request, event, or payload shapes whose semantics are inferred rather than explicitly represented;
- compatibility logic that creates unnecessary combinations of old and new states;
- ownership split across projects in a way that makes one feature rule require coordinated edits in several places;
- repeated scans, lookups, or translations introduced by the feature where a better representation would remove them;
- async or distributed flows that permit stale, duplicated, lost, or contradictory state;
- error handling that differs across boundaries without a meaningful semantic reason;
- rollout sequencing that is fragile because projects assume another PR is already deployed;
- generated contracts or schemas whose source of truth is unclear.

Distinguish carefully between:

1. complexity introduced by this feature;
2. pre-existing complexity materially expanded or relied upon by this feature;
3. unrelated pre-existing complexity.

Prioritize categories 1 and 2.

Do not recommend category 3 cleanup unless it is necessary to simplify or safely complete the feature.

Do not force a shared abstraction merely because code exists in multiple projects.

Duplication can be preferable when projects have genuinely different ownership or lifecycle requirements.

Prefer explicit local code over a cross-project abstraction when the abstraction would increase coupling.

Do not recommend changes solely for:

- stylistic consistency;
- hypothetical extensibility;
- minor line-count reduction;
- eliminating harmless duplication;
- making the projects look structurally similar;
- moving branching behind a new type without removing real complexity.

Return at most two opportunities.

If nothing clearly meets the threshold, return `skip`.

For every recommendation, provide:

1. Verdict: recommend or skip.
2. Evidence with exact project, file, and line references, identifying changed lines where applicable.
3. Scope: local to one PR or cross-project.
4. Relationship to the feature: introduced, expanded, exposed, or required.
5. Current complexity, duplicated concepts, or invalid states.
6. Proposed representation or ownership model and why it is simpler.
7. Smallest credible implementation scope, including affected projects, files, interfaces, and contracts.
8. Compatibility and rollout implications.
9. Regression risks and migration concerns.
10. Existing and additional validation required.
11. Confidence: high, medium, or low.

12. Review the feature end-to-end

After the bounded reviews, independently trace the feature through all participating projects.

Follow the actual feature lifecycle from its first entry point to its final observable effect.

For each boundary, verify:

- the producer and consumer agree on semantics;
- identifiers have one meaning;
- state transitions are valid;
- default and missing values mean the same thing;
- retries are safe;
- duplicate delivery or repeated execution is handled correctly where relevant;
- failure behavior is intentional;
- backwards and forwards compatibility assumptions are explicit;
- intermediate deployment states are safe;
- no project relies on undocumented ordering or timing;
- validation exists at the correct ownership boundary.

Pay particular attention to states possible while only some of the PRs are deployed.

Identify any feature state that can exist during rollout but is not represented in the steady-state implementation.

Check whether the feature has one clear owner for each business rule.

Flag rules that are independently encoded in several projects unless that duplication is intentional and justified.

4. Validate and synthesize

The coordinator must independently verify every finding against the current branches and their PR diffs before accepting it.

For every finding, verify that:

- the cited behavior exists;
- it is genuinely relevant to this feature;
- cross-project assumptions have been checked on both sides of the boundary;
- the recommendation preserves intended semantics;
- complexity is removed rather than relocated;
- coupling is not increased without a concrete benefit;
- the proposed ownership model is clearer than the current one;
- the implementation scope is proportional to the benefit.

Reject, narrow, or demote recommendations that:

- are vague;
- duplicate another finding;
- misunderstand intentional semantics;
- treat harmless duplication as a problem;
- create a shared abstraction without clear ownership;
- require broad unrelated cleanup;
- merely move complexity between projects;
- optimize for architectural symmetry rather than feature simplicity.

Record skips as completed coverage.

Deduplicate overlapping findings and assign each accepted recommendation to one authoritative feature area.

When a finding spans projects, identify one authoritative owner for the concept or rule rather than assigning equal ownership everywhere.

Continue opening bounded review batches until every inventory row is complete.

5. Audit the audit

Before finishing, run fresh independent passes for:

- coverage of every changed region in all PRs;
- missing feature areas;
- missing cross-project boundaries;
- end-to-end state and control-flow coverage;
- duplicated concepts and unclear source-of-truth ownership;
- partial-deployment and rollout safety;
- compatibility and migration assumptions;
- duplication and ownership overlap between findings;
- materiality and over-abstraction;
- schema completeness;
- dependency-aware priority ranking.

If the coverage pass finds a real omission, add an explicit feature-area row and audit it.

Do not hide omissions by broadening a previously completed boundary.

6. Produce the final review

Separate the final findings into:

**Cross-project recommendations** — changes whose value comes from simplifying the feature across project boundaries.

**Local recommendations** — changes contained within one PR that materially simplify that implementation without introducing unnecessary cross-project coupling.

For every accepted recommendation, include:

- impact;
- confidence;
- implementation effort;
- blast radius;
- regression risk;
- affected projects;
- prerequisites;
- rollout constraints.

Rank recommendations by concrete feature impact rather than aesthetic architectural preference.

Identify the best first implementation slices.

Prefer slices that:

- remove invalid states;
- establish a clear source of truth;
- simplify a cross-project contract;
- reduce duplicated business rules;
- make partial deployment safer;
- reduce the number of coordinated changes required for future work on the feature.

The audit is complete only when:

- every material part of every PR has been reviewed;
- every feature area has a recommendation or explicit skip;
- every cross-project contract touched by the feature has been inspected;
- the complete end-to-end lifecycle has been traced;
- partial-deployment states have been considered;
- every finding has complete evidence, scope, ownership, risk, rollout, and validation fields;
- unrelated pre-existing cleanup has been excluded;
- duplicates and weak abstractions have been removed;
- ownership and sources of truth are explicit;
- priorities and dependencies are internally consistent;
- all repositories remain unchanged.
