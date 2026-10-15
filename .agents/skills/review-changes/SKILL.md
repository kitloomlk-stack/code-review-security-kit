---
name: review-changes
description: "Review the current diff or branch and report ranked findings without editing code."
disable-model-invocation: true
---

# review-changes

When invoked, optionally with a base branch, commit or file list:

1. Work out what to review: the argument if given, otherwise the uncommitted changes, otherwise the current branch against its merge-base with the default branch. State the range you chose.
2. Read the full diff, then read the surrounding code, callers and tests for every changed function. Read any referenced issue, spec or documented coding standards in the repository.
3. Review for correctness, security, tests, design, readability, performance and compatibility, in that order of priority.
4. Do not edit any files.

Report findings grouped as blocker, should fix, suggestion and question. Each finding gives the file and line, what is wrong, the consequence, and a concrete fix. End with a short verdict (ready to merge, merge after fixes, or needs rework), and list what you checked and what you could not check.
