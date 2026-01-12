```yaml
number: 8359
title: "Avoid including literal `shell=True` for truthy, non-`True` diagnostics"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/shell
created_at: 2023-10-30T15:35:26Z
updated_at: 2023-10-30T15:50:56Z
url: https://github.com/astral-sh/ruff/pull/8359
synced_at: 2026-01-12T15:55:26Z
```

# Avoid including literal `shell=True` for truthy, non-`True` diagnostics

---

_@charliermarsh_

## Summary

If the value of `shell` wasn't literally `True`, we now show a message describing it as truthy, rather than the (misleading) `shell=True` literal in the diagnostic.

Closes https://github.com/astral-sh/ruff/issues/8310.

---

_Label `bug` added by @charliermarsh on 2023-10-30 15:35_

---

_Renamed from "Use clearer truthiness message when reporting `shell=True` diagnostics" to "Avoid including literal `shell=True` for truthy, non-`True` diagnostics" by @charliermarsh on 2023-10-30 15:35_

---

_@zanieb approved on 2023-10-30 15:38_

---

_Merged by @charliermarsh on 2023-10-30 15:44_

---

_Closed by @charliermarsh on 2023-10-30 15:44_

---

_Branch deleted on 2023-10-30 15:44_

---

_Comment by @github-actions[bot] on 2023-10-30 15:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.

✅ ecosystem check detected no format changes.



---
