```yaml
number: 19677
title: "[ty] support `kw_only=True` for `dataclass()` and `field()`"
type: pull_request
state: merged
author: thejchap
labels:
  - ty
assignees: []
merged: true
base: main
head: thejchap/kw-only
created_at: 2025-08-01T02:21:08Z
updated_at: 2025-08-14T15:02:55Z
url: https://github.com/astral-sh/ruff/pull/19677
synced_at: 2026-01-10T17:52:17Z
```

# [ty] support `kw_only=True` for `dataclass()` and `field()`

---

_Pull request opened by @thejchap on 2025-08-01 02:21_

## Summary
https://github.com/astral-sh/ty/issues/111

adds support for `@dataclass(kw_only=True)` (https://docs.python.org/3/library/dataclasses.html)

## Test Plan
- new mdtests
- triaged conformance diffs (notes here: https://diffswarm.dev/d-01k2gknwyq82f6x17zqf3apjxc)
- `mypy_primer` no-op

---

_Review requested from @carljm by @thejchap on 2025-08-01 02:21_

---

_Review requested from @AlexWaygood by @thejchap on 2025-08-01 02:21_

---

_Review requested from @sharkdp by @thejchap on 2025-08-01 02:21_

---

_Review requested from @dcreager by @thejchap on 2025-08-01 02:21_

---

_Comment by @github-actions[bot] on 2025-08-01 02:23_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-14 01:15:44.974868205 +0000
+++ new-output.txt	2025-08-14 01:15:45.042868491 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(48ad)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(124c6)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -212,13 +212,8 @@
 dataclasses_frozen.py:16:1: error[invalid-assignment] Property `a` defined in `DC1` is read-only
 dataclasses_frozen.py:17:1: error[invalid-assignment] Property `b` defined in `DC1` is read-only
 dataclasses_kwonly.py:23:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
-dataclasses_kwonly.py:32:1: error[missing-argument] No argument provided for required parameter `a`
-dataclasses_kwonly.py:32:5: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["hi"]`
-dataclasses_kwonly.py:35:1: error[missing-argument] No argument provided for required parameter `a`
-dataclasses_kwonly.py:35:5: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["hi"]`
-dataclasses_kwonly.py:35:11: error[parameter-already-assigned] Multiple values provided for parameter `b`
-dataclasses_kwonly.py:38:5: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["hi"]`
-dataclasses_kwonly.py:38:11: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Literal[1]`
+dataclasses_kwonly.py:38:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+dataclasses_kwonly.py:53:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_kwonly.py:61:1: error[missing-argument] No argument provided for required parameter `c`
 dataclasses_kwonly.py:61:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
 dataclasses_kwonly.py:61:14: error[parameter-already-assigned] Multiple values provided for parameter `b`
@@ -264,9 +259,14 @@
 dataclasses_transform_converter.py:121:11: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 7
 dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> Unknown`, found `def converter_simple(s: str) -> int`
 dataclasses_transform_field.py:49:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> @Todo`
+dataclasses_transform_field.py:75:16: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+dataclasses_transform_func.py:53:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
+dataclasses_transform_func.py:53:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_func.py:57:1: error[invalid-assignment] Object of type `Literal[3]` is not assignable to attribute `name` of type `str`
 dataclasses_transform_func.py:61:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
 dataclasses_transform_func.py:65:36: error[unknown-argument] Argument `salary` does not match any known parameter
+dataclasses_transform_func.py:71:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
+dataclasses_transform_func.py:71:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_func.py:77:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@create_model_frozen`
 dataclasses_transform_func.py:97:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
 dataclasses_transform_meta.py:60:36: error[unknown-argument] Argument `other_name` does not match any known parameter
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-01 02:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Converted to draft by @thejchap on 2025-08-01 02:32_

---

_Renamed from "[ty] upport kw_only=True for dataclasses" to "[ty] support kw_only=True for dataclasses" by @thejchap on 2025-08-01 02:38_

---

_Marked ready for review by @thejchap on 2025-08-01 02:40_

---

_Label `ty` added by @AlexWaygood on 2025-08-01 06:33_

---

_@sharkdp had review dismissed on 2025-08-01 07:20_

Thank you very much!

If you look at the typing conformance PR comment here (source code: https://github.com/python/typing/tree/main/conformance), you'll notice that this adds false positives when a single field opts out of `kw_only=True`:
```py
@dataclass(kw_only=True)
class DC3:
    a: str = field(kw_only=False)
    b: int = 0

DC3("hi")  # error on this branch
```

I think it shouldn't be *too* hard to add support for `kw_only` of single fields here as well. You can match on the `kw_only` parameter and add it as additional metadata to `FieldInstance`, similar to what we do for the `init` argument: https://github.com/astral-sh/ruff/blob/a3f28baab4fc085e1448484be23df7036d13191d/crates/ty_python_semantic/src/types/call/bind.rs#L921-L949. Let me know if you need help with that or would rather not like to work on that.

---

_Converted to draft by @thejchap on 2025-08-02 01:50_

---

_Comment by @thejchap on 2025-08-02 02:57_

implemented `kw_only` in `field` as well - need to take a look at the rest of the conformance diffs, will do that this week

---

_@thejchap reviewed on 2025-08-06 12:18_

---

_Review comment by @thejchap on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:228 on 2025-08-06 12:18_

i believe this should cover the `dataclasses_transform_func` [false positive](https://github.com/python/typing/blob/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance/tests/dataclasses_transform_func.py#L53)

---

_Marked ready for review by @thejchap on 2025-08-06 12:19_

---

_Review requested from @sharkdp by @thejchap on 2025-08-06 12:19_

---

_Renamed from "[ty] support kw_only=True for dataclasses" to "[ty] support kw_only=True for `dataclass()` and `field()`" by @thejchap on 2025-08-06 12:28_

---

_Renamed from "[ty] support kw_only=True for `dataclass()` and `field()`" to "[ty] support `kw_only=True` for `dataclass()` and `field()`" by @thejchap on 2025-08-06 13:05_

---

_Comment by @thejchap on 2025-08-08 12:49_

@sharkdp I think this is ready for another look

---

_Comment by @AlexWaygood on 2025-08-08 12:51_

David's on vacation until the end of next week, but we'll try to have someone else on the team take a look soon!

---

_@abhijeetbodas2001 reviewed on 2025-08-12 11:49_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types/class.rs`:1960 on 2025-08-12 11:49_

I could not find it specified in the spec, so I tested that unioning all `kw_only`'s is the right way here. It seems that `kw_only` param of `field` has a lower precedence than `dataclasses.KW_ONLY`, for example:

```py
[nav] In [8]: @dataclass
         ...: class C:
         ...:     _: KW_ONLY
         ...:     x: field(kw_only=False)

[nav] In [9]: C(x=3)
Out[9]: C(x=3)

[ins] In [10]: C(1)
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
Cell In[10], line 1
----> 1 C(1)

TypeError: C.__init__() takes 1 positional argument but 2 were given
```

I think it would be nice to have an mdtest simulating this.

---

_@thejchap reviewed on 2025-08-13 02:20_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/class.rs`:1960 on 2025-08-13 02:20_

@abhijeetbodas2001 thanks! fixed the precedence and added a test [here](https://github.com/astral-sh/ruff/pull/19677/files#diff-ee4187b91515150910d79ad2deba66ccd16357c926c5890fca0f31a002cee0a4R515)

---

_Comment by @thejchap on 2025-08-13 02:31_

> ## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
> Changes were detected when running ty on typing conformance tests

notes on the conformance diffs here: https://diffswarm.dev/d-01k2gknwyq82f6x17zqf3apjxc

---

_@abhijeetbodas2001 reviewed on 2025-08-13 10:46_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types/class.rs`:1960 on 2025-08-13 10:46_

Oh, no, sorry, I didn't mean to say that the earlier code was incorrect. I meant to say that it looks like it was correct (based on the code snippet I pasted above -- `x` can _only_ be a kw argument), and that we should add a test to make sure it stays correct.

---

_@thejchap reviewed on 2025-08-13 11:37_

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/class.rs`:1960 on 2025-08-13 11:37_

@abhijeetbodas2001 the below snippet passes `mypy` and works at runtime which makes me think the new precedence is correct (unless i'm missing something) - can you maybe clarify whats making you think the previous implementation was correct?

```py
from dataclasses import field, dataclass, KW_ONLY
from typing import reveal_type

@dataclass
class C:
    _: KW_ONLY
    x: int = field(kw_only=False)

C(x=1)
C(1)
reveal_type(C.__init__)
```

```bash
../test.py:11: note: Revealed type is "def (self: test.C, x: builtins.int)"
Success: no issues found in 1 source file
```

---

_@abhijeetbodas2001 reviewed on 2025-08-13 11:48_

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types/class.rs`:1960 on 2025-08-13 11:48_

Ah yes, the new code is indeed correct. I was under the impression that the earlier one was correct based on the code snippet I pasted above. But I now realise that that snippet is wrong (I've added `field` as an annotation, while it should be an assignment RHS).

But thanks a lot for fixing this and adding the test! And sorry for the confusion.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:212 on 2025-08-13 23:28_

I think the use of `dataclasses.field` here with a custom `dataclass_transform` is subtly wrong, as described in https://github.com/astral-sh/ty/issues/980

I'd simplify the test to not do that:
```suggestion
    name: str

a = A(name="Harry")
# error: [missing-argument]
# error: [too-many-positional-arguments]
a = A("Harry")
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:929 on 2025-08-13 23:49_

It's not awesome how we're hardcoding a correspondence with the ordering of keyword-only parameters in typeshed here, rather than actually matching by parameter name. But that's orthogonal to this PR.

But this makes it even less robust. Python 3.14 added a new `doc` parameter which will break this as it's currently written, since it means `kw_only` is not necessarily last.

We can't just list all the parameters from the front, because then this `if let` will fail to match at all on older Python versions that don't have the `kw_only` parameter.

I think we should modify this code to do something a bit more intelligent here. Ideally that more intelligent thing would be to actually match by parameter name. That should be very doable, but it is a bit more involved; it will require adding new API to `Binding`, I think. So perhaps it would make more sense as a separate PR.

So for now perhaps we just add a TODO comment here:
```suggestion
                            // TODO this will break on Python 3.14 -- we should match by parameter name instead
                            if let [default, default_factory, init, .., kw_only] =
                                overload.parameter_types()
```

---

_@carljm approved on 2025-08-13 23:51_

Looks great, thank you!

---

_Review comment by @thejchap on `crates/ty_python_semantic/src/types/call/bind.rs`:929 on 2025-08-14 01:19_

TODO added

yeah - i remember thinking this felt a little brittle while i was working on it. if you want to expand a bit on the vision for the `Binding` API or write up an issue or something i'd be interested in taking a look!

---

_@thejchap reviewed on 2025-08-14 01:19_

---

_Comment by @thejchap on 2025-08-14 01:20_

@carljm i think ive addressed everything - thanks!

---

_@carljm reviewed on 2025-08-14 14:59_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:929 on 2025-08-14 14:59_

Basically a `Binding` includes a vector of types for each parameter (matching up with parameters in source order), and it includes a `Signature` which tells us all about those parameters (including their names), but it doesn't currently have any way to say "give me the type for the parameter with this name." Unless I'm missing something (which I might be), there's no particular difficulty here, it just requires a bit of wiring things together.

---

_Merged by @carljm on 2025-08-14 15:02_

---

_Closed by @carljm on 2025-08-14 15:02_

---
