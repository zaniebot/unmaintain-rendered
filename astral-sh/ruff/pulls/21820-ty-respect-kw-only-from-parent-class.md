```yaml
number: 21820
title: "[ty] Respect `kw_only` from parent class"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: charlie/own
created_at: 2025-12-06T03:04:13Z
updated_at: 2025-12-10T09:12:20Z
url: https://github.com/astral-sh/ruff/pull/21820
synced_at: 2026-01-12T15:57:34Z
```

# [ty] Respect `kw_only` from parent class

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/1769.


---

_Review requested from @carljm by @charliermarsh on 2025-12-06 03:04_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-06 03:04_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-06 03:04_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-06 03:04_

---

_Comment by @astral-sh-bot[bot] on 2025-12-06 03:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-08 22:08:55.843892827 +0000
+++ new-output.txt	2025-12-08 22:08:59.562927816 +0000
@@ -285,9 +285,6 @@
 dataclasses_kwonly.py:23:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_kwonly.py:38:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_kwonly.py:53:11: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
-dataclasses_kwonly.py:61:1: error[missing-argument] No argument provided for required parameter `c`
-dataclasses_kwonly.py:61:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
-dataclasses_kwonly.py:61:14: error[parameter-already-assigned] Multiple values provided for parameter `b`
 dataclasses_match_args.py:42:1: error[unresolved-attribute] Class `DC4` has no attribute `__match_args__`
 dataclasses_match_args.py:49:1: error[type-assertion-failure] Type `tuple[()]` does not match asserted type `Unknown | tuple[()]`
 dataclasses_order.py:50:4: error[unsupported-operator] Operator `<` is not supported between objects of type `DC1` and `DC2`
@@ -1030,4 +1027,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1032 diagnostics
+Found 1029 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-06 03:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 39 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @charliermarsh on 2025-12-06 03:10_

I believe the conformance tests are right.

---

_Label `bug` added by @AlexWaygood on 2025-12-06 09:51_

---

_Label `ty` added by @AlexWaygood on 2025-12-06 09:51_

---

_Renamed from "Respect `kw_only` from parent class" to "[ty] Respect `kw_only` from parent class" by @AlexWaygood on 2025-12-06 09:52_

---

_Comment by @AlexWaygood on 2025-12-07 13:39_

> I believe the conformance tests are right.

yup!

---

_Comment by @AlexWaygood on 2025-12-07 13:42_

I'll let @sharkdp review this one, as our `dataclasses`/`dataclass_transform` expert

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-07 13:42_

---

_Comment by @charliermarsh on 2025-12-07 14:28_

I find the fix here pretty awkward (given that `kw_only` is optional and we're now setting it to always-`Some`) so I suspect it doesn't quite fit the intended design.

---

_Comment by @carljm on 2025-12-08 22:13_

I think you're right that this fix is awkward; took a look at why and I think it's mostly existing awkwardness / lack of support. I pushed a few TODO comments and one code change, since it was easier than describing in a review.

I think we never properly added support for `default_*` params on base-class and metaclass-based transformers, but some contributors added partial support when synthesizing `__init__`. Instead of that partial support, we should either have generalized methods for querying dataclass params (from anywhere) which always fall back to the dataclass-transformer-params, or we should ensure that `dataclass_params` on the class itself are always already initialized with values from the transformer param defaults. (It looks like this currently happens for function dataclass transforms but not for metaclass or base-class ones.) It also looks like we store `dataclass_transformer_params` on the class, but this also isn't currently set for metaclass or base-class transformers. So this special-cased support for base-class and metaclass transformers that was added to `__init__` synthesis ought to be generalized, and if that happened this PR wouldn't need to explicitly check the transformer params.

Otherwise this fix looks fine to me. The `ref mut` stuff is a little awkward but that's just a local artifact of how `own_fields` is currently structured. I'd be fine landing this with the TODO comments I added. 

Maybe ideally we'd also make `kw_only` on a field not optional in this PR, since now we are always setting it to `Some`. The only reason it was previously optional was so that `__init__` synthesis could revisit the question, but that shouldn't really be needed (and isn't after this PR.) I removed one bit of code in the `__init__` synthesis which becomes unnecessary with this fix in `own_fields`.

---

_@carljm approved on 2025-12-08 22:13_

---

_Comment by @sharkdp on 2025-12-10 09:12_

I am merging this based on Carl's review. I have opened https://github.com/astral-sh/ty/issues/1835 to track the follow up tasks here (thank you for describing them in detail — I very much agree!).

---

_Merged by @sharkdp on 2025-12-10 09:12_

---

_Closed by @sharkdp on 2025-12-10 09:12_

---

_Branch deleted on 2025-12-10 09:12_

---
