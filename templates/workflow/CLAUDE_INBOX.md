# CLAUDE INBOX
STATUS: READY FOR CLAUDE
FROM: <CHATGPT / USER>
TASK ID: <ID>
WRITTEN: <YYYY-MM-DD HH:MM TZ>

## R: Role
<Who Claude is for this task, 1 to 2 lines.>

## C: Context
<What just happened, why now, what is locked. Point to the decision log.>

## I: Instructions
<The goal in one paragraph, plus the single most important thing.>

## Inputs
1. `AGENTS.md`
2. `CLAUDE.md`
3. `BRIEF.md`
4. <other files, in reading order>

## S: Steps
1. <concrete action>
2. <concrete action>

## E: End goal
<Exact output files, and what "good" looks like.>

## N: Narrowing
<Hard rules, limits, out of scope. What to mark [VERIFY] / [NEEDS FACT].>

## Done when
- [ ] Outputs written
- [ ] Independent checks run
- [ ] `workflow/PROMPT_LOG.md` entry appended (with full inbox text)
- [ ] Next task written to the other inbox; `HUMAN_INBOX.md` updated
- [ ] `TRACKER.md`, `BRIEF.md` and decision log updated
- [ ] Committed `[claude] <TASK ID>: ...`
- [ ] This inbox set to `STATUS: WAITING FOR <who> (<TASK ID>)`
