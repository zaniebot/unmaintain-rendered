```yaml
number: 13317
title: "Remove allocation from `ruff_python_stdlib::builtins::python_builtins`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: builtins-no-alloc
created_at: 2024-09-10T18:53:29Z
updated_at: 2024-09-10T20:34:25Z
url: https://github.com/astral-sh/ruff/pull/13317
synced_at: 2026-01-10T21:38:32Z
```

# Remove allocation from `ruff_python_stdlib::builtins::python_builtins`

---

_Pull request opened by @AlexWaygood on 2024-09-10 18:53_

We only ever iterate through these, so allocating a vec shouldn't really be necessary. It does make the function slightly more complicated, though, so I'm not sure if it's worth it.

---

_Label `internal` added by @AlexWaygood on 2024-09-10 18:53_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-10 18:53_

---

_@AlexWaygood reviewed on 2024-09-10 18:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/mod.rs`:1973 on 2024-09-10 18:54_

I don't fully understand why, but the borrow checker wouldn't let me chain the three iterables together with this change.

---

_Comment by @github-actions[bot] on 2024-09-10 19:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:1973 on 2024-09-10 20:28_

Hmm, that's annoying. The problem is that Rust infers the Item type after the first call to be `&'static str` and the second chain call has a more constraint lifetime and `chain` requires that the `Item` type of both iterators is equal. 

---

_@MichaReiser approved on 2024-09-10 20:28_

---

_Merged by @AlexWaygood on 2024-09-10 20:34_

---

_Closed by @AlexWaygood on 2024-09-10 20:34_

---

_Branch deleted on 2024-09-10 20:34_

---
