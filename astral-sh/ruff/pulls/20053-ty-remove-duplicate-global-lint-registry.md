```yaml
number: 20053
title: "[ty] Remove duplicate global lint registry"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/default-lint-registry
created_at: 2025-08-22T21:50:02Z
updated_at: 2025-08-23T12:59:38Z
url: https://github.com/astral-sh/ruff/pull/20053
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Remove duplicate global lint registry

---

_Pull request opened by @ibraheemdev on 2025-08-22 21:50_

## Summary

Looks like an oversight at some point that led to two identical globals, the one in `ty_project` just calls `ty_python_semantic::register_lints`.

---

_Review requested from @carljm by @ibraheemdev on 2025-08-22 21:50_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-08-22 21:50_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-08-22 21:50_

---

_Review requested from @dcreager by @ibraheemdev on 2025-08-22 21:50_

---

_Label `internal` added by @ibraheemdev on 2025-08-22 21:50_

---

_Label `ty` added by @ibraheemdev on 2025-08-22 21:50_

---

_Comment by @github-actions[bot] on 2025-08-22 21:51_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-22 21:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-08-22 22:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@carljm approved on 2025-08-22 23:09_

Thanks!

---

_Merged by @ibraheemdev on 2025-08-22 23:43_

---

_Closed by @ibraheemdev on 2025-08-22 23:43_

---

_Branch deleted on 2025-08-22 23:43_

---

_Comment by @MichaReiser on 2025-08-23 06:51_

That was sort of intentional to already have an extension point where we register lints that aren't from ty_python_semantic. Similar to how each crate has their own db. For example, the project crate will need to register all rules from the linter crate

---

_Comment by @carljm on 2025-08-23 12:59_

> That was sort of intentional to already have an extension point where we register lints that aren't from ty_python_semantic. Similar to how each crate has their own db. For example, the project crate will need to register all rules from the linter crate

Oops. I would have left this one for you to review if you weren't out :)

That said, I think there's an argument for not leaving around confusing unused things -- we can easily add this back in when we actually have a use for it.

---
