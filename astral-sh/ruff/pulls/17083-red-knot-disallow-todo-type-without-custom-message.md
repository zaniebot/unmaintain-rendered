```yaml
number: 17083
title: "[red-knot] Disallow `todo_type!` without custom message"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/disallow-todo_type-without-message
created_at: 2025-03-31T08:42:30Z
updated_at: 2025-03-31T08:49:22Z
url: https://github.com/astral-sh/ruff/pull/17083
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Disallow `todo_type!` without custom message

---

_@sharkdp_

## Summary

Disallow empty `todo_type!()`s without a custom message. They can lead to spurious diffs in `mypy_primer` where the only thing that's changed is the file/line information.


---

_Label `red-knot` added by @sharkdp on 2025-03-31 08:42_

---

_Review requested from @carljm by @sharkdp on 2025-03-31 08:42_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-31 08:42_

---

_Review requested from @dcreager by @sharkdp on 2025-03-31 08:42_

---

_Comment by @github-actions[bot] on 2025-03-31 08:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
rich (https://github.com/Textualize/rich)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/control.py:198:27: Object of type `dict | @Todo[crates/red_knot_python_semantic/src/types/infer.rs:3627]` cannot be assigned to parameter 2 (`table`) of bound method `translate`; expected type `_TranslateTable`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/control.py:198:27: Object of type `dict | @Todo(dict comprehension type)` cannot be assigned to parameter 2 (`table`) of bound method `translate`; expected type `_TranslateTable`

```
</details>


---

_@MichaReiser approved on 2025-03-31 08:45_

---

_Merged by @sharkdp on 2025-03-31 08:49_

---

_Closed by @sharkdp on 2025-03-31 08:49_

---

_Branch deleted on 2025-03-31 08:49_

---
