---
name: relay
description: Use when starting a new multi-session project, or when the user asks to set up project memory files, design a roadmap step before building it, write an inbox packet for Claude Code, or roll up the journal or decisions log.
---

# Relay

A small set of plain markdown files that act as the project's memory, so every
session picks up exactly where the last one stopped. Claude chat plans; Claude
Code executes. Works for code and non-code projects.

**Core principle:** anything the next session needs lives in a file. Your
memory of this conversation does not survive; the files do.

## Pick the job

| The user wants to... | Do this |
|---|---|
| Start a new project | Read `references/setup.md` and follow it |
| Work on an existing project | Follow `workflow/instructions.md`: Session Start, one step, Session End |
| Plan a new feature or part of the project | **Design a step** (below), in chat |
| Hand chat work to Claude Code | **Write an inbox packet** (below) |
| Shrink a long journal or decisions log | Read `references/rollups.md` and follow it |

If superpowers skills are available in this session, follow
`workflow/superpowers.md`; Claude Code already loads it through `CLAUDE.md`.
If the project doesn't have that file yet, read `references/superpowers.md`
and offer to add it.

The project's own `workflow/instructions.md` is the authority. If it differs
from this skill's template, the user customized it: follow theirs.

## Design a step

Do this in chat for new features, new parts of the project, or anything that
changes how the pieces fit together. A small change to existing files can be
designed where it is built.

1. Size it and say which you chose: a quick test ("can we...?"), a small
   change, or a new build. When unsure, pick the bigger one. A quick test
   needs only the question and how you'll check it.
2. Ask questions before planning, one at a time: purpose, who it's for,
   what success looks like, constraints. Don't propose anything yet.
3. When your questions are answered, write back your understanding,
   separating what the user said from what you're assuming. Ask: "Anything
   to correct, or ready for me to plan?" Plan only after a yes.
4. For a new build, offer 2-3 approaches with tradeoffs and your
   recommendation. Cut anything not needed yet.
5. Present the design in short sections and get a yes on each. Keep pieces
   small, each with one job.
6. Write the approved design as a spec: goal, decisions, constraints, what's
   out of scope, how we'll know it works. For code, also: the main pieces
   and how they connect, and what should happen with missing, wrong or
   unexpected input. Check it for "TBD", contradictions, and anything that
   could be read two ways; fix them.
7. If the spec shows the step is harder or easier than its roadmap model tag,
   update the tag in the inbox packet.
8. Nothing gets built until the user approves the spec. Send it to Claude
   Code in an inbox packet.

Method adapted from superpowers' brainstorming skill.

## Write an inbox packet

Chat cannot change the user's files. When chat work needs to land in the
project, write items for `workflow/inbox.md`, and never say a file "was
updated".

Read `index.md` first and open only the files the packet touches. Before
writing code or anchors from the Project's copy of a file, check the recent
journal entries' "Files changed" lines. If a file you'll touch was changed
since the user last synced or uploaded it, ask them to sync or re-upload it
first. If a file isn't in the Project, ask for it by path.

```
### Item N (NEW | REPLACE | DELETE) — short title
**Apply to**: path/to/file
**Action**: create | append after "<anchor>" | replace "<old>" with this
**Content**:
<the exact content — no placeholders, no "TBD", no "similar to above">
**Expected**: <what the user or Claude Code should see after applying>
```

End every packet with the journal entry for the session and a suggested git
commit message. Claude Code applies items in order. Before each item it
checks that the anchor or old text exists exactly once; if not, it stops and
reports instead of guessing. It checks each Expected line, commits, then
replaces the packet with one line under "Recently Applied" (date, items,
title); git history keeps the full text.

If other packets are already pending, say in the first item which goes first.

## Red flags

| Thought | Reality |
|---|---|
| "Short session, I'll skip the end checklist" | The next session starts from a stale cursor. The end checklist is the price of closing. |
| "Step's done, I'll start the next one here" | One step per session. A long session re-reads everything on every message; a fresh one starts from the journal. |
| "I remember where we left off" | Read the journal. Your memory is a summary; the file is the record. |
| "I'll read the whole folder to be safe" | Read `index.md`, then only the files the step needs. |
| "It's done" (without showing it working) | Show the evidence before marking a step `[x]`. |
| "The chat plan is clear enough to paste loosely" | Vague packets get applied wrong. Exact content, plus an Expected line. |
| "This small extra task is fine to slip in" | Scope guard: ask whether to finish, add, or replace the current step. |
