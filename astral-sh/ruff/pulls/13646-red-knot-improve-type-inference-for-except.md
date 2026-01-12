```yaml
number: 13646
title: "[red-knot] Improve type inference for except handlers where a tuple of exception classes is caught"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: exception-tuples
created_at: 2024-10-06T14:55:59Z
updated_at: 2024-10-07T16:52:45Z
url: https://github.com/astral-sh/ruff/pull/13646
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Improve type inference for except handlers where a tuple of exception classes is caught

---

_@AlexWaygood_

## Summary

There's still a bunch of stuff that is valid in an exception handler but that we can't do type inference for until we have generalised support for `type[T]`. But this improves our type inference for some common cases involving exception tuples. It seems like a nice little short-term win

## Test plan

I fixed the TODO in the existing test `except_handler_multiple_exceptions`, and added a new test with some new TODOs to illustrate where our type inference still falls short.

---

_Label `red-knot` added by @AlexWaygood on 2024-10-06 14:55_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-06 14:55_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-06 14:56_

---

_@AlexWaygood reviewed on 2024-10-06 15:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6203 on 2024-10-06 15:00_

Cc. @charliermarsh -- this is a little edge case in the logic you added in #13579. I think the special-casing that allows e.g. `type[int]` at runtime is buried deep in the interpreter somewhere. According to all the normal rules, it _shouldn't_ be subscriptable... but it is!

```pycon
Python 3.12.4 (main, Jun 12 2024, 09:54:41) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> type[int]
type[int]
>>> type.__getitem__
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: type object 'type' has no attribute '__getitem__'. Did you mean: '__getstate__'?
>>> type.__class_getitem__
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: type object 'type' has no attribute '__class_getitem__'
>>> type.__dict__.keys()
dict_keys(['__new__', '__repr__', '__call__', '__getattribute__', '__setattr__', '__delattr__', '__init__', '__or__', '__ror__', 'mro', '__subclasses__', '__prepare__', '__instancecheck__', '__subclasscheck__', '__dir__', '__sizeof__', '__basicsize__', '__itemsize__', '__flags__', '__weakrefoffset__', '__base__', '__dictoffset__', '__name__', '__qualname__', '__bases__', '__mro__', '__module__', '__abstractmethods__', '__dict__', '__doc__', '__text_signature__', '__annotations__', '__type_params__'])
```

---

_Comment by @github-actions[bot] on 2024-10-06 15:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-10-07 15:10_

---

_Merged by @AlexWaygood on 2024-10-07 15:13_

---

_Closed by @AlexWaygood on 2024-10-07 15:13_

---

_Branch deleted on 2024-10-07 15:13_

---

_@carljm reviewed on 2024-10-07 15:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6203 on 2024-10-07 15:20_

Yeah this is literally hardcoded directly into `PyObject_GetItem` as a special case 🫣 

---

_@AlexWaygood reviewed on 2024-10-07 15:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6203 on 2024-10-07 15:25_

🙃

I'm working on a PR now to add the special case to our semantic model...

---

_@AlexWaygood reviewed on 2024-10-07 16:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6203 on 2024-10-07 16:52_

https://github.com/astral-sh/ruff/pull/13667#issue-2570969264

---
