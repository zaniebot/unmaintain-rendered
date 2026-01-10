```yaml
number: 16169
title: Fix missing serde feature for red_knot_python_semantic
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/fix-missing-serde-feature
created_at: 2025-02-14T20:28:07Z
updated_at: 2025-02-14T20:31:56Z
url: https://github.com/astral-sh/ruff/pull/16169
synced_at: 2026-01-10T19:57:22Z
```

# Fix missing serde feature for red_knot_python_semantic

---

_Pull request opened by @MichaReiser on 2025-02-14 20:28_

## Summary

Running `cargo test -p red_knot_python_semantic` failed because of a missing serde feature. This PR enables the `ruff_python_ast`'`s `serde` if the crate's `serde` feature is enabled

Follow up to https://github.com/astral-sh/ruff/pull/16147

## Test Plan

`cargo test -p red_knot_python_semantic` compiles again


---

_Review requested from @carljm by @MichaReiser on 2025-02-14 20:28_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-14 20:28_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-14 20:28_

---

_Label `red-knot` added by @MichaReiser on 2025-02-14 20:28_

---

_Comment by @MichaReiser on 2025-02-14 20:30_

Come on, build faster :P

---

_Merged by @MichaReiser on 2025-02-14 20:31_

---

_Closed by @MichaReiser on 2025-02-14 20:31_

---

_Branch deleted on 2025-02-14 20:31_

---
