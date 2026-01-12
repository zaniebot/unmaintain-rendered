```yaml
number: 17086
title: "[red-knot] Add `ParamSpecArgs` and `ParamSpecKwargs` as `KnownClass`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/add-paramspecargs-and-kwargs
created_at: 2025-03-31T09:47:06Z
updated_at: 2025-03-31T10:52:24Z
url: https://github.com/astral-sh/ruff/pull/17086
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Add `ParamSpecArgs` and `ParamSpecKwargs` as `KnownClass`

---

_@sharkdp_

## Summary

In preparation for #17017, where we will need them to suppress new false positives (once we understand the `ParamSpec.args`/`ParamSpec.kwargs` properties).

## Test Plan

Tested on branch #17017

---

_Label `red-knot` added by @sharkdp on 2025-03-31 09:47_

---

_Review requested from @carljm by @sharkdp on 2025-03-31 09:47_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-31 09:47_

---

_Review requested from @dcreager by @sharkdp on 2025-03-31 09:47_

---

_Comment by @github-actions[bot] on 2025-03-31 09:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3112 on 2025-03-31 10:29_

I think it's somewhat likely that all of these TODOs would be resolved at once -- they are really a single feature. So I'd be inclined to simplify this to:

```suggestion
                Some(KnownClass::ParamSpec | KnownClass::ParamSpecArgs | KnownClass::ParamSpecKwargs) => Ok(todo_type!(
                    "Support for `typing.ParamSpec`"
                )),
```

---

_@AlexWaygood approved on 2025-03-31 10:29_

---

_Merged by @sharkdp on 2025-03-31 10:52_

---

_Closed by @sharkdp on 2025-03-31 10:52_

---

_Branch deleted on 2025-03-31 10:52_

---
