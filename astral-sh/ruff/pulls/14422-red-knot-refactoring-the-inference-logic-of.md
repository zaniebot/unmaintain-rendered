```yaml
number: 14422
title: "[red-knot] Refactoring the inference logic of lexicographic comparisons"
type: pull_request
state: merged
author: cake-monotone
labels:
  - great writeup
  - ty
assignees: []
merged: true
base: main
head: refactor-lexicographic-compares-inference
created_at: 2024-11-18T03:25:55Z
updated_at: 2024-11-20T01:32:43Z
url: https://github.com/astral-sh/ruff/pull/14422
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Refactoring the inference logic of lexicographic comparisons

---

_Pull request opened by @cake-monotone on 2024-11-18 03:25_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

closes #14279

### Limitations of the Current Implementation
#### Incorrect Error Propagation

In the current implementation of lexicographic comparisons, if the result of an Eq operation is Ambiguous, the comparison stops immediately, returning a bool instance. While this may yield correct inferences, it fails to capture unsupported-operation errors that might occur in subsequent comparisons.
```py
class A: ...

(int_instance(), A()) < (int_instance(), A())  # should error
```

#### Weak Inference in Specific Cases

> Example: `(int_instance(), "foo") == (int_instance(), "bar")`
> Current result: `bool`
> Expected result: `Literal[False]`

`Eq` and `NotEq` have unique behavior in lexicographic comparisons compared to other operators. Specifically:
- For `Eq`, if any non-equal pair exists within the tuples being compared, we can immediately conclude that the tuples are not equal.
- For `NotEq`, if any equal pair exists, we can conclude that the tuples are unequal.

```py
a = (str_instance(), int_instance(), "foo")

reveal_type(a == a)  # revealed: bool
reveal_type(a != a)  # revealed: bool

b = (str_instance(), int_instance(), "bar")

reveal_type(a == b)  # revealed: bool  # should be Literal[False]
reveal_type(a != b)  # revealed: bool  # should be Literal[True]
```
#### Incorrect Support for Non-Boolean Rich Comparisons

In CPython, aside from `==` and `!=`, tuple comparisons return a non-boolean result as-is. Tuples do not convert the value into `bool`.

Note: If all pairwise `==` comparisons between elements in the tuples return Truthy, the comparison then considers the tuples' lengths. Regardless of the return type of the dunder methods, the final result can still be a boolean.

```py
from __future__ import annotations

class A:
    def __eq__(self, o: object) -> str:
        return "hello"

    def __ne__(self, o: object) -> bytes:
        return b"world"

    def __lt__(self, o: A) -> float:
        return 3.14

a = (A(), A())

reveal_type(a == a)  # revealed: bool
reveal_type(a != a)  # revealed: bool
reveal_type(a < a)  # revealed: bool # should be: `float | Literal[False]`

```

### Key Changes
One of the major changes is that comparisons no longer end with a `bool` result when a pairwise `Eq` result is `Ambiguous`. Instead, the function attempts to infer all possible cases and unions the results. This improvement allows for more robust type inference and better error detection.

Additionally, as the function is now optimized for tuple comparisons, the name has been changed from the more general `infer_lexicographic_comparison` to `infer_tuple_rich_comparison`.

## Test Plan

mdtest included


---

_Review requested from @carljm by @cake-monotone on 2024-11-18 03:25_

---

_Review requested from @MichaReiser by @cake-monotone on 2024-11-18 03:25_

---

_Review requested from @AlexWaygood by @cake-monotone on 2024-11-18 03:25_

---

_Review requested from @sharkdp by @cake-monotone on 2024-11-18 03:25_

---

_Comment by @github-actions[bot] on 2024-11-18 03:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @dhruvmanila on 2024-11-18 04:53_

---

_Label `great writeup` added by @AlexWaygood on 2024-11-18 07:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:204 on 2024-11-18 18:24_

Is this supposed to say "if any non-equal pair exists"?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:213 on 2024-11-18 18:25_

I don't think these paragraphs add value to the tests.

---

_@carljm approved on 2024-11-18 18:30_

Excellent! Very clear write-up and fix, thank you!!

---

_@cake-monotone reviewed on 2024-11-19 03:10_

---

_Review comment by @cake-monotone on `crates/red_knot_python_semantic/resources/mdtest/comparison/tuples.md`:204 on 2024-11-19 03:10_

You're right, my mistake! 😅 Thanks for catching that.

---

_Comment by @carljm on 2024-11-20 01:32_

Thank you!! So great.

---

_Merged by @carljm on 2024-11-20 01:32_

---

_Closed by @carljm on 2024-11-20 01:32_

---
