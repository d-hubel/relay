# Session End

Read this when the user approves a step, or when a session has to stop early. It is not loaded at session start, to keep every session's rules short.

Every finished roadmap step ends the session. Run this checklist even if time or tokens are left. Never start the next step in the same session.

## Checklist (mandatory, in order)

1. Run the step's check one last time and show the evidence (the command and what it returned, a file opened, output checked) before marking it done. If the session is stopping early, skip to step 6 and say in the entry what is unfinished
2. Update `index.md` for any new or changed files. If the step changed a specification file, update it in the same commit, including its Contents list if it has one
3. Update `roadmap.md` — mark completed steps `[x]` with the date, move the `← CURRENT` marker
4. Log any decisions in `decisions.md`, each with a Touches line (the files it governs) and a Ruled out line (what was rejected and why), and update its "Current rules" summary if a decision adds, changes or replaces a rule. If `decisions.md` now holds more than 15 entries, roll it up (below)
5. Log any API or service spend in `costs.md`
6. Append a new entry to `journal.md` (never edit past entries), including a "Files changed" line listing every project file created or edited, so the user knows what to re-upload to the Project. Keep the **Work** list to 8 lines or fewer; put long evidence (test output, row counts, review findings) in a report file and name it in the entry. If `journal.md` now holds more than 5 session entries, roll it up (below)
7. Commit the changes. Then bring the Claude Project up to date, using the first route that works:
   - If this session can write Claude Project files (a Projects tool is available), upload every file in "Files changed" that the Project holds, replacing the old copies. Say which files you updated
   - Else, if the Project syncs from GitHub, push the commit and tell the user: "Click **Sync now** in your Project's files." Ask before the first push; after a yes, push every session
   - Else, list the files the user needs to re-upload
8. Tell the user: "Step done and saved. Time for a new session: open a new Claude Code session (or type /clear) and say 'resume'. The journal carries where we left off." Then stop

## Rollups

- **Journal** (more than 5 session entries): move all but the 3 newest, word for word and in order, to the end of `journal-archive.md`; add one line per moved session to the "Project so far" summary; check every entry heading appears exactly once across the two files
- **Decisions** (more than 15 entries): bring the "Current rules" summary up to date; move all but the 10 newest, word for word and in order, to the end of `decisions-archive.md`; change `(see ` to `(archive: ` on summary lines whose entry moved; check every entry heading appears exactly once across the two files

## File update rules

- `journal.md`: append-only; corrections go in a new entry. The only exceptions come from the rollup: old entries move unchanged to `journal-archive.md`, and lines are added to the "Project so far" summary
- `decisions.md`: append-only, except the "Current rules" summary at the top, which is edited when a decision adds, changes or replaces a rule, and the rollup, which moves old entries unchanged to `decisions-archive.md`
- `journal-archive.md`, `decisions-archive.md`: append-only; entries arrive only through the rollups
- `roadmap.md`: edit only to mark completion or move the CURRENT marker; new steps need user approval
- `index.md`: a snapshot of the current project: one line per file (or per folder of similar files) with its path, purpose and main functions or components; no line numbers, dates or change history (those belong in the journal)
- `inbox.md`: Pending Items hold unapplied packets in full; once applied and committed, a packet becomes one line under "Recently Applied" (date, items, title); git history keeps the full text
- `costs.md`: append-only
