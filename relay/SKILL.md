---
name: relay
description: Use when starting a new multi-session project, or when the user asks to set up project memory files, design a roadmap step before building it, write an inbox packet for Claude Code, or roll up the journal.
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
| Start a new project | **Set up** (below) |
| Work on an existing project | Follow `workflow/instructions.md`: Session Start, work, Session End |
| Plan a new feature or part of the project | **Design a step** (below), in chat |
| Hand chat work to Claude Code | **Write an inbox packet** (below) |
| Shrink a long journal | **Roll up the journal** (below) |

If superpowers skills are available in this session, follow
`workflow/superpowers.md`; Claude Code already loads it through `CLAUDE.md`.
If the project doesn't have that file yet, read `references/superpowers.md`
and offer to add it (setup step 4).

## Set up a new project

1. Get a project name, what the user wants to build, rough phases, and
   whether they're comfortable with code. Invite the idea in their own
   words, any length: a paragraph or a few related ideas is fine. Write the
   one-line goal yourself from what they say, and let them correct it. If
   their message already covers something, don't ask again. Otherwise ask
   one question at a time. Also ask what matters for the plan: constraints,
   deadlines, what's already built, what "done" looks like.
2. When your questions are answered, summarize what you heard and ask: "Ready
   for me to draft the roadmap?" Draft only after a yes.
3. Draft the roadmap: phases that each end in something usable, cheap and
   simple versions first. Show it and adjust until the user approves. Put
   exactly one `← CURRENT` marker on the first step. Unless the project uses
   superpowers (it picks its own models), tag each code step with a model:
   - `(Haiku)`: exceedingly simple: a text or style tweak, one small file
   - `(Sonnet)`: most code steps: a feature, several files, routine fixes
   - `(Opus)`: the most ambitious: architecture, a new system, hard debugging
   When unsure, pick the higher one. Non-code steps get no tag.
4. If this is running in Claude Code, use the "With Claude Code" set.
   Otherwise ask whether they will use Claude Code or chat only, and if
   Claude Code, whether they use superpowers. Then create the files from
   `templates/`:
   - **Chat only:** `instructions.md`, `roadmap.md`, `journal.md`,
     `decisions.md`. In `instructions.md`, remove the "File locations" and
     "Chat and Claude Code" sections and every checklist line that mentions
     `index.md`, `costs.md` or `inbox.md`, then renumber. Create
     `journal-archive.md` at the first rollup.
   - **With Claude Code:** all 9 files. `CLAUDE.md` goes at the project root,
     everything else in a `workflow/` folder.
   - **With superpowers** (installed, or the user plans to use it): also copy
     `references/superpowers.md` to `workflow/superpowers.md`, and add
     `@workflow/superpowers.md` to `CLAUDE.md` on its own line, right after
     the instructions import. This loads the rules every session and gives
     them the priority superpowers grants to project instructions.
   - Fill in the `[CUSTOMIZE]` spots; delete the `<!-- TEMPLATE -->` comments.
     Set Working Style from the code-comfort answer: keep the teaching
     bullet for beginners; for experienced users, replace it with "The user
     is an experienced developer: skip basics, explain only non-obvious
     choices."
5. Write a Session 0 entry in `journal.md` pointing at the first step.
6. In chat, deliver the files:
   - **Chat only:** deliver each file separately for download (Projects
     can't upload a zip).
   - **With Claude Code:** don't hand out 9 files. Deliver one `inbox.md`
     holding a setup packet (format below). The user puts it in the empty
     project folder, opens Claude Code there, and says "Apply inbox.md".
     Claude Code builds everything from this skill's templates, so the
     skill must be available there (same Claude account).

   Then tell the user:
   - Add the files to the Project's files (with Claude Code: after it has
     applied the packet, upload the files it created in `workflow/`).
   - If the Project instructions aren't set yet, paste: "If instructions.md
     is in this Project, read it and follow its Session Start Checklist
     before any work. If it isn't, set up the project with the
     relay skill."
   - Start a new chat to begin Session 1.
   - Chat can't save Project files. At each session end, re-upload the
     changed files and remove the old copies.

   Setup packet format (everything decided in chat, in full):

   ```
   # Inbox

   ## Pending Items

   ### Item 1 (NEW) — Set up project files
   **Apply to**: this folder
   **Action**: Run relay setup in Claude Code mode (step 7) with
   the details below. Put this inbox.md into workflow/ with this item moved
   to "Recently Applied" and dated.
   **Project name**: ...
   **Goal**: ...
   **Superpowers**: <yes or no>
   **Push-back exception**: <exploratory work, or "none">
   **Coding experience**: <beginner or experienced>
   **Working style changes**: <other trims to the defaults, or "none">
   **roadmap.md content**:
   <the full approved roadmap, one ← CURRENT marker>
   **Session 0 journal note**: Planned in Claude chat on <date>; files
   created in Claude Code.
   **Expected**: CLAUDE.md at the root starting with
   @workflow/instructions.md (then @workflow/superpowers.md if Superpowers
   is yes); workflow/ holds instructions, roadmap, journal, journal-archive,
   index, decisions, costs and inbox (plus superpowers.md if yes).
   ```
7. In Claude Code, write the files directly into the project folder:
   - If a setup packet in `inbox.md` is being applied, take the name, goal,
     roadmap, customizations and journal note from it; don't ask again.
     Check its Expected line when done.
   - Create `workflow/` if missing. Create only the files that don't exist
     yet; never overwrite an existing workflow file.
   - If `CLAUDE.md` already exists, add `@workflow/instructions.md` as its
     first line on its own and keep everything else. Otherwise create it
     from the template.
   - If the folder isn't a git repo, offer to run `git init`. Then offer a
     first commit of the new files.
   - Tell the user to start a new Claude Code session: `CLAUDE.md` is read
     at session start, so the rules load from the next session on.
   - If they also plan in Claude chat, tell them to upload the `workflow/`
     files to a Project and paste the instructions line from step 6.

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

Before writing code or anchors from the Project's copy of a file, check the
recent journal entries' "Files changed" lines. If a file you'll touch was
changed since the user last uploaded it, ask them to re-upload it first.

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
checks that the anchor or old text exists exactly; if not, it stops and
reports instead of guessing. It checks each Expected line, moves the items
to "Recently Applied" with the date, then commits.

## Roll up the journal

When `journal.md` holds more than 5 session entries:

1. Move all but the 3 newest entries, word for word and in order, to the end
   of `workflow/journal-archive.md` (create it from the template if missing).
2. Add one line per moved session to the "Project so far" summary at the top
   of `journal.md`.
3. Check: every entry heading appears exactly once across the two files.

Nothing is deleted, so the journal stays append-only.

## Red flags

| Thought | Reality |
|---|---|
| "Short session, I'll skip the end checklist" | The next session starts from a stale cursor. The end checklist is the price of closing. |
| "I remember where we left off" | Read the journal. Your memory is a summary; the file is the record. |
| "It's done" (without showing it working) | Show the evidence before marking a step `[x]`. |
| "The chat plan is clear enough to paste loosely" | Vague packets get applied wrong. Exact content, plus an Expected line. |
| "This small extra task is fine to slip in" | Scope guard: ask whether to finish, add, or replace the current step. |
