<!-- TEMPLATE — instructions.md
The rules every session follows, in chat and in Claude Code. Personalize the
two [CUSTOMIZE] spots, then delete this comment. -->

# Project Instructions

These instructions govern every Claude session for this project. Read fully before starting work.

If you were dispatched as a subagent for one specific task, skip the checklists and the rules about asking the user: do your task and report back.

To design a new feature or part of the project, or to write an inbox packet, use the relay skill.

## File locations

In the code repo, the reference files live in the `workflow/` folder: `instructions.md`, `roadmap.md`, `journal.md`, `journal-archive.md`, `index.md`, `decisions.md`, `costs.md`, `inbox.md`, and `superpowers.md` if the project uses superpowers. `CLAUDE.md` stays at the repo root because it is the only file Claude Code loads automatically. In Claude chat, the same files appear in the Project by name.

If `superpowers.md` exists, follow it too. Where it and these instructions cover the same situation, `superpowers.md` wins.

---

## Session Start Checklist (mandatory, in order)

1. Read `journal.md` — find the last entry and its "NEXT SESSION STARTS" line. Do not read `journal-archive.md` unless you need history the summary doesn't cover
2. Read `roadmap.md` — confirm the next uncrossed step matches the journal cursor
3. Check `inbox.md` for unapplied items that affect this step
4. Read `decisions.md` if the step involves an architectural choice
5. Read the relevant `index.md` sections for files that will be touched
6. Confirm with the user: "Resuming at [Phase X, Step Y: description]. Proceed?" If the step has a model tag, add: "This step: [Model]. Switch with `/model [model]` (in chat, the model picker) if you haven't." If work turns out harder than tagged, say so and suggest switching up
7. Do not begin work until the user confirms

---

## Session End Checklist (mandatory, in order)

1. Show evidence that the step works (a test run, a file opened, output checked) before marking it done
2. Update `index.md` for any new or changed files
3. Update `roadmap.md` — mark completed steps `[x]` with the date, move the `← CURRENT` marker
4. Log any decisions in `decisions.md`
5. Log any API or service spend in `costs.md`
6. Append a new entry to `journal.md` (never edit past entries), including a "Files changed" line listing every project file created or edited, so the user knows what to re-upload to the Project. If `journal.md` now holds more than 5 session entries, roll up: move all but the 3 newest, word for word and in order, to the end of `journal-archive.md`, and add one line per moved session to the "Project so far" summary
7. Suggest a git commit message

---

## Token Efficiency Rules

- Read only the files the current step needs; use `index.md` to find them
- Read large files (>500 lines) by line range when possible
- Never re-read files already in context
- Never re-debate decisions in `decisions.md` — reference them and move on
- If unsure whether a file is needed, ask rather than reading speculatively

---

## Scope Guard Rule

If a request would expand work beyond the current roadmap step, stop and ask:
- (a) Finish the current step first, then do this?
- (b) Add this as a new roadmap step?
- (c) Replace the current step with this?

Never silently expand scope. The roadmap is the source of truth.

---

## Push-Back Rule

If the user asks to skip a roadmap step, push back: explain what depends on it and recommend the proper order.

<!-- [CUSTOMIZE] Name any exploratory work (content, copy, research) that may be
done out of order, or delete this exception. -->
**Exception**: [exploratory work, e.g. "content and copy development"] is iterative. Skipping ahead and doubling back is fine there. Note the deviation in the journal, but don't push back.

---

## File Update Rules

- `journal.md`: append-only; corrections go in a new entry. The only exceptions come from the rollup: old entries move unchanged to `journal-archive.md`, and lines are added to the "Project so far" summary
- `journal-archive.md`: append-only; entries arrive only through the rollup
- `roadmap.md`: edit only to mark completion or move the CURRENT marker; new steps need user approval
- `index.md`: update on every file change; file path + identifier, no line numbers
- `decisions.md`, `costs.md`: append-only

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

- Plan in Claude chat; execute in Claude Code
- If a big design discussion starts in Claude Code, suggest moving it to chat
- Chat cannot change project files. It writes exact change instructions ("apply packets") into `inbox.md`, each with an Expected line, and Claude Code applies them
- Applying a packet: before each item, confirm its anchor or old text exists exactly. If it doesn't, stop and report; never guess
- Chat reads code from the Project's uploaded copies. Before writing code from them, check the journal's "Files changed" lines and ask the user to re-upload any file changed since
