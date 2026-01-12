```yaml
number: 18824
title: "[ty] Fix panics when pulling types for `ClassVar` or `Final` parameterized with >1 argument"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: pulltypes-panics
created_at: 2025-06-20T16:43:29Z
updated_at: 2025-06-20T17:06:42Z
url: https://github.com/astral-sh/ruff/pull/18824
synced_at: 2026-01-12T15:56:26Z
```

# [ty] Fix panics when pulling types for `ClassVar` or `Final` parameterized with >1 argument

---

_@AlexWaygood_

## Summary

On `main` we panic if we try to "pull types" for a file that includes a parameterized `Final` or `ClassVar` with the wrong number of arguments. This can cause unpredictable crashes in the playground in the same way as the video I recorded in https://github.com/astral-sh/ruff/pull/18642#issue-3139766135

## Test Plan

Mdtests


---

_Review requested from @carljm by @AlexWaygood on 2025-06-20 16:43_

---

_Label `bug` added by @AlexWaygood on 2025-06-20 16:43_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-20 16:43_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-20 16:43_

---

_Label `ty` added by @AlexWaygood on 2025-06-20 16:43_

---

_Comment by @github-actions[bot] on 2025-06-20 16:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-06-20 16:46_

---

_Merged by @AlexWaygood on 2025-06-20 17:06_

---

_Closed by @AlexWaygood on 2025-06-20 17:06_

---

_Branch deleted on 2025-06-20 17:06_

---
