# Set up a new project

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
   exactly one `← CURRENT` marker on the first step. Size each step to fit
   one session. Unless the project uses superpowers (it picks its own
   models), tag each code step with a model:
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
     "Chat and Claude Code" sections and every checklist or rule line that
     mentions `index.md`, `costs.md` or `inbox.md`, then renumber. Create
     `journal-archive.md` and `decisions-archive.md` at their first rollup.
   - **With Claude Code:** all 10 files. `CLAUDE.md` goes at the project
     root, everything else in a `workflow/` folder.
   - **With superpowers** (installed, or the user plans to use it): also copy
     `references/superpowers.md` to `workflow/superpowers.md`, and add
     `@workflow/superpowers.md` to `CLAUDE.md` on its own line, right after
     the instructions import. This loads the rules every session and gives
     them the priority superpowers grants to project instructions.
   - Fill in the `[CUSTOMIZE]` spots; delete the `<!-- TEMPLATE -->` comments
     (they cost tokens in every session that reads the file).
     Set Working Style from the code-comfort answer: keep the teaching
     bullet for beginners; for experienced users, replace it with "The user
     is an experienced developer: skip basics, explain only non-obvious
     choices."
5. Write a Session 0 entry in `journal.md` pointing at the first step.
6. In chat, deliver the files:
   - **Chat only:** deliver each file separately for download (Projects
     can't upload a zip).
   - **With Claude Code:** don't hand out 10 files. Deliver one `inbox.md`
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
   - Chat can't save Project files. With Claude Code, keep the Project in
     sync the easy way: put the project on GitHub and add the repo to the
     Project's files (+ → GitHub). **Sync only the files chat needs**:
     `CLAUDE.md` and, in `workflow/`, `instructions.md`, `roadmap.md`,
     `journal.md`, `index.md`, `decisions.md`, `costs.md` and `inbox.md`,
     plus any specification the project is built from. Leave out code,
     tests, reports and both archive files: a whole-repo sync grows past
     the Project's limit and chat falls back to searching snippets, which
     costs more and misses things. Chat asks for any other file by path
     when it needs it. Each Code session ends by pushing, and one click on
     **Sync now** updates the Project. Code sessions attached to the Project
     update it directly. Otherwise, re-upload the changed files at each
     session end.

   Setup packet format (everything decided in chat, in full):

   ```
   # Inbox

   ## Pending Items

   ### Item 1 (NEW) — Set up project files
   **Apply to**: this folder
   **Action**: Run relay setup in Claude Code mode (step 7) with
   the details below. Put this inbox.md into workflow/ with this item
   replaced by one line under "Recently Applied" (date, item, title).
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
   index, decisions, decisions-archive, costs and inbox (plus
   superpowers.md if yes).
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
     first commit of the new files, and offer to connect a GitHub repo so
     the Claude Project can sync from it (only the files listed in step 6).
   - Tell the user to start a new Claude Code session: `CLAUDE.md` is read
     at session start, so the rules load from the next session on.
   - If they also plan in Claude chat, tell them to upload the `workflow/`
     files (not the archives) to a Project and paste the instructions line
     from step 6.
