```yaml
number: 8058
title: "[pylint] - implement `global-at-module-level` (`W0604`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
assignees: []
merged: true
base: main
head: add-PLW0604
created_at: 2023-10-19T03:04:09Z
updated_at: 2023-10-19T04:54:28Z
url: https://github.com/astral-sh/ruff/pull/8058
synced_at: 2026-01-12T15:55:25Z
```

# [pylint] - implement `global-at-module-level` (`W0604`)

---

_@diceroll123_

## Summary

Implements [`global-at-module-level`/`W0604`](https://pylint.pycqa.org/en/latest/user_guide/messages/warning/global-at-module-level.html)

See #970

## Test Plan

`cargo test` and manually

---

_@charliermarsh reviewed on 2023-10-19 03:16_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/global_at_module_level.rs`:33 on 2023-10-19 03:16_

This won't catch cases like:

```python
if True:
    global X
```

Should those be flagged? If so, we should use `checker.semantic().current_scope().kind.is_module()`.

---

_@diceroll123 reviewed on 2023-10-19 03:18_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/global_at_module_level.rs`:33 on 2023-10-19 03:18_

Good catch! I do think they should.

---

_Comment by @github-actions[bot] on 2023-10-19 03:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Label `rule` added by @charliermarsh on 2023-10-19 04:40_

---

_Merged by @charliermarsh on 2023-10-19 04:48_

---

_Closed by @charliermarsh on 2023-10-19 04:48_

---
