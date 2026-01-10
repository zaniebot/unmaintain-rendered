```yaml
number: 6178
title: "Tolerate missing `[project]` table in `uv venv`"
type: pull_request
state: merged
author: branchv
labels: []
assignees: []
merged: true
base: main
head: venv
created_at: 2024-08-17T23:47:21Z
updated_at: 2024-08-20T00:46:32Z
url: https://github.com/astral-sh/uv/pull/6178
synced_at: 2026-01-10T13:09:50Z
```

# Tolerate missing `[project]` table in `uv venv`

---

_Pull request opened by @branchv on 2024-08-17 23:47_

## Summary

Fixes #6177

This ensures a `pyproject.toml` file without a `[project]` table is not a fatal error for `uv venv`, which is just trying to discover/respect the project's `python-requires` (#5592).

Similarly, any caught `WorkspaceError` is now also non-fatal and instead prints a warning message (feeback welcome here, felt less surprising than e.g. a malformed `pyproject.toml` breaking `uv venv`).

## Test Plan

I added two test cases: `cargo test -p uv --test venv`

Also, existing venv tests were failing for me since I use fish and the printed activation script was `source .venv/bin/activate.fish` (to repro, just run the tests with `SHELL=fish`). So added an insta filter to normalize that.


---

_@charliermarsh approved on 2024-08-18 17:02_

This looks reasonable to me, thank you.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-18 17:02_

---

_Comment by @charliermarsh on 2024-08-18 17:12_

(I might just play with whether or not we show that warning prior to merging.)

---

_Merged by @charliermarsh on 2024-08-18 18:50_

---

_Closed by @charliermarsh on 2024-08-18 18:50_

---

_Branch deleted on 2024-08-20 00:46_

---
