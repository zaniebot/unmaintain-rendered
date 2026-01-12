```yaml
number: 19981
title: "[ty] fix unpacking a type alias with detailed tuple spec"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/alias-iterate
created_at: 2025-08-18T23:47:31Z
updated_at: 2025-08-19T00:54:07Z
url: https://github.com/astral-sh/ruff/pull/19981
synced_at: 2026-01-12T15:56:51Z
```

# [ty] fix unpacking a type alias with detailed tuple spec

---

_@carljm_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1046

We special-case iteration of certain types because they may have a more detailed tuple-spec. Now that type aliases are a distinct type variant, we need to handle them as well.

I don't love that `Type::TypeAlias` means we have to remember to add a case for it basically anywhere we are special-casing a certain kind of type, but at the moment I don't have a better plan. It's another argument for avoiding fallback cases in `Type` matches, which we usually prefer; I've updated this match statement to be comprehensive.

## Test Plan

Added mdtest.


---

_Label `ty` added by @carljm on 2025-08-18 23:47_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-18 23:47_

---

_Review requested from @sharkdp by @carljm on 2025-08-18 23:47_

---

_Review requested from @dcreager by @carljm on 2025-08-18 23:47_

---

_Comment by @github-actions[bot] on 2025-08-18 23:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-18 23:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-08-19 00:04_

---

_Merged by @carljm on 2025-08-19 00:54_

---

_Closed by @carljm on 2025-08-19 00:54_

---

_Branch deleted on 2025-08-19 00:54_

---
