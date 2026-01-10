```yaml
number: 20317
title: "[ty] `\"foo\".startswith` is not an instance of `types.MethodWrapperType`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/builtin-bound-methods
created_at: 2025-09-09T16:26:18Z
updated_at: 2025-09-10T11:15:22Z
url: https://github.com/astral-sh/ruff/pull/20317
synced_at: 2026-01-10T17:46:22Z
```

# [ty] `"foo".startswith` is not an instance of `types.MethodWrapperType`

---

_Pull request opened by @AlexWaygood on 2025-09-09 16:26_

## Summary

This PR fixes various minor inaccuracies associated with our `Type::MethodWrapper` variant.

Most notably, due to a bug in this line here, we do not currently recognise `Type::MethodWrapper()` types as being subtypes of `types.MethodWrapper`! This should be `KnownClass::MethodWrapperType` rather than `KnownClass::WrapperDescriptorType`:

https://github.com/astral-sh/ruff/blob/9cdac2d6fb65f8d4d2818590cbfb14915003b8d7/crates/ty_python_semantic/src/types.rs#L1664-L1666

But there's also the fact that while `f.__get__` (for any function `f`) is an instance of `types.MethodWrapperType` at runtime, `"foo".startswith` is not -- it's actually an instance of `types.BuiltinFunctionType`:

```pycon
>>> import types
>>> def f(): ...
... 
>>> isinstance(f.__get__, types.MethodWrapperType)
True
>>> isinstance("foo".startswith, types.MethodWrapperType)
False
>>> isinstance("foo".startswith, types.BuiltinFunctionType)
True
```

This PR therefore renames the `Type` variant entirely to `Type::KnownBoundMethod` (reflecting the fact that not all inhabitants of these types are instances of `types.MethodWrapperType` at runtime), and reworks our logic so that attribute access and subtyping for the `StrStartsWith` variant fall back to attribute access/subtyping on `types.BuiltinFunctionType` rather than attribute access/subtyping on `types.MethodWrapperType`.

## Test Plan

Mdtests extended


---

_Review requested from @carljm by @AlexWaygood on 2025-09-09 16:26_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-09 16:26_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-09 16:26_

---

_Label `ty` added by @AlexWaygood on 2025-09-09 16:26_

---

_Comment by @github-actions[bot] on 2025-09-09 16:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-09 16:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Assigned to @sharkdp by @sharkdp on 2025-09-09 19:11_

---

_@carljm approved on 2025-09-09 21:17_

---

_@sharkdp approved on 2025-09-10 07:57_

Thank you — good catch!

---

_Merged by @AlexWaygood on 2025-09-10 11:14_

---

_Closed by @AlexWaygood on 2025-09-10 11:14_

---

_Branch deleted on 2025-09-10 11:14_

---
