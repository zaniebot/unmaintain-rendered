```yaml
number: 7057
title: "Fix `getattr` calls on int literals"
type: pull_request
state: merged
author: density
labels:
  - bug
  - fuzzer
assignees: []
merged: true
base: main
head: B009-fix
created_at: 2023-09-02T03:47:14Z
updated_at: 2023-09-02T11:51:49Z
url: https://github.com/astral-sh/ruff/pull/7057
synced_at: 2026-01-12T02:45:38Z
```

# Fix `getattr` calls on int literals

---

_Pull request opened by @density on 2023-09-02 03:47_

## Summary

`getattr` calls on `int` literals need to be parenthesized in order to avoid a syntax error

Fixes #6989

## Test Plan

Additional snapshot tests


---

_Comment by @github-actions[bot] on 2023-09-02 04:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Marked ready for review by @density on 2023-09-02 04:01_

---

_Label `bug` added by @charliermarsh on 2023-09-02 11:36_

---

_Label `fuzzer` added by @charliermarsh on 2023-09-02 11:36_

---

_Comment by @charliermarsh on 2023-09-02 11:36_

Thanks!

---

_Merged by @charliermarsh on 2023-09-02 11:45_

---

_Closed by @charliermarsh on 2023-09-02 11:45_

---
