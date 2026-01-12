```yaml
number: 9492
title: Fix message for __aenter__ in PLC2801
type: pull_request
state: merged
author: alex-700
labels:
  - bug
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-01-12T12:37:03Z
updated_at: 2024-02-03T22:09:02Z
url: https://github.com/astral-sh/ruff/pull/9492
synced_at: 2026-01-12T15:55:29Z
```

# Fix message for __aenter__ in PLC2801

---

_@alex-700_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix the message for `__aenter__ ` in PLC2801 (introduced in https://github.com/astral-sh/ruff/pull/9166)
There is no `aenter` builtin in Python, so the current message is misleading.
I take the message from original lint https://github.com/pylint-dev/pylint/blob/main/pylint/constants.py#L211

P.S. I think here should be more accurate synchronization with original lint (e.g. the current implementation will not lint `__enter__`  on my first sight), but it is out-of-scope of this change.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-01-12 12:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @charliermarsh on 2024-01-12 13:48_

---

_Comment by @charliermarsh on 2024-01-12 13:48_

Thanks!

---

_Merged by @charliermarsh on 2024-01-12 13:48_

---

_Closed by @charliermarsh on 2024-01-12 13:48_

---

_Comment by @diceroll123 on 2024-01-13 01:10_

Woops!

---

_Branch deleted on 2024-02-03 22:09_

---
