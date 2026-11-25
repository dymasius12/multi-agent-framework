# Multi-Agent Setup: Human + Claude + ChatGPT

A reusable working framework for any project (writing, research, coding, planning).
Give this one file to any AI agent and it can set up, or join, the framework.

> **Agent reading this:** if the project already has `AGENTS.md` and `07_workflow/` (or the equivalent `workflow/` folder), the framework is **already running**. Follow `AGENTS.md`; this file is the template. If not, run the Bootstrap in section 12.

---

## 0. In one minute (for the human)

- **You decide. Claude thinks and reviews. ChatGPT executes.**
- To move work forward, say **"Run your inbox"** to one agent at a time.
- **Your to do list** is one short file: `workflow/HUMAN_INBOX.md`.
- **Your decisions** go in one place: the decision log in `AGENTS.md`.
- **Everything is traceable**: `workflow/PROMPT_LOG.md` and `git log`.

---

## 1. Roles

| Role | Default agent | Does | Does not |
|---|---|---|---|
| **Decider** | Human | Sets goals, makes every final decision, supplies facts, does the "voice" or ownership pass, approves and ships | Copy prompts between agents |
| **Strategist and Reviewer** | Claude | Frames the problem, writes specs, verifies facts and fit, reviews rigorously, keeps rules and status files current | Write final deliverables unless the human asks |
| **Executor** | ChatGPT (Codex or desktop with folder access) | Researches, drafts, builds, revises to spec, logs its work | Change direction, decide for the human, or invent facts |

Roles can be swapped between models. The structure stays the same.

## 2. Principles

1. **The human decides.** Agents propose options with a recommendation; they never decide on the human's behalf.
2. **Never invent.** Facts come only from verified sources. Gaps are marked `[NEEDS FACT: ...]`, `[VERIFY: ...]` or `[CONFIRM]`, never guessed.
3. **One source of truth per kind of thing.** Rules: `AGENTS.md`. Decisions: the decision log. Status: `TRACKER.md`. Current spec: `BRIEF.md`. No duplicate files or folders.
4. **Spec before work.** Nothing is drafted or built without a spec in `BRIEF.md`.
5. **One turn at a time.** An agent does one inbox task, hands off, and stops. No loops.
6. **Everything is logged and versioned.** Every turn is logged; every draft is a new version; nothing is overwritten or silently deleted.
7. **Plain, readable formats while working** (Markdown for documents). Convert to final formats (PDF, docx) only at the end.
8. **Short for the human, complete for the agents.** Human files are one line per item; agent prompts are full RISEN+.

## 3. File structure

```
README.md            Human compass: status, where truth is, how to run a round
AGENTS.md            Rules for all agents + decision log (ground truth rank 0)
CLAUDE.md            First line "@AGENTS.md", then the reviewer's extra rules
BRIEF.md             Active spec block + execution log + review; archive; queue
TRACKER.md           The only task board
workflow/
  CLAUDE_INBOX.md    Next task for Claude
  CHATGPT_INBOX.md   Next task for ChatGPT
  HUMAN_INBOX.md     Short to do list for the human
  PROMPT_LOG.md      Append only history of every turn
00_research/ 01_.../ Numbered work folders, named per project (see section 11)
archive/             Superseded or duplicate files (inside the folder they came from)
```

Rules: numbered folders only, no new top-level folders without the human's approval, and the human's own files are never edited in place (copy, then revise).

## 4. Ground truth and the decision log

**Ranking (higher wins when sources disagree):**

