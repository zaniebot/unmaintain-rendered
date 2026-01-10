```yaml
number: 6253
title: "Impl `Ord` for ADD `MarkerTree`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/impl-ord-for-add-marker-tree
created_at: 2024-08-20T14:57:41Z
updated_at: 2024-08-20T17:11:59Z
url: https://github.com/astral-sh/uv/pull/6253
synced_at: 2026-01-10T13:09:51Z
```

# Impl `Ord` for ADD `MarkerTree`

---

_Pull request opened by @konstin on 2024-08-20 14:57_

The ADD `MarkerTree` was including the non-deterministic, unstable `NodeId` in its `Ord` implementation since switching algebraic decision diagrams. By replacing this with a correct `Ord` implementation, we fix #6249.

Requires https://github.com/astral-sh/pubgrub/pull/31

---

_Review requested from @ibraheemdev by @konstin on 2024-08-20 14:57_

---

_@BurntSushi approved on 2024-08-20 15:20_

LGTM!

---

_Merged by @konstin on 2024-08-20 17:11_

---

_Closed by @konstin on 2024-08-20 17:11_

---

_Branch deleted on 2024-08-20 17:11_

---
