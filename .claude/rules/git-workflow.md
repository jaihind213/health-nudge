# Task & git workflow

- One task = one file `tasks/HN-XXX-slug.md` and one branch `HN-XXX-short-slug`,
  created from an up-to-date `main`.
- Every commit message starts with the task ID: `HN-XXX: <what>`.
- At the start of each new task: for every `in-review` task, run
  `gh pr view <url> --json state`; if `MERGED`, set status `done` (task file + INDEX).
- When done:
  1. pytest, isort --check, black --check, flake8 all pass (report results).
  2. Record branch, PR URL and commit hashes in the task file; status `in-review`;
     update `tasks/INDEX.md` — in the same PR.
  3. `git push -u origin <branch>`.
  4. `gh pr create --title "[HN-XXX] <title>"` with body: Summary, How to test,
     and `Task: tasks/HN-XXX-....md`.
- The user merges PRs. Never push to main, never merge PRs, never force-push.
- If a task settles something future tasks need (schema detail, metric
  definition, convention), add it to the right doc (docs/, CLAUDE.md, rules)
  as part of that task.
