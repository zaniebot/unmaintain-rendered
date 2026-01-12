```yaml
number: 18642
title: "[ty] Fix panics when pulling types for various special forms that have the wrong number of parameters"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/pulltypes-panics
created_at: 2025-06-12T10:44:32Z
updated_at: 2025-06-17T09:40:52Z
url: https://github.com/astral-sh/ruff/pull/18642
synced_at: 2026-01-12T15:56:23Z
```

# [ty] Fix panics when pulling types for various special forms that have the wrong number of parameters

---

_@AlexWaygood_

## Summary

On `main` we panic if we try to "pull types" for a file that includes a certain parameterized special form with the wrong number of arguments. This can cause unpredictable crashes in the playground, for example:

https://github.com/user-attachments/assets/e531d821-714e-433b-b9dd-b1b75b8c06d1

## Test Plan

Mdtests (which now pull types automatically if they don't have the `<!-- pull-types:skip -->` directive in them, following https://github.com/astral-sh/ruff/pull/18539)


---

_Review requested from @carljm by @AlexWaygood on 2025-06-12 10:44_

---

_Label `ty` added by @AlexWaygood on 2025-06-12 10:44_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-12 10:44_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-12 10:44_

---

_Comment by @github-actions[bot] on 2025-06-12 10:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `bug` added by @AlexWaygood on 2025-06-16 14:53_

---

_@carljm approved on 2025-06-17 04:54_

---

_Merged by @AlexWaygood on 2025-06-17 09:40_

---

_Closed by @AlexWaygood on 2025-06-17 09:40_

---

_Branch deleted on 2025-06-17 09:40_

---
