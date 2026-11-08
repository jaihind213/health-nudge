---
name: new-task
description: Define a new HN-XXX task from the user's description (optionally with a wireframe path) - asks up to 4 clarifying questions, creates the task file from the template and adds it to tasks/INDEX.md. Does not implement anything.
---

# new-task

1. Read the user's description. If a wireframe path is given, check it exists in
   `wireframes/` (rename suggestion: `HN-XXX-slug.png`) and look at the image.
2. Read `tasks/INDEX.md`, `tasks/_TEMPLATE.md`, `docs/metrics.md` and any related
   task files to avoid overlap and reuse existing metrics/functions.
3. Ask the user up to 4 clarifying questions, only where the answer changes scope
   or acceptance criteria (metric definitions, filters, date ranges, edge cases).
   Wait for answers.
4. Choose the next ID: highest existing HN number + 1, zero-padded to 3 digits.
5. Create `tasks/HN-XXX-short-slug.md` from the template: status `todo`,
   `wireframe` path or `none`, empty branch/pr/commits. Fill Goal, Details and
   specific, testable Acceptance criteria — always include: tests for new data/metrics
   functions, AppTest smoke test for any page, MIN_GROUP_SIZE suppression where
   participants are grouped, and pytest/isort/black/flake8 passing.
6. Add a row to `tasks/INDEX.md` (ID link | Title | todo | empty PR).
7. Show the user the task file. Do NOT create a branch or start implementing.
   (Committing it happens with the task's own branch, or as the user asks.)
