# Android EPIC agent instructions

Apply this workflow to the user's Android EPIC. The user supplies desired behavior and constraints; you discover technical details, maintain records, and generate concrete continuation prompts.

## Planning

When asked to plan, inspect applicable repository instructions and relevant architecture, build variants, tests, and CI. Create a specification and progress log in the repository's established documentation location. Include observable acceptance criteria with stable IDs, non-goals, key decisions, relevant Android risks, and dependency-ordered vertical slices with IDs and validation approaches. Keep slices small enough for bounded sessions. Recommend the first slice and return its exact authorization prompt. Do not implement during planning.

## Execution

Execute only the authorized slice. Complete its implementation, relevant validation, and fixes for confirmed issues caused by the change. Compilation or a first implementation alone is not completion. Follow existing patterns and preserve unrelated changes.

Proceed with routine local edits, development checks, and implementation decisions without repeated approval. Ask when a decision materially changes product behavior or scope, makes an external commitment, or exceeds authorization. Continue unaffected work while awaiting answers. If an instruction causes a pause, identify its source and exact rule and resolve conflicts using the applicable instruction hierarchy.

Read only relevant code and documentation; reuse established knowledge. Select checks by changed behavior and repository requirements. Rerun checks when changes or failures justify it. Consider lifecycle recreation/process death, coroutine cancellation, persistence, offline/retry behavior, navigation, permissions, and accessibility where relevant. Record unavailable validation honestly.

If a slice is clearly too large, record a smaller coherent subdivision and execute only its first part. Do not silently reduce acceptance criteria or claim the original slice is complete.

## Delegation

The main agent owns integration; preferred main settings are Astra/low or Sol/medium, selected by the user. Run short checks directly. Delegate only bounded independent work when its benefit justifies overhead, with at most one subagent at a time. Explicitly select Luna/low for mechanical verification, Terra/medium for ordinary correctness review, or Sol/medium for a justified complex independent investigation. Retain difficult integration judgment yourself.

Fill delegation packets yourself: actual workspace, stable code state, scope, acceptance criteria, commands/scenarios, environment, and evidence locations. Verifiers may generate artifacts but cannot edit source/configuration/tests; reviewers cannot edit files. Require actual results, actionable findings with file references and triggers, and verification/review limits. Resolve confirmed findings before completion. Avoid editing reviewed files, competing Gradle builds, or shared emulator use during checks. Account for uncommitted changes when identifying snapshots; a commit ID alone is insufficient.

## Checkpoint and stop

Update the progress log at meaningful milestones and before expensive work: slice/status, workspace/branch/code identity, changed files, decisions, acceptance evidence, actual checks and outcomes, remaining issues, and next action. Usage may end before a final response; maintain accurate partial progress. Never mark skipped checks as passed.

Stop after the authorized slice is complete or genuinely blocked after finishing unaffected work. Do not begin another slice or schedule continuation without explicit user authorization. A request to resume continues only the unfinished authorized slice.

Return concise status, delivered behavior, validation/gaps, and the proposed next action. Recommend the same conversation for unfinished or closely related work; recommend a new conversation at a clean boundary when prior context is stale or largely irrelevant. Usage reset alone is not a reason to restart.

Provide a ready-to-paste continuation prompt with actual absolute paths to this instruction file, the specification, and progress log, plus the actual slice ID. For partial work, resume the current slice; for completed work, propose the next slice for authorization. Fill all engineering fields yourself; do not return placeholders for the user to construct.
