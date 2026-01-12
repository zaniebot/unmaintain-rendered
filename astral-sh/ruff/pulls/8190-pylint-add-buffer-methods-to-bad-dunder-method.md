```yaml
number: 8190
title: "[`pylint`] Add buffer methods to `bad-dunder-method-name` (`PLW3201`) exclusions"
type: pull_request
state: merged
author: TeamSpen210
labels:
  - rule
assignees: []
merged: true
base: main
head: buffer-special-method
created_at: 2023-10-25T00:05:21Z
updated_at: 2023-10-25T05:03:59Z
url: https://github.com/astral-sh/ruff/pull/8190
synced_at: 2026-01-12T02:32:42Z
```

# [`pylint`] Add buffer methods to `bad-dunder-method-name` (`PLW3201`) exclusions

---

_Pull request opened by @TeamSpen210 on 2023-10-25 00:05_

## Summary

Python 3.12 added the `__buffer__()`/`__release_buffer_()` special methods, which are incorrectly flagged as invalid dunder methods by `PLW3201`.

## Test Plan

Added definitions to the test suite, and confirmed they failed without the fix and are ignored after the fix was done.


---

_Comment by @github-actions[bot] on 2023-10-25 00:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb approved on 2023-10-25 05:03_

Thank you for contributing!

---

_Label `rule` added by @zanieb on 2023-10-25 05:03_

---

_Merged by @zanieb on 2023-10-25 05:03_

---

_Closed by @zanieb on 2023-10-25 05:03_

---

_Branch deleted on 2023-10-25 05:03_

---
