```yaml
number: 6850
title: "Update FastAPI integration docs (#6833)"
type: pull_request
state: merged
author: denkasyanov
labels:
  - documentation
assignees: []
merged: true
base: main
head: update-fastapi-docs
created_at: 2024-08-30T02:56:35Z
updated_at: 2024-08-30T14:00:26Z
url: https://github.com/astral-sh/uv/pull/6850
synced_at: 2026-01-10T12:53:35Z
```

# Update FastAPI integration docs (#6833)

---

_Pull request opened by @denkasyanov on 2024-08-30 02:56_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

- Fixed the directory structure and commands for the second scenario #6833
- Added a headline "Migrating an existing FastAPI project" because the first part looks like a migration scenario, but didn't have its own section before.
- Added explicit `--app` flags to commands to emphasize that the commands create an Application project. Although maybe unnecessary considering that `--app` is now default.
- Added instructions for testing that the dev server and Docker image work correctly
- Took the liberty of adding a `project` at the root of all directory structures and appropriate commands.
  - With explicitly defined root directory it is easier to differentiate between the `project` root directory and FastAPI's `app` directory.
  - Without it it could be less obvious for developers less familiar with FastAPI. Had a similar issue when started using Django several years ago.
  - If I left `app` in the command, then after copying the **app directory** from https://github.com/astral-sh/uv-fastapi-example the path would be `app/app/...`.
- Cleaned up glyphs in tree sctructures that were copied from FastAPI docs.

## Caveats

- On project initialization `hello.py` is created. It is not reflected in directory structure trees in this PR and may be slightly confusing for developers less familiar with uv. 
  - I believe it will be soon addressed in #6750 and after that the docs will reflect actual directory structure. 

---

_Comment by @zanieb on 2024-08-30 03:22_

Oh! We both did this at once. I'm going to merge https://github.com/astral-sh/uv/pull/6851 quick because it's shorter, but I think some of your changes still make sense if you want to rebase after? I think we can just drop the whole second section now that I wrote the first to use `uv init`.

---

_Comment by @denkasyanov on 2024-08-30 03:49_

I knew I need to leave a comment that I'm working on it, condsidering your speed of shipping 😄

Sure, I'll fix my PR.

Based on what you saw, is there anything I should change? Maybe add/not add/clarify something?

---

_Comment by @zanieb on 2024-08-30 04:08_

Sorry, I'm about to go on vacation and only gave it a quick skim. @charliermarsh will probably take over.

---

_Assigned to @charliermarsh by @zanieb on 2024-08-30 04:08_

---

_@charliermarsh approved on 2024-08-30 13:59_

Great changes, thanks.

---

_Label `documentation` added by @charliermarsh on 2024-08-30 13:59_

---

_Merged by @charliermarsh on 2024-08-30 14:00_

---

_Closed by @charliermarsh on 2024-08-30 14:00_

---
