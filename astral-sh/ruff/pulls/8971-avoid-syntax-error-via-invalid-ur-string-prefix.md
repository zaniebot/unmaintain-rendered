```yaml
number: 8971
title: Avoid syntax error via invalid ur string prefix
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unicode
created_at: 2023-12-02T18:24:46Z
updated_at: 2023-12-02T18:44:54Z
url: https://github.com/astral-sh/ruff/pull/8971
synced_at: 2026-01-10T23:40:55Z
```

# Avoid syntax error via invalid ur string prefix

---

_Pull request opened by @charliermarsh on 2023-12-02 18:24_

## Summary

If a string has a Unicode prefix, we can't add the `r` prefix on top of that -- we need to remove and replace it. (The Unicode prefix is redundant anyway in Python 3.)

Closes https://github.com/astral-sh/ruff/issues/8967.


---

_Label `bug` added by @charliermarsh on 2023-12-02 18:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pycodestyle/mod.rs`:34 on 2023-12-02 18:25_

I deleted `W605_1.py` which looks identical to `W605_0.py`? And then renamed `W605_2.py` to `W605_1.py`.

---

_@charliermarsh reviewed on 2023-12-02 18:25_

---

_Merged by @charliermarsh on 2023-12-02 18:37_

---

_Closed by @charliermarsh on 2023-12-02 18:37_

---

_Branch deleted on 2023-12-02 18:37_

---

_Comment by @github-actions[bot] on 2023-12-02 18:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
