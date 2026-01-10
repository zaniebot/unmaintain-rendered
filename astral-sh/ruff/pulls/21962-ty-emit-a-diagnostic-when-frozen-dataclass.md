```yaml
number: 21962
title: "[ty] Emit a diagnostic when frozen dataclass inherits a non-frozen dataclass and the other way around"
type: pull_request
state: merged
author: silamon
labels:
  - ty
assignees: []
merged: true
base: main
head: frozen-non-frozen-diagnostics
created_at: 2025-12-13T16:52:37Z
updated_at: 2025-12-13T20:59:27Z
url: https://github.com/astral-sh/ruff/pull/21962
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Emit a diagnostic when frozen dataclass inherits a non-frozen dataclass and the other way around

---

_Pull request opened by @silamon on 2025-12-13 16:52_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Introduce a new diagnostic that is raised in the following situations: 
* Checks for non-frozen dataclasses that inherit from frozen dataclasses.
* Checks for frozen dataclasses that inherit from non-frozen dataclasses.

## Test Plan

Tests have been added. 
The diagnostics include example codes to test. 



---

_Review requested from @carljm by @silamon on 2025-12-13 16:52_

---

_Review requested from @AlexWaygood by @silamon on 2025-12-13 16:52_

---

_Review requested from @sharkdp by @silamon on 2025-12-13 16:52_

---

_Review requested from @dcreager by @silamon on 2025-12-13 16:52_

---

_Renamed from "Raise a diagnostic when frozen dataclass inherits a non-frozen dataclass and the other way around" to "[ty] Emit a diagnostic when frozen dataclass inherits a non-frozen dataclass and the other way around" by @AlexWaygood on 2025-12-13 16:53_

---

_Label `ty` added by @AlexWaygood on 2025-12-13 16:53_

---

_Comment by @astral-sh-bot[bot] on 2025-12-13 16:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-13 20:51:37.689793991 +0000
+++ new-output.txt	2025-12-13 20:51:41.343814255 +0000
@@ -282,6 +282,8 @@
 dataclasses_final.py:38:1: error[invalid-assignment] Cannot assign to final attribute `final_with_default` on type `<class 'D'>`
 dataclasses_frozen.py:16:1: error[invalid-assignment] Property `a` defined in `DC1` is read-only
 dataclasses_frozen.py:17:1: error[invalid-assignment] Property `b` defined in `DC1` is read-only
+dataclasses_frozen.py:23:7: error[invalid-frozen-dataclass-subclass] Non-frozen dataclass `DC2` cannot inherit from frozen dataclass `DC1`
+dataclasses_frozen.py:33:7: error[invalid-frozen-dataclass-subclass] Frozen dataclass `DC4` cannot inherit from non-frozen dataclass `DC3`
 dataclasses_kwonly.py:23:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_kwonly.py:38:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_kwonly.py:53:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
@@ -342,6 +344,7 @@
 dataclasses_transform_func.py:70:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_func.py:70:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_func.py:76:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@create_model_frozen`
+dataclasses_transform_func.py:89:7: error[invalid-frozen-dataclass-subclass] Non-frozen dataclass `Customer3Subclass` cannot inherit from frozen dataclass `Customer3`
 dataclasses_transform_func.py:96:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
 dataclasses_transform_meta.py:63:1: error[invalid-assignment] Property `id` defined in `Customer1` is read-only
 dataclasses_transform_meta.py:66:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
@@ -1022,4 +1025,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1024 diagnostics
+Found 1027 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-13 16:56_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ tests/test_functional.py:84:7: error[invalid-frozen-dataclass-subclass] Non-frozen dataclass `SubFrozen` cannot inherit from frozen dataclass `Frozen`
+ tests/test_next_gen.py:252:15: error[invalid-frozen-dataclass-subclass] Non-frozen dataclass `C` cannot inherit from frozen dataclass `B`
+ tests/test_next_gen.py:297:19: error[invalid-frozen-dataclass-subclass] Non-frozen dataclass `C` cannot inherit from frozen dataclass `A`
- Found 605 diagnostics
+ Found 608 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5136 diagnostics
+ Found 5137 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:2279 on 2025-12-13 16:58_

we should indeed catch this error, but not for the reason you describe in the documentation here 😄 We should catch this error because it raises an exception at runtime!

```pycon
>>> from dataclasses import dataclass
>>> @dataclass(frozen=True)
... class Foo:
...     x: int
...     
>>> @dataclass
... class Bar(Foo):
...     y: str
...     
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    @dataclass
     ^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1305, in dataclass
    return wrap(cls)
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1295, in wrap
    return _process_class(cls, init, repr, eq, order, unsafe_hash,
                          frozen, match_args, kw_only, slots,
                          weakref_slot)
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1038, in _process_class
    raise TypeError('cannot inherit non-frozen dataclass from a '
                    'frozen one')
TypeError: cannot inherit non-frozen dataclass from a frozen one
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:2250 on 2025-12-13 17:01_

Same here. I think we should definitely emit a diagnostic if we see this. But the documentation should just say that it raises an exception at runtime; there's no need to start talking abstractly about contracts or anything like that :-)

```pycon
>>> from dataclasses import dataclass
>>> @dataclass
... class Foo:
...     x: int
...
>>> @dataclass(frozen=True)
... class Bar(Foo):
...     y: str
...     
Traceback (most recent call last):
  File "<python-input-8>", line 1, in <module>
    @dataclass(frozen=True)
     ~~~~~~~~~^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1295, in wrap
    return _process_class(cls, init, repr, eq, order, unsafe_hash,
                          frozen, match_args, kw_only, slots,
                          weakref_slot)
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1043, in _process_class
    raise TypeError('cannot inherit frozen dataclass from a '
                    'non-frozen one')
TypeError: cannot inherit frozen dataclass from a non-frozen one
```

---

_@AlexWaygood reviewed on 2025-12-13 17:02_

Nice, we should definitely emit these diagnostics! These always raise exceptions at runtime, so it's a great thing for a type checker to catch.

But let's improve the documentation and add some tests.

---

_Comment by @AlexWaygood on 2025-12-13 17:16_

The typing conformance suite results are great! They show us now emitting diagnostics on several places where we are meant to, but where we previously didn't.

The mypy_primer results show us now emitting three false-positive diagnostics on `attrs`. `attrs` has slightly different semantics to the stdlib `dataclasses` module here, apparently. For the stdlib `dataclasses` module, you _have_ to explicitly pass `frozen=True` for a `@dataclass` subclass of a `frozen=True` `@dataclass` superclass. But for `attrs`, any subclass of a `frozen=True` `@attr.s` superclass is implicitly frozen.

The fact that we now emit a few false positives on `attrs` is a bit unfortunate, but I think it's fine. I don't think you need to worry about it here. We might add some special-cased support for `attrs` in the future, but until that point it's the right thing for us to just assume that `attrs` classes have the exact same semantics as stdlib dataclasses.

---

_Comment by @AlexWaygood on 2025-12-13 18:14_

Maybe these should just be one error code? `invalid-frozen-dataclass-subclass`?

---

_Review requested from @MichaReiser by @silamon on 2025-12-13 18:47_

---

_@AlexWaygood approved on 2025-12-13 20:52_

Thanks!

---

_Merged by @AlexWaygood on 2025-12-13 20:59_

---

_Closed by @AlexWaygood on 2025-12-13 20:59_

---
