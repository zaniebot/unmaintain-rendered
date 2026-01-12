```yaml
number: 7665
title: "Refactor: use `Settings` struct"
type: pull_request
state: merged
author: bluthej
labels:
  - internal
assignees: []
merged: true
base: main
head: pass-settings-struct
created_at: 2023-09-26T08:05:24Z
updated_at: 2023-09-27T01:14:59Z
url: https://github.com/astral-sh/ruff/pull/7665
synced_at: 2026-01-12T02:39:10Z
```

# Refactor: use `Settings` struct

---

_Pull request opened by @bluthej on 2023-09-26 08:05_

## Summary

Pass around a `Settings` struct instead of individual members to simplify function signatures and to make it easier to add new settings.

This PR was suggested in [this comment](https://github.com/astral-sh/ruff/issues/1567#issuecomment-1734182803).

## Note on the choices

I chose which functions to modify based on which seem most likely to use new settings, but suggestions on my choices are welcome!

---

_Comment by @github-actions[bot] on 2023-09-26 08:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb approved on 2023-09-26 15:14_

Thanks!

---

_Label `internal` added by @charliermarsh on 2023-09-26 15:16_

---

_Merged by @zanieb on 2023-09-26 17:17_

---

_Closed by @zanieb on 2023-09-26 17:17_

---

_Branch deleted on 2023-09-26 20:28_

---

_Comment by @charliermarsh on 2023-09-27 01:14_

Thanks @bluthej, welcome to the project!

---
