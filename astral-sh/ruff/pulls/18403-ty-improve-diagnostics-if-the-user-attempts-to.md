```yaml
number: 18403
title: "[ty] Improve diagnostics if the user attempts to import a stdlib module that does not exist on their configured Python version"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/pyversion-hint-import
created_at: 2025-05-31T12:31:19Z
updated_at: 2025-06-02T10:52:27Z
url: https://github.com/astral-sh/ruff/pull/18403
synced_at: 2026-01-12T15:56:18Z
```

# [ty] Improve diagnostics if the user attempts to import a stdlib module that does not exist on their configured Python version

---

_@AlexWaygood_

## Summary

If the user attempts to import a module that doesn't exist on their configured Python version, but does exist in the standard library on some other Python versions, give them a hint in the diagnostic that they may have misconfigured their Python version.

Demo:

![image](https://github.com/user-attachments/assets/3da33786-293a-4215-8c9c-afaeb383c6ac)

## Test Plan

Snapshots 'n' screenshots


---

_Review requested from @carljm by @AlexWaygood on 2025-05-31 12:31_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-31 12:31_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-31 12:31_

---

_Label `ty` added by @AlexWaygood on 2025-05-31 12:31_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-31 12:31_

---

_Comment by @github-actions[bot] on 2025-05-31 12:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-31 12:45_

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

_@MichaReiser reviewed on 2025-06-02 07:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:14 on 2025-06-02 07:16_

I would use `PythonVersion { 3, 0 }` for the one use case we have for this version. 

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/typeshed.rs`:275 on 2025-06-02 07:17_

What's the reason for implementing this as a method instead of implementing `Display` for `PyVersionRange`. `Display` is intended for user facing messages.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/typeshed.rs`:286 on 2025-06-02 07:18_

```suggestion
                        if range_inclusive.start() <= (3, 0).into() {
```

---

_@MichaReiser approved on 2025-06-02 07:19_

Nice

---

_@AlexWaygood reviewed on 2025-06-02 07:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/module_resolver/typeshed.rs`:275 on 2025-06-02 07:34_

`Display` is already implemented for `PyVersionRange`, and does something different (it just round-trips back to the original representation of the range in typeshed's `VERSIONS` file). But maybe this new method should be the `Display` implementation, and what is currently the `Display` implementation should be a custom method. Or maybe both should be custom methods, and we shouldn't implement `Display` at all for this type (since it isn't really obvious to me that one is a more canonical way of displaying a `PyVersionRange` than the other).

Not sure, WDYT?

---

_@MichaReiser reviewed on 2025-06-02 07:43_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/typeshed.rs`:275 on 2025-06-02 07:43_

Oh I see. What I understand is that `FromStr` and `Display` should be compatible. So what you have is correct then.

---

_Merged by @AlexWaygood on 2025-06-02 10:52_

---

_Closed by @AlexWaygood on 2025-06-02 10:52_

---

_Branch deleted on 2025-06-02 10:52_

---
