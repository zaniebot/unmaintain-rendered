```yaml
number: 1956
title: Properly apply constraints in venv audit
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/constraints
created_at: 2024-02-24T19:57:26Z
updated_at: 2024-02-24T21:42:46Z
url: https://github.com/astral-sh/uv/pull/1956
synced_at: 2026-01-10T14:54:43Z
```

# Properly apply constraints in venv audit

---

_Pull request opened by @charliermarsh on 2024-02-24 19:57_

## Summary

We were applying every constraint to every dependency. This is "harmless" in practice since this is just an optimization, but we thus had false negatives ~every time which could lead to wasted work.


---

_Label `bug` added by @charliermarsh on 2024-02-24 19:57_

---

_Marked ready for review by @charliermarsh on 2024-02-24 21:42_

---

_Merged by @charliermarsh on 2024-02-24 21:42_

---

_Closed by @charliermarsh on 2024-02-24 21:42_

---

_Branch deleted on 2024-02-24 21:42_

---
