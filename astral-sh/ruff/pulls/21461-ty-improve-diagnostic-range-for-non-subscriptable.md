```yaml
number: 21461
title: "[ty] Improve diagnostic range for `non-subscriptable` diagnostics"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex-brent/subscript-diagnostic-range
created_at: 2025-11-14T18:14:58Z
updated_at: 2025-11-14T21:15:16Z
url: https://github.com/astral-sh/ruff/pull/21461
synced_at: 2026-01-10T16:53:56Z
```

# [ty] Improve diagnostic range for `non-subscriptable` diagnostics

---

_Pull request opened by @AlexWaygood on 2025-11-14 18:14_

## Summary

Currently our diagnostic only covers the range of the thing being subscripted:

<img width="1702" height="312" alt="image" src="https://github.com/user-attachments/assets/7e630431-e846-46ca-93c1-139f11aaba11" />

But it should probably cover the _whole_ subscript expression (arguably the more "incorrect" bit is the `["foo"]` part of this expression, not the `x` part of this expression!)

## Test Plan

Added a snapshot

Co-authored-by: Brent Westbrook <36778786+ntBre@users.noreply.github.com>

---

_Review requested from @carljm by @AlexWaygood on 2025-11-14 18:14_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-14 18:14_

---

_Label `ty` added by @AlexWaygood on 2025-11-14 18:14_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-14 18:15_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-14 18:15_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 18:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 18:18_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pandas (https://github.com/pandas-dev/pandas)
- pandas/tests/series/test_arithmetic.py:646:17: error[non-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
+ pandas/tests/series/test_arithmetic.py:646:16: error[non-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
- pandas/tests/series/test_arithmetic.py:647:21: error[non-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method
+ pandas/tests/series/test_arithmetic.py:647:20: error[non-subscriptable] Cannot subscript object of type `bool` with no `__getitem__` method

sympy (https://github.com/sympy/sympy)
- sympy/parsing/autolev/test-examples/ruletest6.py:31:6: error[non-subscriptable] Cannot subscript object of type `<module 'math'>` with no `__getitem__` method
+ sympy/parsing/autolev/test-examples/ruletest6.py:31:5: error[non-subscriptable] Cannot subscript object of type `<module 'math'>` with no `__getitem__` method


```

</details>


No memory usage changes detected ✅



---

_Review requested from @MichaReiser by @AlexWaygood on 2025-11-14 18:21_

---

_Comment by @carljm on 2025-11-14 21:01_

I think highlighting the whole expression (as you do here) is best. It is both the type of the base object (which is the type that is not subscriptable), and the fact that you are subscripting it, that's "at fault" for the error. (In the case of not-subscriptable, the actual key is arguably not involved at all! But it still makes sense to highlight the entire expression IMO.)

---

_@carljm approved on 2025-11-14 21:01_

---

_Comment by @MichaReiser on 2025-11-14 21:06_

Similar to attribute expressions, I think it's undesired to highlight the entire object because it can get very noisy if that expression is long (think a call expression)

I think the ideal is to highlight .attribute or [value].

Related discussion https://github.com/astral-sh/ty/issues/169

I don't think this should block this PR because it at least makes it consistent but we should update said issue to also mention subscript

---

_Comment by @AlexWaygood on 2025-11-14 21:15_

Heh, well, I disagree for the same reasons I gave in https://github.com/astral-sh/ty/issues/169 ;) but I'll add a note in that issue to mention that the same discussion also applies to subscript diagnostics!

---

_Merged by @AlexWaygood on 2025-11-14 21:15_

---

_Closed by @AlexWaygood on 2025-11-14 21:15_

---

_Branch deleted on 2025-11-14 21:15_

---
