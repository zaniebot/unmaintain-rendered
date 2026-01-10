```yaml
number: 20023
title: "[ty] Add precise iteration and unpacking inference for string literals and bytes literals"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/more-try-iterate
created_at: 2025-08-21T13:38:28Z
updated_at: 2025-08-22T18:33:10Z
url: https://github.com/astral-sh/ruff/pull/20023
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Add precise iteration and unpacking inference for string literals and bytes literals

---

_Pull request opened by @AlexWaygood on 2025-08-21 13:38_

## Summary

Previously we held off from doing this because we weren't sure that it was worth the added complexity cost. But our code has changed in the months since we made that initial decision, and I think the structure of the code is such that it no longer really leads to much added complexity to add precise inference when unpacking a string literal or a bytes literal.

The improved inference we gain from this has real benefits to users (see the mypy_primer report), and this PR doesn't appear to have a performance impact.

## Test plan

mdtests

---

_Label `ty` added by @AlexWaygood on 2025-08-21 13:38_

---

_Comment by @github-actions[bot] on 2025-08-21 13:40_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-21 13:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scipy (https://github.com/scipy/scipy)
- scipy/linalg/tests/test_lapack.py:171:41: warning[possibly-unresolved-reference] Name `ref` used when possibly not defined
- Found 6401 diagnostics
+ Found 6400 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-21 13:48_

> ```diff
> - scipy/linalg/tests/test_lapack.py:171:41: warning[possibly-unresolved-reference] Name `ref` used when possibly not defined
> ```

Okay, this one's pretty cool -- ty is now able to infer that all possibilities have been exhaustively covered here, meaning the variable cannot possibly be undefined by the time the code reaches the assertion at the bottom: https://github.com/scipy/scipy/blob/99c0ef6af161a4d8157cae5276a20c30b7677c6f/scipy/linalg/tests/test_lapack.py#L147-L171

---

_Renamed from "[ty] Add precise tuple specs when iterating over other kinds of types" to "[ty] Add precise unpacking support for string literals and bytes literals" by @AlexWaygood on 2025-08-22 11:48_

---

_Renamed from "[ty] Add precise unpacking support for string literals and bytes literals" to "[ty] Add precise iteration and unpacking inference for string literals and bytes literals" by @AlexWaygood on 2025-08-22 11:53_

---

_Marked ready for review by @AlexWaygood on 2025-08-22 12:38_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-22 12:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-22 12:38_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-22 12:38_

---

_Comment by @carljm on 2025-08-22 18:22_

> ty is now able to infer that all possibilities have been exhaustively covered here

This is a good example of the sort of issue that led us to disable `possibly-unresolved-reference` by default. Maybe at some point we'll be able to reconsider that!

---

_@carljm approved on 2025-08-22 18:28_

Nice!

---

_Merged by @AlexWaygood on 2025-08-22 18:33_

---

_Closed by @AlexWaygood on 2025-08-22 18:33_

---

_Branch deleted on 2025-08-22 18:33_

---
