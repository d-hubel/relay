<!-- TEMPLATE — instructions.md
The core rules every session follows, in chat and in Claude Code. Loaded into
every Claude Code session, so keep it short: for each line, ask "would removing
this cause mistakes?" Personalize the two [CUSTOMIZE] spots, then delete this
comment. -->

# Project Instructions

These rules govern every Claude session for this project. Read fully before starting work.

If you were dispatched as a subagent for one specific task, skip the checklists and the rules about asking the user: do your task and report back.

To design a new feature or part of the project, or to write an inbox packet, use the relay skill.

## File locations

The reference files live in `workflow/`: `instructions.md` (this file), `session-end.md`, `roadmap.md`, `journal.md`, `journal-archive.md`, `index.md`, `decisions.md`, `decisions-archive.md`, `costs.md`, `inbox.md`, and `superpowers.md` if the project uses superpowers. `CLAUDE.md` stays at the repo root because it is the only file Claude Code loads automatically.

The Claude Project holds only the files chat needs: `CLAUDE.md`, `instructions.md`, `roadmap.md`, `journal.md`, `index.md`, `decisions.md`, `costs.md`, `inbox.md`, and any specification the project is built from. If a chat needs another file, ask for it by path.

If `superpowers.md` exists, follow it too. Where it and these instructions cover the same situation, `superpowers.md` wins.

---

## Session Start Checklist (mandatory, in order)

1. Read the "Project so far" summary and the last entry of `journal.md`, and find its "NEXT SESSION STARTS" line. Read older entries or `journal-archive.md` only if you need history they hold
2. Read `roadmap.md` — confirm the next uncrossed step matches the journal cursor
3. Check the Pending Items in `inbox.md` for items that affect this step
4. Read `index.md` and pick the files this step will read or change; open only those
5. Read the "Current rules" summary at the top of `decisions.md`. Then search `decisions.md` for `**Touches**:` lines naming the files you picked (or their folders, or `all`) and read only those entries, including their Ruled out lines: don't retry what was already rejected. Search `decisions-archive.md` the same way only if you need older history
6. Confirm with the user: "Resuming at [Phase X, Step Y: description]. Proceed?" If the step has a model tag, add: "This step: [Model]. Switch with `/model [model]` (in chat, the model picker) if you haven't." If work turns out harder than tagged, say so and suggest switching up
7. Do not begin work until the user confirms

---

## During the step

- Every step has a check that runs and returns pass or fail (a test, a script, a command and its expected output). Run it and fix until it passes before calling the work done. Show the evidence, don't just say it works
- Investigations that would open more than a few files go to a subagent that returns a short summary with file paths, so the reading stays out of this session
- When a subagent reviews work, tell it to report only gaps that affect correctness or the stated requirements. Treat anything else it finds as optional
- If the same problem survives two attempted fixes, stop. Write what was tried and why it failed into the journal (and a Ruled out line if it rejects an approach), then end the session with `workflow/session-end.md`. A fresh session with a better plan beats a session full of failed attempts
- When the user approves the step, read `workflow/session-end.md` and follow it. Every finished step ends the session

---

## Token Efficiency Rules

- Before any task, read `index.md` first: it is the table of contents for the whole project. Pick the files the task needs from it and open only those
- Read large files (>500 lines) by line range when possible. A large reference file (a specification, a long design doc) starts with a Contents list of its headings: read that first, then only the sections you need
- Never re-read files already in context
- Never re-debate decisions in `decisions.md` — reference them and move on. Never retry an approach a decision lists under Ruled out without telling the user first
- If unsure whether a file is needed, ask rather than reading speculatively

---

## Scope Guard Rule

If a request would expand work beyond the current roadmap step, stop and ask:
- (a) Finish the current step first, then do this?
- (b) Add this as a new roadmap step?
- (c) Replace the current step with this?

Never silently expand scope. The roadmap is the source of truth.

If the user asks to skip a roadmap step, push back: explain what depends on it and recommend the proper order.

<!-- [CUSTOMIZE] Name any exploratory work (content, copy, research) that may be
done out of order, or delete this exception. -->
**Exception**: [exploratory work, e.g. "content and copy development"] is iterative. Skipping ahead and doubling back is fine there. Note the deviation in the journal, but don't push back.

---

## Working Style

<!-- [CUSTOMIZE] Set at setup from the user's coding experience. Experienced
     users: replace the first bullet with "The user is an experienced
     developer: skip basics, explain only non-obvious choices." -->
- The user is learning as the project goes: explain WHAT and WHY in plain language, define technical terms on first use, and comment code generously
- Default to the simplest solution that works; flag tradeoffs
- Never add a dependency without logging the decision
- Be concise; the user is often on mobile
- Ask one question at a time
- Planning: ask questions first, then confirm the user is ready, then make the plan. Never plan before that yes
- State assumptions so they can be corrected early

---

## Chat and Claude Code

- Plan in Claude chat; execute in Claude Code. If a big design discussion starts in Claude Code, suggest moving it to chat
- Chat cannot change project files. It writes exact change instructions ("apply packets") into `inbox.md`, each with an Expected line, and Claude Code applies them
- Applying a packet: before each item, confirm its anchor or old text exists exactly once. If it doesn't, stop and report; never guess. After the packet is applied and committed, replace it with one line under "Recently Applied"
- Chat reads code from the Project's copies or from files the user uploads. Before writing code from them, check the journal's "Files changed" lines and ask the user to sync or re-upload any file changed since
