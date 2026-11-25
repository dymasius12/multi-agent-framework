# CHATGPT INBOX
STATUS: READY FOR CHATGPT
FROM: <CLAUDE / USER>
TASK ID: <ID>
WRITTEN: <YYYY-MM-DD HH:MM TZ>

## R: Role
<Who ChatGPT is for this task, 1 to 2 lines.>

## C: Context
<What just happened, why now, what is locked. Point to the decision log.>

## My understanding (confirm before working)
<ChatGPT restates the task in 2 to 3 lines. If it is wrong or unclear, stop and put the question in HUMAN_INBOX.md.>

## I: Instructions
<The goal in one paragraph, plus the single most important thing.>

## Inputs
1. `AGENTS.md`
2. `BRIEF.md`
3. <other files, in reading order>

## S: Steps
1. <concrete action>
2. <concrete action>

## E: End goal
<Exact output files (new version, never overwrite), and what "good" looks like.>

## N: Narrowing
<Hard rules, limits, out of scope. Never invent facts: mark [VERIFY] / [NEEDS FACT] / [CONFIRM].>

## Done when
- [ ] Outputs written as a new version with the standard header
- [ ] Checks against `BRIEF.md` limits done
- [ ] `BRIEF.md` [Execution Log] filled
- [ ] `workflow/PROMPT_LOG.md` entry appended (with full inbox text)
- [ ] Next task written to Claude's inbox; `HUMAN_INBOX.md` updated
- [ ] `TRACKER.md` updated
- [ ] Committed `[chatgpt] <TASK ID>: ...` (or noted that Claude should commit)
- [ ] This inbox set to `STATUS: WAITING FOR <who> (<TASK ID>)`
