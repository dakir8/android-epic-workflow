# Android EPIC Development Workflow

Human reference guide. For agent execution, reference the compact sibling file `android-epic-agent.md`. The longer templates below explain the workflow; you do not need to send them to the agent.

This workflow is for medium-to-hard Android EPICs when usage credits may run out before the work is finished. The main agent owns implementation and integration. Human approval is required before starting the next slice; there is no automatic continuation.

## Quick start: what you actually type

You provide the feature request, known constraints, and authorization to work on a slice. The main agent fills in the engineering details below. You do not need to prepare subagent prompts or maintain the checkpoint yourself.

For the example below, first copy `android-epic-agent.md` into your Android repository root. You only need the compact file in that project. Alternatively, keep it elsewhere and replace the first line with its actual absolute path.

Once per EPIC, in the Android project's conversation, reference the compact file and describe the feature:

```text
Follow `android-epic-agent.md` from the repository root.
Plan an EPIC for [describe the feature and desired behavior].
Constraints: [known requirements, or say none known].
Create the specification and progress log, assign slice IDs, and recommend
the first slice. Give me the exact prompt to authorize it. Do not implement yet.
```

That is the only template here that normally needs your own feature description. Planning produces the rest. You can also use the longer initial design prompt if you already have detailed requirements.

After reviewing the plan, in the same conversation:

```text
Execute the first recommended slice under the saved one-slice workflow.
Complete its implementation, validation, and fixes, then stop.
```

After each completed slice, use the exact next-slice prompt returned by the agent, or say:

```text
Execute the next recommended slice under the same workflow, then stop.
```

If the last slice was interrupted instead, say:

```text
Resume the unfinished authorized slice under the same workflow, then stop.
```

You do not need to paste full Prompt 2 at every slice. The compact agent file contains the execution rules. In a new conversation, explicitly reference `android-epic-agent.md`, the specification, progress log, and authorized slice. The agent must return a ready-to-paste handoff with actual absolute paths and the actual slice ID, not placeholders. Open the same Android project/worktree; if it has moved, have the agent reconcile the paths and working tree first.

Saving the compact file does not automatically load it in every conversation. Reference it when starting a new one. If it is unavailable on another machine, copy it there or paste its contents once in that conversation. Reading the file still uses context; the compact version avoids loading this longer human guide.

## Who fills in each field?

| Template field | Filled by | Meaning |
|---|---|---|
| EPIC name and desired outcome | You describe it; agent can name it | The feature or problem to solve |
| Specification path | Main agent during planning | File containing agreed behavior, acceptance criteria, and slices |
| Progress log path | Main agent during planning | File tracking completed and unfinished work |
| Authorized slice ID | Agent proposes; you authorize | A stable identifier such as S01, assigned in the specification |
| Workspace and snapshot identity | Main agent before delegation | Actual checkout and exact code state being checked |
| Scope | Main agent | Relevant behavior, modules, or files for that assignment |
| Acceptance criteria and constraints | Main agent from specification | Criterion IDs plus expected behavior and constraints |
| Commands/scenarios | Main agent from repository and validation plan | Real Gradle tasks or reproducible device steps; never guessed task names |
| Environment/variant/device | Main agent from inspection | Actual build variant, toolchain, and available device/API; mark unavailable items |
| Evidence output location | Main agent | Existing test reports or a chosen local evidence directory |
| Base and reviewed snapshot | Main agent | The before/after code states defining the review diff |
| Every checkpoint field | Main agent throughout execution | Factual status and evidence, not a form for you to complete |

For example, planning an offline bookmarks feature might produce specification `docs/epics/offline-bookmarks/spec.md`, progress log `docs/epics/offline-bookmarks/progress.md`, slice `S01`, and criterion `AC-01: saved bookmarks remain available after relaunch while offline`. These are illustrative names, not existing files or prescribed module names. The agent chooses the repository's established location and returns actual paths.

A commit ID alone does not identify uncommitted edits. The main agent must record those too, using a stable captured diff/content identity or isolated snapshot, and keep reviewed files unchanged while the check runs. A special commit is not required simply to fill in this field. Missing devices, checks, or evidence must be recorded as unavailable or not run, never invented.

## Model allocation

| Work | Model and reasoning | Use |
|---|---|---|
| Main EPIC agent | Astra / low (default) or Sol / medium | Design, implementation, integration, and decisions within the authorized slice. Select the main model in the app; do not specify it in prompts. |
| Mechanical verification | Luna / low | Independent, bounded test execution, build/lint checks, or evidence gathering when delegation has no material effect on the result. Run directly when delegation overhead is not worthwhile. |
| Bounded review | Terra / medium | Independent focused correctness review of a stable snapshot. |
| Complex review or investigation | Main agent, or Sol / medium when a separate bounded investigation is useful | Cross-module behavior, concurrency, lifecycle, persistence, or other judgment-heavy issues. |

