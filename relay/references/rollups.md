# Rollups

Two files grow every session. Rollups keep them short without deleting
anything: old entries move word for word to an archive that sessions read
only when they need history. Session End runs both checks.

## Journal

When `journal.md` holds more than 5 session entries:

1. Move all but the 3 newest entries, word for word and in order, to the end
   of `workflow/journal-archive.md` (create it from the template if missing).
2. Add one line per moved session to the "Project so far" summary at the top
   of `journal.md`.
3. Check: every entry heading appears exactly once across the two files.

## Decisions

When `decisions.md` holds more than 15 entries:

1. Make sure the "Current rules" summary at the top is up to date: one line
   per rule in force, each ending `(see <entry heading>)`. Where a later
   entry replaced an earlier one, summarise only the later one.
2. Move all but the 10 newest entries, word for word and in order, to the
   end of `workflow/decisions-archive.md` (create it from the template if
   missing).
3. In the summary, change `(see ` to `(archive: ` on every line whose entry
   moved.
4. Check: every entry heading appears exactly once across the two files;
   every `(see ...)` line names a heading in `decisions.md` and every
   `(archive: ...)` line names one in `decisions-archive.md`.

Moved entries keep their Touches and Ruled out lines, so a search of the archive still finds them by file.

Nothing is deleted, so both logs stay append-only.
