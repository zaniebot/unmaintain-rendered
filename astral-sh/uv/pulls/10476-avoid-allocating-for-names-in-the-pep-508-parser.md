```yaml
number: 10476
title: Avoid allocating for names in the PEP 508 parser
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/p
created_at: 2025-01-10T19:23:44Z
updated_at: 2025-01-10T20:12:24Z
url: https://github.com/astral-sh/uv/pull/10476
synced_at: 2026-01-10T11:44:51Z
```

# Avoid allocating for names in the PEP 508 parser

---

_Pull request opened by @charliermarsh on 2025-01-10 19:23_

## Summary

We can read from the slice directly. I don't think this will affect performance today, because `from_str` will then allocate, but it _should_ be a speedup once #10475 merges, since we can then avoid allocating a `String` and go straight from `str` to `ArcStr`.


---

_Label `performance` added by @charliermarsh on 2025-01-10 19:23_

---

_@zanieb approved on 2025-01-10 19:28_

---

_Merged by @charliermarsh on 2025-01-10 20:12_

---

_Closed by @charliermarsh on 2025-01-10 20:12_

---

_Branch deleted on 2025-01-10 20:12_

---
