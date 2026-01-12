```yaml
number: 15457
title: "Change `ProgramSettings::python_platform` to return a reference"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/program-settings-platform-refe
created_at: 2025-01-13T14:57:30Z
updated_at: 2025-01-13T15:23:36Z
url: https://github.com/astral-sh/ruff/pull/15457
synced_at: 2026-01-12T15:55:51Z
```

# Change `ProgramSettings::python_platform` to return a reference

---

_@MichaReiser_

## Summary


`PythonPlatform` is now clone, we should return a reference instead of cloning it everytime the field is read.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-01-13 14:57_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-13 14:57_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-13 14:57_

---

_Label `red-knot` added by @MichaReiser on 2025-01-13 14:58_

---

_@AlexWaygood approved on 2025-01-13 15:00_

I feel like this field could also probably be `Box<str>` rather than `String`, since I don't think we ever need to mutate it?

https://github.com/astral-sh/ruff/blob/0f8ccfcdb0bdcecbb17a30b8f721a7c3e4d48d4a/crates/red_knot_python_semantic/src/python_platform.rs#L18

though I doubt it matters much

---

_Comment by @github-actions[bot] on 2025-01-13 15:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-01-13 15:23_

I prefer using `String` or `Vec` because they're simpler over `Box` unless the difference matters because the type is frequently used (which `PythonPlatform` isn't)

---

_Merged by @MichaReiser on 2025-01-13 15:23_

---

_Closed by @MichaReiser on 2025-01-13 15:23_

---

_Branch deleted on 2025-01-13 15:23_

---
