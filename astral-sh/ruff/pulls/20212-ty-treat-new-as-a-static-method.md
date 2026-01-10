```yaml
number: 20212
title: "[ty] Treat `__new__` as a static method"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/new-staticmethod
created_at: 2025-09-03T14:02:47Z
updated_at: 2025-09-03T17:55:21Z
url: https://github.com/astral-sh/ruff/pull/20212
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Treat `__new__` as a static method

---

_Pull request opened by @sharkdp on 2025-09-03 14:02_

## Summary

Pull this out of https://github.com/astral-sh/ruff/pull/18473 as an isolated change to make sure it has no adverse effects.

The wrong behavior is observable on `main` for something like
```py
class C:
    def __new__(cls) -> "C":
        cls.x = 1

C.x  # previously: Attribute `x` can only be accessed on instances
     # now:        Type `<class 'C'>` has no attribute `x`
```
where we currently treat `x` as an *instance* attribute (because we consider `__new__` to be a normal function and `cls` to be the "self" attribute). With this PR, we do not consider `x` to be an attribute, neither on the class nor on instances of `C`. If this turns out to be an important feature, we should add it intentionally, instead of accidentally.

## Test Plan

Ecosystem checks.

---

_Label `ty` added by @sharkdp on 2025-09-03 14:02_

---

_Comment by @github-actions[bot] on 2025-09-03 14:04_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-03 14:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-09-03 14:08_

---

_Review requested from @carljm by @sharkdp on 2025-09-03 14:08_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-03 14:08_

---

_Review requested from @dcreager by @sharkdp on 2025-09-03 14:08_

---

_@carljm approved on 2025-09-03 15:51_

---

_@AlexWaygood approved on 2025-09-03 17:10_

---

_@dcreager approved on 2025-09-03 17:54_

---

_Merged by @sharkdp on 2025-09-03 17:55_

---

_Closed by @sharkdp on 2025-09-03 17:55_

---

_Branch deleted on 2025-09-03 17:55_

---
