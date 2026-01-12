```yaml
number: 7904
title: Respect project upper bounds when filtering wheels on requires-python
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/ignore
created_at: 2024-10-03T17:57:35Z
updated_at: 2024-10-04T10:35:52Z
url: https://github.com/astral-sh/uv/pull/7904
synced_at: 2026-01-12T16:08:04Z
```

# Respect project upper bounds when filtering wheels on requires-python

---

_@charliermarsh_

## Summary

If the user sets an upper-bound on their `requires-python`, we can omit more wheels.


---

_Review requested from @konstin by @charliermarsh on 2024-10-03 17:57_

---

_Label `enhancement` added by @charliermarsh on 2024-10-03 17:57_

---

_Comment by @charliermarsh on 2024-10-03 17:58_

@konstin -- I would appreciate a close review on this one as I'm not 100% confident on the Python and ABI tag rules.

---

_Review comment by @konstin on `crates/uv-resolver/src/requires_python.rs`:384 on 2024-10-04 09:56_

fwiw I don't know the semantics for the PyPy (haven't found any docs) but unless we find counterexample I'd follow CPython as we do

---

_@konstin approved on 2024-10-04 10:04_

---

_Merged by @charliermarsh on 2024-10-04 10:35_

---

_Closed by @charliermarsh on 2024-10-04 10:35_

---

_Branch deleted on 2024-10-04 10:35_

---
