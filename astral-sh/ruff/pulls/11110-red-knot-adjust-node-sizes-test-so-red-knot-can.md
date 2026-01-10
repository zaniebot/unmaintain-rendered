```yaml
number: 11110
title: "[red-knot] Adjust node sizes test so red-knot can pass CI"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: red-knot
head: cjm/red-knot-pass-tests
created_at: 2024-04-23T18:03:40Z
updated_at: 2024-04-26T20:06:45Z
url: https://github.com/astral-sh/ruff/pull/11110
synced_at: 2026-01-10T22:37:01Z
```

# [red-knot] Adjust node sizes test so red-knot can pass CI

---

_Pull request opened by @carljm on 2024-04-23 18:03_

This is needed because of the AST changes from `OnceCell` to `OnceLock`.

If we decide we don't need AST to be Sync/Send, we can revert this along with those changes; just want green CI for now.

Also adjust a URL in doc comments so `cargo docs` doesn't complain about it.


---

_Review requested from @MichaReiser by @carljm on 2024-04-23 18:03_

---

_Renamed from "Adjust node sizes test so red-knot can pass CI" to "[red-knot] Adjust node sizes test so red-knot can pass CI" by @carljm on 2024-04-23 18:19_

---

_Comment by @carljm on 2024-04-23 18:21_

I don't know what the ecosystem failure is about, or whether it's related to red-knot. Shouldn't matter much though, since most red-knot PRs won't touch the linter for now anyway.

---

_@charliermarsh approved on 2024-04-23 18:22_

I think the ecosystem failure is ok. It may get confused sometimes when you have a non-`main` upstream (can't remember).

---

_@MichaReiser approved on 2024-04-23 19:04_

---

_Merged by @carljm on 2024-04-23 19:06_

---

_Closed by @carljm on 2024-04-23 19:06_

---

_Branch deleted on 2024-04-23 19:06_

---

_Label `internal` added by @carljm on 2024-04-26 20:06_

---
