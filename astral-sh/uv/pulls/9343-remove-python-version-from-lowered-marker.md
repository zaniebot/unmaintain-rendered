```yaml
number: 9343
title: "Remove `python_version` from lowered marker representation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lower-ii
created_at: 2024-11-22T00:39:46Z
updated_at: 2024-11-26T14:39:38Z
url: https://github.com/astral-sh/uv/pull/9343
synced_at: 2026-01-12T16:08:46Z
```

# Remove `python_version` from lowered marker representation

---

_@charliermarsh_

## Summary

We never construct these -- they should be impossible, since we always translate to `python_full_version`. This PR encodes that impossibility in the types.


---

_@charliermarsh reviewed on 2024-11-22 00:40_

---

_Review comment by @charliermarsh on `crates/uv-pep508/src/marker/tree.rs`:942 on 2024-11-22 00:40_

An example of why this is important: I think this was just straight-up wrong? (But it's unused.)

---

_Label `internal` added by @charliermarsh on 2024-11-22 00:40_

---

_@konstin approved on 2024-11-26 10:55_

nice

---

_Merged by @charliermarsh on 2024-11-26 14:39_

---

_Closed by @charliermarsh on 2024-11-26 14:39_

---

_Branch deleted on 2024-11-26 14:39_

---
