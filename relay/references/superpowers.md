# Using this workflow with superpowers

This workflow runs the project: roadmap, journal, decisions, inbox.
Superpowers runs one code step: spec, plan, build, review.

1. **One go-ahead per session.** If the current step has an approved spec,
   the Session Start confirmation names it: "Resuming at [step], building
   from the approved spec <path>. Proceed?" That yes also counts as
   superpowers' spec approval; go straight to writing-plans.

2. **Inside an approved plan, decide instead of asking.** While superpowers
   runs a plan, any rule in `instructions.md` that says to stop or ask the
   user (scope guard, push-back, asking before reading a file or adding a
   dependency) becomes: decide, record it as a Ruling, keep going. Only
   superpowers' own four stops pause the run. The rules apply again when
   choosing the next step.

3. **Design in chat, plan and build in Code.** Chat designs new builds with
   the relay skill's "Design a step". The inbox packet saves the
   approved spec to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`
   with the line "Status: approved by the user in chat on <date>". In Code,
   brainstorming resumes at its earliest unfinished stage, writing-plans,
   and doesn't re-ask what the spec answers. Exception: a small change to
   existing code may use superpowers' short design in Code, since it can
   read the files.

4. **Code-step packets carry a spec, not code.** Superpowers writes the
   failing test first and discards code written before it. Describe what to
   build, the constraints, and how to know it works. Packets for non-code
   files still carry exact content.

5. **Non-code work skips superpowers.** Content, documents and planning steps
   use this workflow alone.

6. **Explanations come at the end.** Superpowers keeps narration to a line
   between steps. Put teaching explanations in code comments and in the
   end-of-run summary, in plain language.

7. **Record it before superpowers deletes it.** At Session End:
   - copy each "Rulings I made" item into `decisions.md`
   - in the journal, name the spec and plan paths and any "Deferred minors"
   - mark the step `[x]` only after the final review is clean and tests pass
   - if the plan isn't finished, don't mark the step; set NEXT SESSION
     STARTS to "Resume plan <path>". Superpowers resumes from its own
     progress file.

8. **Superpowers builds in a worktree; workflow files stay on main.** Using an
   isolated worktree is the standing preference, so superpowers doesn't need
   to ask. Never edit `workflow/` files inside the worktree. Make Session End
   updates in the main project folder, with their own commit. Superpowers
   commits its own code.

9. **Index the folders, not every file.** `index.md` lists
   `docs/superpowers/specs/` and `docs/superpowers/plans/` once; the journal
   names the individual files.
