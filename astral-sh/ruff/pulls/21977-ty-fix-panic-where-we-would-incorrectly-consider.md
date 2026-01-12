```yaml
number: 21977
title: "[ty] Fix panic where we would incorrectly consider overloads in another file as belonging to a function in the file being checked"
type: pull_request
state: open
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: alex/panicky-overloads
created_at: 2025-12-14T16:36:51Z
updated_at: 2025-12-15T18:23:57Z
url: https://github.com/astral-sh/ruff/pull/21977
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Fix panic where we would incorrectly consider overloads in another file as belonging to a function in the file being checked

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1867. In certain cases where a function definition has not yet been completed, we can end up associating overloads from a totally different function (in a totally different file) with the unfinished function in the file being checked. That then leads us to panic in other places that assume (reasonably) that a function's overloads will all always be defined in a single file.

This PR fixes the panic by adding more sanity checks to `OverloadLiteral::previous_overload()`.

## Test Plan

Added a regression test that causes us to panic on `main`.


---

_Review requested from @carljm by @AlexWaygood on 2025-12-14 16:36_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-14 16:36_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-14 16:36_

---

_Label `bug` added by @AlexWaygood on 2025-12-14 16:36_

---

_Label `ty` added by @AlexWaygood on 2025-12-14 16:36_

---

_Comment by @astral-sh-bot[bot] on 2025-12-14 16:38_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-14 16:41_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@MichaReiser reviewed on 2025-12-15 07:22_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/function.rs`:390 on 2025-12-15 07:22_

I'm not sure I understand why they need `invalid_syntax` to reproduce. 

Is the `name` assertion necessary or do we need the file assertion anyway? What if we have something like this:


```py
import module
f = module.f
@staticmethod
def f  # error: [invalid-syntax]
```



---

_@MichaReiser reviewed on 2025-12-15 07:23_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/function.rs`:394 on 2025-12-15 07:23_

Maybe just consider comparing if `self.body_scope(db) == previous_overload.body_scope()` because I don't think we ever want to merge two overloads from different scopes

---

_@dhruvmanila reviewed on 2025-12-15 07:55_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/overloads.md`:751 on 2025-12-15 07:55_

I don't think it requires the code to have a syntax error as the following also panics:

```py
foo = next


@staticmethod
def foo(): ...
```

But, if I remove the `staticmethod` decorator, then it no longer panics.

---

_@dhruvmanila reviewed on 2025-12-15 08:10_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/overloads.md`:751 on 2025-12-15 08:10_

I think I see why this is happening and why it doesn't produce a panic when `staticmethod` is removed.

The type of `foo` in the assignment statement is `FunctionLiteral` (as you've noted as well in the issue thread) because `foo` is considered as an implicit type alias, and here the `def foo` is marked as an implementation to the overloaded `next` builtin. So, when the diagnostic is trying to get the range because `@staticmethod` is not used consistently across all overloads and implementation, it panics as the overloads are defined in a different file.

If I remove the `staticmethod`, you can clearly see this:

```py
foo = next


# @staticmethod
def foo(): ...

# revealed: Overload[(i: SupportsNext[_T@next], /) -> _T@next, (i: SupportsNext[_T@next], default: _VT@next, /) -> _T@next | _VT@next]
reveal_type(foo)
```

I'd update the test case to avoid syntax error and add this test case (without the `@staticmethod` decorator) to make sure the reveal type is correct.

---

_@dhruvmanila reviewed on 2025-12-15 08:12_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/function.rs`:394 on 2025-12-15 08:12_

Yeah, I think just checking the scope should be fine 👍 

---

_Renamed from "[ty] Fix panic on invalid syntax where we would incorrectly consider overloads in another file as belonging to a function in the file being checked" to "[ty] Fix panic where we would incorrectly consider overloads in another file as belonging to a function in the file being checked" by @AlexWaygood on 2025-12-15 16:49_

---

_@AlexWaygood reviewed on 2025-12-15 18:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:390 on 2025-12-15 18:23_

Ah yeah, thanks, figured out a repro without invalid syntax.

I think the name assertion is also necessary, I'll add another test

---

_Converted to draft by @AlexWaygood on 2025-12-15 18:23_

---
