```yaml
number: 4911
title: Avoid AND-ing multi-term specifiers in marker normalization
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/term
created_at: 2024-07-09T04:42:23Z
updated_at: 2024-07-09T16:10:19Z
url: https://github.com/astral-sh/uv/pull/4911
synced_at: 2026-01-10T13:42:52Z
```

# Avoid AND-ing multi-term specifiers in marker normalization

---

_Pull request opened by @charliermarsh on 2024-07-09 04:42_

## Summary

Given `python_version != '3.8' and python_version < '3.10'`, the first term was expanded to `python_version < '3.8'` and `python_version > '3.8'`. We then AND'd all three terms together. We don't seem to have a way to differentiate between the terms to AND and the terms to OR in the normalization code (it all gets flattened together), so instead this PR expands the expressions at the leaf level and then flattens them at the level above when appropriate.

Closes https://github.com/astral-sh/uv/issues/4910.


---

_Review requested from @ibraheemdev by @charliermarsh on 2024-07-09 04:42_

---

_Label `bug` added by @charliermarsh on 2024-07-09 04:42_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-09 04:42_

---

_@charliermarsh reviewed on 2024-07-09 04:43_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/marker.rs`:269 on 2024-07-09 04:43_

Painful attempt to avoid allocating a vector in the common case (a single term).

---

_@BurntSushi approved on 2024-07-09 14:21_

---

_Merged by @charliermarsh on 2024-07-09 16:04_

---

_Closed by @charliermarsh on 2024-07-09 16:04_

---

_Branch deleted on 2024-07-09 16:04_

---

_@ibraheemdev reviewed on 2024-07-09 16:10_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/marker.rs`:224 on 2024-07-09 16:10_

Is one level deeper enough? Or do we have to recurse?

---
