<!-- TEMPLATE — instructions.md
The rules every session follows, in chat and in Claude Code. Personalize the
two [CUSTOMIZE] spots, then delete this comment. -->

# Project Instructions

These instructions govern every Claude session for this project. Read fully before starting work.

If you were dispatched as a subagent for one specific task, skip the checklists and the rules about asking the user: do your task and report back.

To design a new feature or part of the project, or to write an inbox packet, use the relay skill.

## File locations

In the code repo, the reference files live in the `workflow/` folder: `instructions.md`, `roadmap.md`, `journal.md`, `journal-archive.md`, `index.md`, `decisions.md`, `decisions-archive.md`, `costs.md`, `inbox.md`, and `superpowers.md` if the project uses superpowers. `CLAUDE.md` stays at the repo root because it is the only file Claude Code loads automatically.

The Claude Project holds only the files chat needs: `CLAUDE.md`, the `workflow/` files except the two archives, and any specification the project is built from. Chat sees any other file (code, reports, archives) only when the user uploads it. If a chat needs one, ask for it by path.

If `superpowers.md` exists, follow it too. Where it and these instructions cover the same situation, `superpowers.md` wins.

---

## Session Start Checklist (mandatory, in order)

1. Read the "Project so far" summary and the last entry of `journal.md`, and find its "NEXT SESSION STARTS" line. Read older entries or `journal-archive.md` only if you need history they hold
2. Read `roadmap.md` — confirm the next uncrossed step matches the journal cursor
3. Check the Pending Items in `inbox.md` for items that affect this step
4. If the step involves an architectural choice, read the "Current rules" summary at the top of `decisions.md`; open a full entry only when you need its reasoning (search for its heading in `decisions.md` or `decisions-archive.md`, then read that range)
5. Read `index.md` and pick the files this step will read or change; open only those
6. Confirm with the user: "Resuming at [Phase X, Step Y: description]. Proceed?" If the step has a model tag, add: "This step: [Model]. Switch with `/model [model]` (in chat, the model picker) if you haven't." If work turns out harder than tagged, say so and suggest switching up
7. Do not begin work until the user confirms

---

## Session End Checklist (mandatory, in order)

Every finished roadmap step ends the session. As soon as the user approves a step, run this checklist, even if time or tokens are left. Never start the next step in the same session.

1. Show evidence that the step works (a test run, a file opened, output checked) before marking it done
2. Update `index.md` for any new or changed files. If the step changed a specification file, update it in the same commit, including its Contents list if it has one
3. Update `roadmap.md` — mark completed steps `[x]` with the date, move the `← CURRENT` marker
4. Log any decisions in `decisions.md`, and update its "Current rules" summary if a decision adds, changes or replaces a rule. If `decisions.md` now holds more than 15 entries, roll it up (relay skill, `references/rollups.md`)
5. Log any API or service spend in `costs.md`
6. Append a new entry to `journal.md` (never edit past entries), including a "Files changed" line listing every project file created or edited, so the user knows what to re-upload to the Project. Keep the **Work** list to 8 lines or fewer; put long evidence (test output, row counts, review findings) in a report file and name it in the entry. If `journal.md` now holds more than 5 session entries, roll up: move all but the 3 newest, word for word and in order, to the end of `journal-archive.md`, and add one line per moved session to the "Project so far" summary
7. Commit the changes. Then bring the Claude Project up to date, using the first route that works:
   - If this session can write Claude Project files (a Projects tool is available), upload every file in "Files changed" that the Project holds, replacing the old copies. Say which files you updated
   - Else, if the Project syncs from GitHub, push the commit and tell the user: "Click **Sync now** in your Project's files." Ask before the first push; after a yes, push every session
   - Else, list the files the user needs to re-upload
8. Tell the user: "Step done and saved. Time for a new session: open a new Claude Code session (or type /clear) and say 'resume'. The journal carries where we left off." Then stop

---

## Token Efficiency Rules

- Before any task, read `index.md` first: it is the table of contents for the whole project. Pick the files the task needs from it and open only those
- Read large files (>500 lines) by line range when possible. A large reference file (a specification, a long design doc) starts with a Contents list of its headings: read that first, then only the sections you need
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
- `decisions.md`: append-only, except the "Current rules" summary at the top, which is edited when a decision adds, changes or replaces a rule, and the rollup, which moves old entries unchanged to `decisions-archive.md`
- `journal-archive.md`, `decisions-archive.md`: append-only; entries arrive only through the rollups
- `roadmap.md`: edit only to mark completion or move the CURRENT marker; new steps need user approval
- `index.md`: a snapshot of the current project, updated on every file change: one line per file (or per folder of similar files) with its path, purpose and main functions or components; no line numbers, dates or change history (those belong in the journal)
- `inbox.md`: Pending Items hold unapplied packets in full; once applied and committed, a packet becomes one line under "Recently Applied" (date, items, title); git history keeps the full text
- `costs.md`: append-only

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
- Applying a packet: before each item, confirm its anchor or old text exists exactly once. If it doesn't, stop and report; never guess. After the packet is applied and committed, replace it with one line under "Recently Applied"
- Chat reads code from the Project's copies or from files the user uploads. Before writing code from them, check the journal's "Files changed" lines and ask the user to sync or re-upload any file changed since
