---
name: next-task
description: Implement the next task end to end - sync in-review statuses, pick the named or lowest-numbered todo task, branch, present a plan and wait for approval, implement, run checks, commit, open the PR and update the task file and INDEX.
---

# next-task

1. Sync: `git checkout main && git pull`. For each `in-review` task in
   `tasks/INDEX.md`, run `gh pr view <pr-url> --json state`; if `MERGED`, set status
   `done` in the task file and INDEX (carry this change in the new task's branch).
2. Pick the task the user named, else the lowest-numbered `todo`. Read it fully,
   plus CLAUDE.md rules and docs it touches. If it has a wireframe, use the
   `wireframe-to-page` skill for the build steps.
3. Create branch `HN-XXX-short-slug` from main. Set status `in-progress` and the
   `branch` field in the task file.
4. Present a plan: files to create/change, functions, tests, any new dependency
   (needs the user's approval), open questions. STOP and WAIT for approval.
5. Implement in small commits, each message `HN-XXX: <what>`. Follow the rules in
   `.claude/rules/`. Run everything via `mamba run -n health-nudge ...`.
6. If the task settled something future tasks need, update the right doc
   (`docs/metrics.md`, `docs/`, CLAUDE.md, rules).
7. Run pytest, isort --check, black --check, flake8 (and pre-commit once HN-003 is
   done). Fix until green. Verify each acceptance criterion; tick them in the file.
8. `git push -u origin <branch>`, then `gh pr create --title "[HN-XXX] <title>"`
   with body: Summary, How to test, `Task: tasks/HN-XXX-....md`.
9. In the task file set status `in-review`, `pr` URL, `commits` hashes; update
   INDEX.md (status + PR link). Commit `HN-XXX: update task status` and push.
10. Report check results and the PR URL. Never merge, never push to main, never force-push.
