```yaml
number: 10596
title: "[`flake8-comprehensions`] Handled special case for `C401` which also matches `C416`"
type: pull_request
state: merged
author: hikaru-kajita
labels:
  - rule
assignees: []
merged: true
base: main
head: unnecessary-generator-set
created_at: 2024-03-26T03:47:16Z
updated_at: 2024-03-29T04:42:55Z
url: https://github.com/astral-sh/ruff/pull/10596
synced_at: 2026-01-12T15:55:32Z
```

# [`flake8-comprehensions`] Handled special case for `C401` which also matches `C416`

---

_@hikaru-kajita_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Similar to #10419, there was a case where there is a collision of C401 and C416 (as discussed in #10101).
Fixed this by implementing short-circuit for the comprehension of the form `{x for x in foo}`.

## Test Plan

<!-- How was it tested? -->

Extended `C401.py` with the case where `set` is not builtin function, and divided the case where the short-circuit should occur.
Removed the last testcase of `print(f"{ {set(a for a in 'abc')} }")` test as this is invalid as a python code, but should I keep this?

---

_@charliermarsh approved on 2024-03-26 03:50_

This is great, thank you for following up on it!

---

_Label `rule` added by @charliermarsh on 2024-03-26 03:50_

---

_Merged by @charliermarsh on 2024-03-26 03:54_

---

_Closed by @charliermarsh on 2024-03-26 03:54_

---

_Comment by @github-actions[bot] on 2024-03-26 04:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Branch deleted on 2024-03-29 04:42_

---
