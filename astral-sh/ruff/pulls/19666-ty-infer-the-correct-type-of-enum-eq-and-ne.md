```yaml
number: 19666
title: "[ty] Infer the correct type of Enum `__eq__` and `__ne__` comparisions"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: correct-enum-eq
created_at: 2025-07-31T18:05:01Z
updated_at: 2025-11-06T11:48:18Z
url: https://github.com/astral-sh/ruff/pull/19666
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Infer the correct type of Enum `__eq__` and `__ne__` comparisions

---

_Pull request opened by @MatthewMckee4 on 2025-07-31 18:05_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/920

## Test Plan

Update `enums.md`


---

_Comment by @github-actions[bot] on 2025-07-31 18:06_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-31 18:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MatthewMckee4 on 2025-07-31 18:09_

It seems like we are losing a lot of information with `CompareUnsupportedError`. It seems like this should also hold `CallError`

---

_Comment by @MatthewMckee4 on 2025-07-31 18:13_

We also don't seem to have any tests where `__eq__` and similar are set to `None`

---

_Renamed from "Infer the correct type of Enum `__eq__` and `__ne__` comparisions" to "[ty] Infer the correct type of Enum `__eq__` and `__ne__` comparisions" by @AlexWaygood on 2025-07-31 18:23_

---

_Label `bug` added by @AlexWaygood on 2025-07-31 18:23_

---

_Label `ty` added by @AlexWaygood on 2025-07-31 18:23_

---

_Marked ready for review by @MatthewMckee4 on 2025-08-03 10:50_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-08-03 10:50_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-08-03 10:50_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-08-03 10:50_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-08-03 10:50_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:8412 on 2025-08-04 08:09_

I'm not sure I understand this?

---

_@sharkdp reviewed on 2025-08-04 08:09_

---

_@MatthewMckee4 reviewed on 2025-08-04 14:06_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/infer.rs`:8412 on 2025-08-04 14:06_

I thought it was about synthesizing the eq and ne methods from object. And we don't want to do that if the policy says no object 

---

_Review request for @carljm removed by @carljm on 2025-08-05 03:33_

---

_@sharkdp reviewed on 2025-08-18 14:36_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:8412 on 2025-08-18 14:36_

> I thought it was about synthesizing the eq and ne methods from object

It's about *looking up* these methods on class `object`. But yes, I see that this makes sense now. This branch implements behavior that originates from `object.__eq__`/`object.__ne__`, so we need to make an exception if the lookup-policy tells us to skip attributes on objects. Thank you

---

_@sharkdp approved on 2025-08-18 14:38_

Thank you!

---

_Merged by @sharkdp on 2025-08-18 17:45_

---

_Closed by @sharkdp on 2025-08-18 17:45_

---

_Branch deleted on 2025-11-06 11:48_

---