Delegation is optional. Use Luna for fully independent mechanical tasks when the lower-cost assignment is unlikely to compromise result quality. Findings may still change the implementation. Use at most one subagent at a time for this workflow, and provide the exact scope, stable snapshot, commands, and output required. Specify both model and reasoning explicitly. Avoid concurrent edits to the reviewed snapshot, competing Gradle builds, or shared emulator use.

## Workflow

1. During initial EPIC planning, inspect repository instructions, architecture, build variants, dependencies, tests, and CI commands. On later slices, revisit only relevant information or changed assumptions.
2. Create an implementation-ready EPIC specification with acceptance criteria, non-goals, risks, and dependency-ordered vertical slices.
3. Authorize one slice at a time. The main agent may make ordinary implementation decisions within that slice.
4. Complete the slice: implementation, meaningful tests, relevant validation, and fixes for confirmed issues.
5. Update the progress log incrementally, including before expensive work so an interruption leaves a usable checkpoint.
6. Stop at the slice boundary and ask the human to authorize the next slice. Never schedule automatic continuation.
7. At EPIC completion, map every acceptance criterion to evidence and record remaining risks or unverified behavior.

Use checks that match the changed behavior, including lifecycle recreation/process death, coroutine cancellation, offline/retry behavior, navigation, persistence, permissions, and accessibility where relevant. Do not claim skipped or unavailable checks passed. There is no guaranteed token saving from changing conversations or delegating work.

## Initial design prompt

```text
Design an implementation-ready plan for this Android EPIC:

EPIC: [name]
User problem and desired outcome: [description]
Acceptance criteria: [observable behaviors, if known]
Constraints and non-goals: [details]
Relevant modules, designs, or references: [paths/links]

Inspect the repository and applicable AGENTS.md instructions first. Discover the actual architecture, supported Android versions, build variants, dependency versions, test conventions, and CI commands. Use existing patterns unless there is a concrete reason to change them.

Create an EPIC specification and a concise progress/evidence log in the repository's established documentation location.

Assign stable slice IDs and fill in all technical fields from repository evidence. Return the actual absolute paths, the recommended first slice, and a ready-to-paste authorization prompt referencing this workflow. Do not leave the user to construct engineering task packets.

Include:
- Observable acceptance criteria with stable IDs.
- Scope, non-goals, assumptions, and unresolved decisions.
- Existing code paths and proposed changes.
- State ownership and important state transitions.
- API, persistence, compatibility, and migration implications.
- Relevant Android failure cases, omitting irrelevant items.
- Dependency-ordered vertical slices with behavior, modules, prerequisites, and verification.
- The highest-risk uncertainty and the smallest experiment to resolve it.

Prefer the simplest design that meets the requirements. Do not introduce generic frameworks or unrelated refactoring. Delegate only fully independent, bounded research or inspection when it adds value; use Luna/low for mechanical fact gathering and Terra/medium when tracing behavior requires judgment. Keep dependent architectural decisions with the main agent. Ask only questions whose answers materially change scope or behavior. Finish with an implementation-ready plan; do not implement the EPIC yet.
```

## Prompt 2 — complete one authorized slice

```text
Complete one Android EPIC slice.

Specification: [path]
Progress log: [path]
Authorized slice: [ID]

Outcome
Deliver the slice's acceptance criteria as working, integrated behavior. Completion includes implementation, relevant validation, independent review when warranted, and fixing confirmed issues caused by this change. A first implementation or successful compilation alone is not completion.

Authority within this slice
Proceed with ordinary local implementation decisions, source/test edits, and appropriate development checks without asking for approval at each step. Follow existing repository patterns and preserve unrelated changes. When a check fails, investigate and fix failures caused by this slice, then rerun affected checks. Ask only when a decision materially changes product behavior or scope, requires an external commitment, or exceeds the available authorization.

Continue until the slice is complete or a specific blocker prevents further useful work. Continue unaffected work while a question is pending. Do not stop merely to ask whether you should test, review, or fix your work.

Scope and efficiency
Read the specification, checkpoint, applicable repository instructions, and code needed for this slice. Choose validation based on changed behavior and repository requirements. Avoid redundant exploration, speculative refactoring, and repeated checks without a new reason. If the slice is clearly too large, record a smaller coherent subdivision and complete its first part; do not silently reduce acceptance criteria.

Delegation
Use tools directly for short checks. Delegate only bounded, fully independent work that adds enough value to justify its overhead, using at most one subagent at a time. Select Luna/low for mechanical verification and Terra/medium for ordinary focused review. Keep difficult integration reasoning with the main agent. Review and act on findings before declaring completion.

Specify both model and reasoning. Give each subagent the exact scope and stable snapshot; avoid editing that snapshot during verification or review. Avoid competing Gradle builds or shared emulator use.

Checkpoint
Update the progress log at meaningful milestones and before expensive work. Record status, changed files, decisions, validation evidence, remaining issues, and the next concrete action. If usage is interrupted, leave an accurate partial checkpoint and never describe incomplete validation as passed.

Stopping boundary
Stop after this slice is complete. Starting another slice requires my explicit authorization. Resuming unfinished work in this authorized slice does not require another scope approval. Do not begin the next slice or schedule automatic continuation.

If genuinely blocked, record the blocker and finish unaffected work. If a skill or repository instruction causes an otherwise unnecessary pause, identify its file and exact instruction, explain the conflict, and resolve it using the applicable instruction hierarchy.

Final response
Report complete/partial/blocked status, delivered behavior, validation evidence and gaps, proposed next slice, a same/new conversation recommendation, and a copyable continuation prompt.

Fill the continuation prompt with actual absolute workflow/specification/progress paths and the actual slice ID. If this slice is incomplete, the prompt must resume it; if complete, propose authorization for the next slice. Populate subagent task packets and checkpoint fields yourself from evidence.
```

