```yaml
number: 7700
title: Include radix base prefix in large number representation
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/base
created_at: 2023-09-28T20:23:21Z
updated_at: 2023-09-28T20:44:22Z
url: https://github.com/astral-sh/ruff/pull/7700
synced_at: 2026-01-12T02:39:10Z
```

# Include radix base prefix in large number representation

---

_Pull request opened by @charliermarsh on 2023-09-28 20:23_

## Summary

When lexing a number like `0x995DC9BBDF1939FA` that exceeds our small number representation, we were only storing the portion after the base (in this case, `995DC9BBDF1939FA`). When using that representation in code generation, this could lead to invalid syntax, since `995DC9BBDF1939FA)` on its own is not a valid integer.

This PR modifies the code to store the full span, including the radix prefix.

See: https://github.com/astral-sh/ruff/issues/7455#issuecomment-1739802958.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-09-28 20:23_

---

_@zanieb approved on 2023-09-28 20:29_

---

_Merged by @charliermarsh on 2023-09-28 20:38_

---

_Closed by @charliermarsh on 2023-09-28 20:38_

---

_Branch deleted on 2023-09-28 20:38_

---

_Comment by @github-actions[bot] on 2023-09-28 20:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
