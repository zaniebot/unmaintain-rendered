```yaml
number: 10700
title: Sort preferences by environment, then index
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pref-sort
created_at: 2025-01-17T00:18:00Z
updated_at: 2025-01-17T17:27:45Z
url: https://github.com/astral-sh/uv/pull/10700
synced_at: 2026-01-12T16:09:26Z
```

# Sort preferences by environment, then index

---

_@charliermarsh_

## Summary

This has a few effects:

1. We only call `preferences` once, which should be more efficient.
2. We collect `preferences` into a vector when there are multiple. Less efficient, but pretty rare?
3. We now correctly prefer preferences from the same index.

---

_Marked ready for review by @charliermarsh on 2025-01-17 01:14_

---

_Review requested from @konstin by @charliermarsh on 2025-01-17 01:14_

---

_Label `bug` added by @charliermarsh on 2025-01-17 01:15_

---

_Review comment by @konstin on `crates/uv-resolver/src/candidate_selector.rs`:199 on 2025-01-17 10:32_

We can use a smallvec here to avoid the allocation for diverging forks (with 2 elements it's even the same size, though this is stack only anyway)

---

_@konstin approved on 2025-01-17 10:32_

---

_Merged by @charliermarsh on 2025-01-17 17:27_

---

_Closed by @charliermarsh on 2025-01-17 17:27_

---

_Branch deleted on 2025-01-17 17:27_

---
