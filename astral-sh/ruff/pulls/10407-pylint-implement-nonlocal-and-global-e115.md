```yaml
number: 10407
title: "[`pylint`] Implement `nonlocal-and-global` (`E115`)"
type: pull_request
state: merged
author: hikaru-kajita
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: nonlocal-and-global
created_at: 2024-03-14T05:36:25Z
updated_at: 2024-03-18T05:04:27Z
url: https://github.com/astral-sh/ruff/pull/10407
synced_at: 2026-01-12T15:55:32Z
```

# [`pylint`] Implement `nonlocal-and-global` (`E115`)

---

_@hikaru-kajita_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement `E115` in the issue #970.
Reference to pylint docs: https://pylint.readthedocs.io/en/stable/user_guide/messages/error/nonlocal-and-global.html
Throws an error when a variable name is both declared as global and nonlocal

## Test Plan

With `nonlocal_and_global.py`


---

_Comment by @github-actions[bot] on 2024-03-14 05:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-18 00:28_

---

_Label `rule` added by @charliermarsh on 2024-03-18 00:28_

---

_Label `preview` added by @charliermarsh on 2024-03-18 00:28_

---

_@charliermarsh approved on 2024-03-18 00:36_

Thanks!

---

_Merged by @charliermarsh on 2024-03-18 00:43_

---

_Closed by @charliermarsh on 2024-03-18 00:43_

---

_Branch deleted on 2024-03-18 05:04_

---
