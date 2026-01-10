```yaml
number: 16337
title: Extract class and instance types
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/extract-class-instance-from-types
created_at: 2025-02-24T07:59:51Z
updated_at: 2025-02-24T12:48:26Z
url: https://github.com/astral-sh/ruff/pull/16337
synced_at: 2026-01-10T19:49:01Z
```

# Extract class and instance types

---

_Pull request opened by @MichaReiser on 2025-02-24 07:59_

## Summary

This is a first step towards reudcing the size of `types.rs`. 
It moves all types related to classes and instances into a new `class.rs`

## Test Plan

This PR contains no behavior changes.


---

_Review requested from @carljm by @MichaReiser on 2025-02-24 07:59_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-24 07:59_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-24 07:59_

---

_Label `red-knot` added by @MichaReiser on 2025-02-24 07:59_

---

_@sharkdp approved on 2025-02-24 09:21_

Thanks — and sorry for already introducing merge conflicts.

---

_Comment by @MichaReiser on 2025-02-24 11:21_

Oh no, what did you change 😆 

---

_Merged by @MichaReiser on 2025-02-24 11:36_

---

_Closed by @MichaReiser on 2025-02-24 11:36_

---

_Branch deleted on 2025-02-24 11:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class_base.rs`:55 on 2025-02-24 12:21_

hrm, I preferred the code the way it was here -- I find unpacking the inner `Class` more readable when it comes to `ClassLiteralType` :/

it makes it easier for me to distinguish the concepts of a _class-literal type_ (a set that has exactly one inhabitant) and the underlying _class_ (the sole member of that set at runtime)

---

_@AlexWaygood reviewed on 2025-02-24 12:21_

Nice, thank you!

---

_@AlexWaygood reviewed on 2025-02-24 12:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class_base.rs`:55 on 2025-02-24 12:48_

That said, there probably would be benefits to treating the `class` field as fully opaque to external modules in the long term. And this probably gets us closer to that

---
