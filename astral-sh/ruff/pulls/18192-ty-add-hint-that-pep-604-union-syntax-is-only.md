```yaml
number: 18192
title: "[ty] Add hint that PEP 604 union syntax is only available in 3.10+"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: david/pep604-union-diagnostic-hint
created_at: 2025-05-19T08:12:15Z
updated_at: 2025-05-19T17:47:33Z
url: https://github.com/astral-sh/ruff/pull/18192
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Add hint that PEP 604 union syntax is only available in 3.10+

---

_Pull request opened by @sharkdp on 2025-05-19 08:12_

## Summary

Add a new diagnostic hint if you try to use PEP 604 `X | Y` union syntax in a non-type-expression before 3.10:

![image](https://github.com/user-attachments/assets/e29b9335-3273-439f-9d7c-1154b6d00687)

closes https://github.com/astral-sh/ty/issues/437

## Test Plan

New snapshot test

---

_Review requested from @carljm by @sharkdp on 2025-05-19 08:12_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-19 08:12_

---

_Review requested from @dcreager by @sharkdp on 2025-05-19 08:12_

---

_Label `ty` added by @sharkdp on 2025-05-19 08:12_

---

_Label `diagnostics` added by @sharkdp on 2025-05-19 08:12_

---

_Comment by @github-actions[bot] on 2025-05-19 08:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp reviewed on 2025-05-19 08:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:5903 on 2025-05-19 08:17_

One could certainly construct cases where we emit this info even if the user is trying to do something that does not involve type hints, but it seems unlikely? And not too distracting even if?

On the contrary, there are probably also cases where we fail to emit this info due to the class-literal filter above. We could think about relaxing this (e.g. checking if `type` is in the MRO of both `left_ty` and `right_ty`, or further trying to detect other types that might appear in those positions), but I'm not sure if it's worth it?

---

_@MichaReiser reviewed on 2025-05-19 08:31_

Sweet. I let someone else answer your comment around "filtering".

It would be nice if we could also show the actual python version, similar to https://github.com/astral-sh/ruff/pull/18068

---

_Review comment by @danielhollas on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:104 on 2025-05-19 10:54_

Should there be a test case for stringified annotations (`from __future__ import annotations`)?

---

_@danielhollas reviewed on 2025-05-19 10:54_

---

_@sharkdp reviewed on 2025-05-19 14:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:104 on 2025-05-19 14:34_

I don't understand how that's related to the functionality being added here? `int | str` in *non-type-expressions* is an error in Python 3.9 and earlier, no matter if annotations are deferred or not, right?

---

_@danielhollas reviewed on 2025-05-19 14:45_

---

_Review comment by @danielhollas on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:104 on 2025-05-19 14:45_

if `from __future__ import` is present at the top of the file then all annotations are stringified, and so they should not error even in 3.9.

---

_@sharkdp reviewed on 2025-05-19 14:48_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:104 on 2025-05-19 14:48_

In annotations, yes. But not in non-type expressions: 
```pycon
▶ uv run -p 3.9 python
Python 3.9.20 (main, Oct 16 2024, 04:36:33) 
[Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from __future__ import annotations
>>> def f(x: int | str): ...
... 
>>> A = int | str
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for |: 'type' and 'type'
```

---

_@carljm reviewed on 2025-05-19 14:55_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:104 on 2025-05-19 14:55_

I think it would be clearer to say "non-annotations" than "non-type-expressions", since (with implicit type alias support, which we don't fully have yet), `int | str` in `A = int | str` should be evaluated as a type expression -- but it will still fail at runtime in older Python versions, regardless of `from __future__ import annotations`, because it is not syntactically an _annotation_, and `from __future__ import annotations` affects only _annotations_.

---

_@AlexWaygood reviewed on 2025-05-19 14:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:5900 on 2025-05-19 14:57_

I think we could probably add the note whenever `left_ty` and `right_ty` are both subtypes of `KnownClass::Type.to_instance(db)`? `is_assignable_to` would not be appropriate here because e.g. `Any` is assignable to `type`, but `is_subtype_of` should be safe

---

_@carljm reviewed on 2025-05-19 14:59_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:104 on 2025-05-19 14:59_

(This will probably also add complexity to the implementation to keep these tests passing when we improve our support for implicit type aliases, because we might need to evaluate these both as value expressions -- in order to get these diagnostics -- and as type expressions -- in order to know how to interpret them when used as a type alias. )

---

_@danielhollas reviewed on 2025-05-19 15:00_

---

_Review comment by @danielhollas on `crates/ty_python_semantic/resources/mdtest/assignment/annotations.md`:104 on 2025-05-19 15:00_

I am sorry, I should have read the PR description more closely. The linked issue talks about the union syntax in function definitions so I got confused. I am assuming that error messages coming from type expressions are coming in a separate PR? (ie. to address astral-sh/ty#449). 

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:5899 on 2025-05-19 15:03_

I'm not sure we need to check the RHS type at all here. There could be cases like `int | foo` where `foo` is something other than a class literal type where this runtime error would still occur, and this hint would be just as useful.

---

_@carljm approved on 2025-05-19 15:04_

Thank you!

---

_@AlexWaygood reviewed on 2025-05-19 15:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:5899 on 2025-05-19 15:08_

Yes, if either the l.h.s. or the r.h.s. is a subtype of `type`, it should be fine to display the hint. `TypeVar("T") | int` succeeds on Python 3.10+ but not on earlier versions

---

_Merged by @sharkdp on 2025-05-19 17:47_

---

_Closed by @sharkdp on 2025-05-19 17:47_

---

_Branch deleted on 2025-05-19 17:47_

---
