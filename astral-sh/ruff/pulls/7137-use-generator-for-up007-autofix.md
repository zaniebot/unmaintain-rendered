```yaml
number: 7137
title: Use generator for UP007 autofix
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: charlie/UP007
created_at: 2023-09-04T21:44:21Z
updated_at: 2023-09-05T11:51:43Z
url: https://github.com/astral-sh/ruff/pull/7137
synced_at: 2026-01-12T02:45:38Z
```

# Use generator for UP007 autofix

---

_Pull request opened by @charliermarsh on 2023-09-04 21:44_

## Summary

Ensures that we generate syntactically valid code, at the cost of lower fidelity. For example, we might collapse implicitly concatenated strings, or add unnecessary parentheses (although the latter is solvable by improving the generator).

Closes https://github.com/astral-sh/ruff/issues/7131.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-09-04 21:44_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-04 21:44_

---

_@MichaReiser approved on 2023-09-05 06:59_

---

_Merged by @charliermarsh on 2023-09-05 11:41_

---

_Closed by @charliermarsh on 2023-09-05 11:41_

---

_Branch deleted on 2023-09-05 11:41_

---

_Comment by @github-actions[bot] on 2023-09-05 11:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
