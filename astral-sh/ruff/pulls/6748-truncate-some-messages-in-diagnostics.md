```yaml
number: 6748
title: Truncate some messages in diagnostics
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/truncate
created_at: 2023-08-22T00:36:33Z
updated_at: 2023-08-22T03:46:25Z
url: https://github.com/astral-sh/ruff/pull/6748
synced_at: 2026-01-12T02:52:04Z
```

# Truncate some messages in diagnostics

---

_Pull request opened by @charliermarsh on 2023-08-22 00:36_

## Summary

I noticed this in the ecosystem CI check from https://github.com/astral-sh/ruff/pull/6742. If we include source code directly in a diagnostic, we need to be careful to avoid rendering multi-line diagnostics or even excessively long diagnostics.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-08-22 00:51_

---

_Merged by @charliermarsh on 2023-08-22 03:46_

---

_Closed by @charliermarsh on 2023-08-22 03:46_

---

_Branch deleted on 2023-08-22 03:46_

---
