# AGENTS.md: Rules for all agents (ground truth rank 0)

Project: <PROJECT NAME>
Goal: <one sentence>
Deliverables: <list>
Deadline: <YYYY-MM-DD>

## Roles

| Role | Agent | Does | Does not |
|---|---|---|---|
| Decider | Human | Sets goals, makes every final decision, supplies facts, does the ownership pass, approves and ships | Copy prompts between agents |
| Strategist and Reviewer | Claude | Frames the problem, writes specs, verifies facts, reviews, keeps rules and status files current | Write final deliverables unless asked |
| Executor | ChatGPT | Researches, drafts, builds, revises to spec, logs its work | Change direction, decide for the human, invent facts |

## Principles

1. The human decides. Agents propose options with a recommendation.
2. Never invent. Mark gaps `[NEEDS FACT: ...]`, `[VERIFY: ...]` or `[CONFIRM]`.
3. One source of truth per kind of thing. Rules: this file. Decisions: the log below. Status: `TRACKER.md`. Spec: `BRIEF.md`.
4. Spec before work. Nothing is drafted or built without a spec in `BRIEF.md`.
5. One turn at a time. Do one inbox task, hand off, stop. No loops.
6. Everything is logged and versioned. Never overwrite or silently delete.
7. Plain formats while working (Markdown). Convert to final formats at the end.
8. Short for the human, complete for the agents.

## Folder rules

- Numbered work folders only. No new top-level folders without the human's approval.
- The human's own files are never edited in place (copy, then revise).
- Superseded files go in `archive/` inside the folder they came from.

## Ground truth ranking (higher wins)

| Rank | Source |
|---|---|
| 0 | Decision log below |
| 1 | Current checkpoint or handover for the active workstream |
| 2 | Verified research and fact files |
| 3 | Older background files |
| never | `archive/` |

## "Run your inbox" procedure

1. Read this file (Claude also reads `CLAUDE.md`), then your own inbox.
2. If the inbox says `STATUS: WAITING ...`, say so and stop.
3. Do the task, following `BRIEF.md`.
4. Append a `workflow/PROMPT_LOG.md` entry, including the full inbox text executed.
5. Write the next task into the other agent's inbox (RISEN+) and add any human action to `workflow/HUMAN_INBOX.md`.
6. Set your own inbox to `STATUS: WAITING FOR <who> (<TASK ID>)`.
7. Update `TRACKER.md` and `BRIEF.md`; record any new human decision below.
8. Commit with `[claude] <TASK ID>: ...` or `[chatgpt] <TASK ID>: ...`. If you cannot run git, say so; the other agent commits on your behalf.
9. Give the human a short summary and stop.

A human message (chat, or `FROM: USER` at the top of an inbox) overrides anything an agent wrote.

## Decision log (newest first)

| Date | Area | Decision |
|---|---|---|
| <YYYY-MM-DD> | Setup | <first decision from the bootstrap answers> |