## Independent verification prompt

Agent-facing template: the main agent fills and sends this; the user does not need to paste it.

Use Luna / low only for a bounded task where delegation has no material effect on the result.

```text
Independently verify this bounded Android change.

Workspace and immutable snapshot: [path and identity]
Scope: [module/feature]
Acceptance criteria: [IDs and expected behavior]
Exact commands/scenarios: [commands and device steps]
Environment/variant/device: [details]
Evidence output location: [path]

Do not modify source, dependencies, configuration, or tests. Build/test artifacts are allowed. Confirm the snapshot, run the assigned checks, and distinguish product, environment, and flaky/unclear failures. Do not infer behavioral correctness from compilation alone or treat skipped tests as success.

Return the snapshot, commands/scenarios executed, pass/fail/blocked per criterion, concise evidence and artifact paths, and unverified items with reasons. If diagnosis requires architectural reasoning, return the evidence and precise question without expanding the task.
```

## Independent review prompt

Agent-facing template: the main agent fills and sends this; the user does not need to paste it.

Use Terra / medium for an ordinary bounded review; use the main agent or Sol / medium for complex review.

```text
Review this Android change independently.

Workspace: [path]
Base and reviewed snapshot: [identities]
Scope: [files/modules/behavior]
Acceptance criteria and constraints: [details]

Read the diff and enough surrounding code to trace affected behavior. Do not modify files. Prioritize concrete correctness issues and regressions, including lifecycle restoration, coroutine cancellation, Flow collection, concurrent updates, persistence, retry behavior, navigation, and resource cleanup where relevant.

Report only actionable findings supported by code: severity, file and line, trigger and user-visible consequence, evidence or minimal reproduction, and correction direction. Separate confirmed findings from questions. Avoid style-only comments and speculative redesigns. If none are found, state that and identify review limits. Return the exact snapshot reviewed.
```

## Incremental checkpoint template

Agent-maintained record in the progress log, not a user prompt or user-filled form.

```text
EPIC: [name]
Current slice: [ID]
Status: not started | in progress | complete | blocked
Working tree / branch / commit: [identity]
Changed files: [paths]
Decisions: [short rationale]
Acceptance evidence: [criterion ID -> command, test, or scenario]
Checks: [passed, failed, blocked, not run; exact commands]
Remaining issues or uncertainty: [details]
Next concrete action: [action and dependency]
```

## Conversation guidance

Resume the same conversation when a slice is unfinished, especially after an interruption, because it preserves recent reasoning and failures. Start a new conversation at a clean slice boundary when the next slice is independent or the existing context contains substantial stale or abandoned material. A usage reset alone does not require a new conversation.

Resume prompt:

```text
Resume the current slice only. Read the latest progress log and inspect the working tree before making changes. Reconcile any interruption, continue from the first unfinished action, and avoid repeating completed checks unless the code or environment changed. Stop at the slice boundary.
```

Next-slice prompt:

```text
Execute slice [ID] from [EPIC specification path], using [progress/evidence log path] as the handoff. Read applicable repository instructions and verify the recorded state against the working tree. Follow the one-slice workflow: implement, validate, update the checkpoint, recommend the next conversation choice, and stop for my decision.
```

## Optional one-time instruction audit

```text
Audit this repository's AGENTS.md files and relevant development skills for redundant instructions, overly broad skill triggers, unnecessary mandatory reading/testing, and ambiguous approval or stopping rules.

Our intended boundary is:
- Independently complete implementation, relevant validation, and fixes within one authorized slice.
- Stop before starting the next slice.
- Ask about material scope/product decisions or actions outside authority.

Propose a minimal patch. Preserve concrete project constraints and required checks. Make document references conditional on the work being performed. Do not assert that tests or scripts are safe without inspecting them. Identify each removed or revised rule and the behavior it would improve.
```

This workflow does not change global model configuration, promise automatic continuation, or guarantee that an EPIC slice fits within available credits.

## Sources

- [Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra)
- [Android architecture](https://developer.android.com/topic/architecture)
- [Android testing strategies](https://developer.android.com/training/testing/fundamentals/strategies?hl=en)
- [OpenAI subagent guidance](https://learn.chatgpt.com/docs/agent-configuration/subagents)
