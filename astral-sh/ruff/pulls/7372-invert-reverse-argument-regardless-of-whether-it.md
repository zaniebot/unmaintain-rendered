```yaml
number: 7372
title: "Invert reverse argument regardless of whether it's a boolean"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/invert
created_at: 2023-09-13T23:52:56Z
updated_at: 2023-09-14T00:12:37Z
url: https://github.com/astral-sh/ruff/pull/7372
synced_at: 2026-01-12T15:55:23Z
```

# Invert reverse argument regardless of whether it's a boolean

---

_@charliermarsh_

## Summary

When fixing `reversed(sorted(x, reverse=False))`, we rewrite as `sorted(x, reverse=True)`. However, if the `reverse` argument isn't `True` or `False`, we leave it as-is, which is incorrect.

Now, given `reversed(sorted(x, reverse=y))`, we rewrite as `sorted(x, reverse=not y)`.


---

_Label `bug` added by @charliermarsh on 2023-09-13 23:53_

---

_@charliermarsh reviewed on 2023-09-13 23:54_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pytest_style/rules/assertion.rs`:585 on 2023-09-13 23:54_

Pulled this out into a common home + generalized it to `True` and `False`.

---

_Comment by @github-actions[bot] on 2023-09-14 00:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-09-14 00:12_

---

_Closed by @charliermarsh on 2023-09-14 00:12_

---

_Branch deleted on 2023-09-14 00:12_

---
