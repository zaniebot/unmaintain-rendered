```yaml
number: 2692
title: "Respect `--no-index` with `--find-links` in `pip sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dist-db
created_at: 2024-03-27T15:55:30Z
updated_at: 2024-03-27T16:15:15Z
url: https://github.com/astral-sh/uv/pull/2692
synced_at: 2026-01-10T14:49:08Z
```

# Respect `--no-index` with `--find-links` in `pip sync`

---

_Pull request opened by @charliermarsh on 2024-03-27 15:55_

## Summary

In `pip sync`, we weren't properly handling cases in which a package _only_ existed in `--find-links` (e.g., the user passed `--offline` or `--no-index`).

I plan to explore removing `Finder` entirely to avoid these mismatch bugs between `pip sync` and other commands, but this is fine for now.

Closes https://github.com/astral-sh/uv/issues/2688.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2024-03-27 15:55_

---

_Marked ready for review by @charliermarsh on 2024-03-27 15:55_

---

_Merged by @charliermarsh on 2024-03-27 16:15_

---

_Closed by @charliermarsh on 2024-03-27 16:15_

---

_Branch deleted on 2024-03-27 16:15_

---
