```yaml
number: 3586
title: Split extra validation from graph construction
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-05-14T18:43:48Z
updated_at: 2024-05-14T20:41:17Z
url: https://github.com/astral-sh/uv/pull/3586
synced_at: 2026-01-12T16:05:44Z
```

# Split extra validation from graph construction

---

_@charliermarsh_

## Summary

Splits this into two loops that each handle independent cases, to make the code a little easier to reason about. No behavioral or logic changes -- just splitting the `match` across two loops.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-14 18:43_

---

_Label `internal` added by @charliermarsh on 2024-05-14 18:43_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-05-14 18:43_

---

_Marked ready for review by @charliermarsh on 2024-05-14 18:44_

---

_@ibraheemdev approved on 2024-05-14 20:22_

---

_Merged by @charliermarsh on 2024-05-14 20:41_

---

_Closed by @charliermarsh on 2024-05-14 20:41_

---

_Branch deleted on 2024-05-14 20:41_

---