| Rank | Source |
|---|---|
| 0 | Decision log in `AGENTS.md` (the human's latest decisions) |
| 1 | The current checkpoint or handover for the active workstream |
| 2 | Verified research and fact files |
| 3 | Older background files |
| never | `archive/` |

**Decision log:** a dated table, newest first: `| Date | Area | Decision |`. When the human decides something in chat, the agent who heard it adds a row **in the same turn**.

## 5. The loop

```
Human: "Run your inbox" → Executor works → hands to Reviewer
Human: "Run your inbox" → Reviewer reviews → hands back (revise) or to Human (decide / own it)
Human: answers or does the ownership pass → next round
```

**What an agent does when told "Run your inbox":**
1. Read `AGENTS.md` (Claude also reads `CLAUDE.md`), then its own inbox.
2. If the inbox says `STATUS: WAITING ...`, say so and stop.
3. Do the task, following `BRIEF.md`.
4. Append a `PROMPT_LOG.md` entry, **including the full inbox text it executed**.
5. Write the next task into the **other** agent's inbox (RISEN+), and add any human action to `HUMAN_INBOX.md`.
6. Set its own inbox to `STATUS: WAITING FOR <who> (<TASK ID>)`.
7. Update `TRACKER.md` and the relevant `BRIEF.md` section; record any new human decision in the decision log.
8. Commit with the prefix `[claude]` or `[chatgpt] <TASK ID>: ...`. If it cannot run git, it says so, and the other agent commits on its behalf.
9. Give the human a short summary and stop.

**The human can step in anytime:** say it in chat, or write at the top of any inbox with `FROM: USER`. A human message overrides anything an agent wrote.

## 6. Agent prompt format: RISEN+

Every agent inbox uses this structure. RISEN (Role, Instructions, Steps, End goal, Narrowing) plus three parts agent handoffs need: Context, Inputs, and a Done checklist.

```
# <AGENT> INBOX
STATUS: READY FOR <AGENT>
FROM: <CLAUDE / CHATGPT / USER>
TASK ID: <ID>
WRITTEN: <YYYY-MM-DD HH:MM TZ>

## R: Role          who the agent is for this task (1 to 2 lines)
## C: Context       what just happened, why now, what is locked (point to the decision log)
## I: Instructions  the goal in one paragraph + the single most important thing
## Inputs           files to read, in order
## S: Steps         numbered, concrete actions
## E: End goal      exact output files, and what "good" looks like
## N: Narrowing     hard rules, limits, out of scope, what to mark [VERIFY]/[NEEDS FACT]
## Done when        checklist the agent ticks before stopping (outputs, checks, log, handoff, tracker, commit)
```

**Task IDs:** short and stable, one per round: `DOC-v1`, `DOC-v1-review`, `DOC-v2`, `FEAT-login`, `FIX-123`. The same ID goes in the inbox, the log, `BRIEF.md` and the commit.

## 7. Human inbox format (short)

```
# Your inbox
Deadline: <date>

Tick items with [x]; agents move them to Done.

## To do
- [ ] **<Verb> <thing>** in `<file>` (<time estimate>). <one short instruction>

## Waiting on others
- <Agent>: <what>

## Done
- [x] <item> (<date>)
```

Rules: one line per item, verb first, file path, time estimate. No background (that lives in `BRIEF.md` and the log). At most about 10 open items. Questions to the human are asked here or in chat, one decision each, with the agent's recommendation first.

## 8. Spec, execution and review (BRIEF.md)

One **active block** at a time, with six sections:

| Section | Written by | Contains |
|---|---|---|
| [Current Objective] | Claude | What and why, what the audience judges, sources to read first |
| [Target & Limits] | Claude | Output file, length or size limits, format |
| [Strategic Angle & Outline] | Claude | The spine, the structure with budgets, must have / must not have, open decisions for the human |
| [Constraints & Anti-Cliché] | Claude | Hard rules, banned phrases, verification rules |
| [Execution Log] | ChatGPT | What was done, counts, sources checked, open items, change log |
| [Reviewer Critique] | Claude | Verdict, ranked issues, single most important change |

Finished blocks move to `## Archive` (with their status); upcoming work sits in `## Queue`.

## 9. Review standard

- **Verdict:** ACCEPT / REVISE (light) / REVISE / RETHINK.
- **Ranked issues:** `[Blocking]`, `[Important]`, `[Nit]`. Each gives the quoted problem, why it matters to the real audience, and a fix direction (not a rewrite).
- **End with the single most important change.**
- **Verify independently:** recount words, recheck sources, rerun tests. Never trust the executor's numbers.
- **Separate substance from style.** Flag substance. Leave the human's own voice alone unless asked.
- **Human ownership pass:** before anything ships, the human reads it and makes it theirs (voice pass for writing; read through and run for code).

## 10. Logging, versions and tracing

**PROMPT_LOG entry (append only, newest at the bottom):**
```
### [YYYY-MM-DD HH:MM TZ] | Agent: CLAUDE / CHATGPT / USER | Task ID: <ID>
- **Prompt executed:** 1 to 2 sentences
- **Prompt text:** full inbox text in a <details> block
- **Key decisions / output:**
- **Files modified:**
- **Next handoff written to:**
- **Commit:**
```

**Versions:** `<document>_v<N>.md`, never overwritten. Every draft starts with a header: `Document / Version / Date / Exact count / Limit / Open items`.
**Commits:** `[claude] <ID>: ...`, `[chatgpt] <ID>: ...`, `[user] <ID>: ...`.
**Tracing:** read `PROMPT_LOG.md` top to bottom, or run `git log --oneline`.

## 11. Adapting to the task type

| Task type | Work folders (example) | Deliverable versions | "Done" means |
|---|---|---|---|
| Writing / applications | `00_research`, `01_drafts`, `02_final`, `03_submission` | `essay_v1.md`, `v2`... | Within limits, facts verified, human voice pass done |
| Research / analysis | `00_sources`, `01_notes`, `02_analysis`, `03_report` | `report_v1.md`... | Claims cited, citation log complete, human sign off |
| Coding | Project's own layout; agents work on branches | One branch or PR per task ID | Tests pass, reviewer checks the diff, human runs it |
| Planning / ops | `00_inputs`, `01_plans`, `02_decisions` | `plan_v1.md`... | Decisions logged, owners and dates set |

For coding, keep `BRIEF.md` as the spec and review place. The "version" is the branch or PR, the executor's Execution Log lists files changed and test results, and the reviewer verifies by running the tests.

## 12. Bootstrap (for an agent setting up a new project)

1. **Ask the human five questions** (one message, short):
   1. What is the goal, and what are the deliverables?
   2. What is the deadline?
   3. Which agents are in this project, and can each one read and write the folder and run git?
   4. Where are the existing materials and facts?
   5. Any hard rules (style, privacy, things to avoid)?
2. Look at the existing files before creating anything. Reuse what exists; do not duplicate.
3. Create the files in section 3 from the templates in sections 4 to 10, with project specific work folders (section 11).
4. Write the first decision log rows from the human's answers.
5. Write the first spec in `BRIEF.md`, the first RISEN+ task in the executor's inbox (including a "My understanding" check before it starts work), and `HUMAN_INBOX.md`.
6. Initialise git, commit as `[claude] SETUP: ...`, and tell the human the one sentence to say to the executor.

## 13. Lessons learned

- **Desktop agents may edit files but not commit.** The other agent commits on their behalf with their prefix, so the trail stays honest.
- **Do not create parallel folder structures.** Fit the framework into the human's existing layout.
- **Executors over prune.** "Unverified" is not the same as "invented": the human's own account of their own work needs confirmation (`[CONFIRM]`), not deletion.
- **Record decisions in the same turn.** Otherwise agents drift back to old assumptions.
- **Make continuity explicit.** Related documents should visibly continue one line of thought; say how they connect.
- **Wording can read as the opposite of what the human means.** Explain framing choices in plain words and confirm.
- **Keep the human's files short.** They will not read long ones, and then the loop stalls.
