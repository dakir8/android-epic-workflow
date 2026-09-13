# Android EPIC: human guide

Use `$android-epic` to plan and deliver medium-to-hard Android EPICs one authorized slice at a time. You describe the feature and decide when to start each slice; the agent handles implementation, verification, and progress records.

## Which file should I use?

- [SKILL.md](skills/android-epic/SKILL.md) is the single source of agent execution rules. Invoke the installed skill instead of pasting those rules.
- This file explains how to use the skill. You do not need to give it to the agent.
- [android-epic-agent.md](android-epic-agent.md) is a compatibility pointer for older prompts.

The personal installation is at `~/.codex/skills/android-epic`. After changing the repository's `skills/android-epic` directory, sync it to that installation. On another machine, install the skill there or reference the actual absolute path to its `SKILL.md`.

## Start an EPIC

Open your Android project and select **Astra / low** or **Sol / medium** as the main model. Then send:

```text
Use $android-epic to plan an EPIC for [describe your feature].
Constraints: [known requirements, or none known].
Do not implement yet.
```

The agent creates the specification and progress log, assigns slice IDs, and returns an exact prompt to authorize the first slice. Review the plan and send that prompt when ready.

## Execute and continue

Mention `$android-epic` when starting the EPIC or a new conversation. For follow-ups on the same EPIC in the same conversation, you do not need to repeat its name or the workflow rules.

Authorize the first slice with:

```text
Execute the first recommended slice, then stop.
```

After a completed slice:

```text
Execute the next recommended slice, then stop.
```

The agent completes implementation, relevant validation, and fixes within that slice. You decide whether to start another. You do not need to paste a full implementation prompt each time.

If usage interrupts unfinished work, resume with:

```text
Resume the unfinished authorized slice, then stop.
```

Small slices and incremental checkpoints help with usage limits, but do not guarantee that a slice fits within your remaining allowance.

## Same conversation or a new one?

Continue in the same conversation for an unfinished slice or closely related work. Consider a new conversation after a completed slice when the next work is independent or the old context is stale. A usage reset alone does not require starting fresh.

For a new conversation, open the same Android project/worktree and paste the handoff the agent supplies. It contains the skill invocation, actual specification and progress-log paths, and the slice ID. You do not need to construct those fields. If the workspace moved, tell the agent so it can reconcile the recorded state.

## What do I fill in?

Only your feature description, known constraints, and the decision to proceed. The agent discovers technical details, prepares any subagent assignments, records checks and evidence, and maintains the progress log.

The skill contains the delegation preferences: Luna / low for useful independent mechanical verification and Terra / medium for ordinary focused review. Short checks run directly when delegation overhead is not worthwhile. The main agent owns integration.

## Background

The workflow's autonomy within a slice and explicit completion boundary were informed by [Rethinking skills and prompts for GPT-6 Astra](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra). Detailed execution instructions live only in the skill so the human guide and agent rules do not become competing prompt sets.
