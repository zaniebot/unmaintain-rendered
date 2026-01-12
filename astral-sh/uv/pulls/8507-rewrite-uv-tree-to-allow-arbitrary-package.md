```yaml
number: 8507
title: "Rewrite `uv tree` to allow arbitrary `--package` includes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tree
created_at: 2024-10-23T18:31:59Z
updated_at: 2024-10-23T18:43:27Z
url: https://github.com/astral-sh/uv/pull/8507
synced_at: 2026-01-12T16:08:21Z
```

# Rewrite `uv tree` to allow arbitrary `--package` includes

---

_@charliermarsh_

## Summary

Previously, `uv tree --package` had some strange behavior due to how we were computing the root nodes. This PR refactors the entire implementation to use `petgraph` so we can do proper operations on a graph structure.

Closes https://github.com/astral-sh/uv/issues/8382.


---

_Label `bug` added by @charliermarsh on 2024-10-23 18:32_

---

_Comment by @charliermarsh on 2024-10-23 18:32_

Needs some new tests prior to merging.

---

_Merged by @charliermarsh on 2024-10-23 18:43_

---

_Closed by @charliermarsh on 2024-10-23 18:43_

---

_Branch deleted on 2024-10-23 18:43_

---
