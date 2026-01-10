```yaml
number: 20995
title: "[ty] Fix bug where ty would think all types had an `__mro__` attribute"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/mro-attr
created_at: 2025-10-20T13:12:09Z
updated_at: 2025-10-27T11:31:47Z
url: https://github.com/astral-sh/ruff/pull/20995
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Fix bug where ty would think all types had an `__mro__` attribute

---

_Pull request opened by @AlexWaygood on 2025-10-20 13:12_

## Summary

**This PR is best reviewed with the "hide whitespace changes" option enabled on GitHub.**

This PR fixes a bug where ty would consider all types as having an `__mro__` attribute, when in reality this is only true for instances of `type`. The bug is fixed by lifting our special-casing for the `__mro__` attribute out of `Type::member()` and into `TypeInferenceBuilder::infer_attribute_load()`. This means that we now only infer precise types for the `__mro__` attribute for literal attribute accesses, whereas we previously also inferred this precise type for implicit attribute accesses. I think that's okay, however: we only really need the precise inference of `__mro__` for our internal tests. Calling `iter_mro()` at a higher level (rather than in the guts of `Type::member()`) also appears to have the effect that we properly propagate the specialisation of a generic class down through the entire MRO when inferring the type of `someclass.__mro__`, resulting in more intuitive types being revealed in our `mro.md` test.

The type displayed in the tooltip for some autocomplete suggestions also becomes less precise, but that also seems fine. I don't think users of autocompletion need a type that's so precise there; the less precise type seems like it's probably less noisy.

Fixes https://github.com/astral-sh/ty/issues/986

## Test Plan

Existing mdtests updated and TODOs removed


---

_Review requested from @carljm by @AlexWaygood on 2025-10-20 13:12_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-20 13:12_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-20 13:12_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-20 13:12_

---

_Label `bug` added by @AlexWaygood on 2025-10-20 13:12_

---

_Label `ty` added by @AlexWaygood on 2025-10-20 13:12_

---

_Comment by @codspeed-hq[bot] on 2025-10-20 13:29_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmro-attr?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #20995 will **improve performances by 6.23%**

<sub>Comparing <code>alex/mro-attr</code> (7c5ad22) with <code>main</code> (3c7f56f)</sub>



### Summary

`⚡ 1` improvement  
`✅ 20` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Simulation | [`` ty_micro[many_enum_members] ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmro-attr?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_enum_members%3A%3Aty_micro%5Bmany_enum_members%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 90.2 ms | 84.9 ms | +6.23% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmro-attr?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @github-actions[bot] on 2025-10-20 16:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/_util/hint/pep/proposal/pep484/pep484generic.py:418:18: warning[possibly-missing-attribute] Attribute `__mro__` may be missing on object of type `None | type`
- Found 514 diagnostics
+ Found 515 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/core/function.py:186:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `~AlwaysFalsy`
+ sympy/core/function.py:188:29: error[invalid-argument-type] Argument to function `as_int` is incorrect: Expected `SupportsIndex`, found `~None`
- Found 13696 diagnostics
+ Found 13698 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-10-20 18:19_

> This PR fixes a bug where ty would consider all types as having an `__mro__` attribute, when in reality this is only true for instances of `type`. The bug is fixed by lifting our special-casing for the `__mro__` attribute out of `Type::member()` and into `TypeInferenceBuilder::infer_attribute_load()`. This means that we now only infer precise types for the `__mro__` attribute for literal attribute accesses, whereas we previously also inferred this precise type for implicit attribute accesses.

It seems like this could have been fixed by moving that special handling to `class_member` instead?

Special-casing things in type inference (instead of core `Type` operations) is something I would wish we would do less of, not more. It's probably not very relevant for `.__mro__`, but in general, there are a lot of disadvantages to specializing things in type inference. The whole attribute access "machinery" with the descriptor protocol etc. will be overwritten, unions/intersections won't be handled, etc.

> The type displayed in the tooltip for some autocomplete suggestions also becomes less precise, but that also seems fine. I don't think users of autocompletion need a type that's so precise there

They probably don't *need* it, but I found it kind of nice?

---

_Comment by @github-actions[bot] on 2025-10-20 21:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-27 11:15:58.398773069 +0000
+++ new-output.txt	2025-10-27 11:16:01.520786835 +0000
@@ -867,11 +867,9 @@
 specialtypes_type.py:56:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[BasicUser] | type[ProUser]`, found `<class 'TeamUser'>`
 specialtypes_type.py:76:17: error[invalid-type-form] type[...] must have exactly one type argument
 specialtypes_type.py:84:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
-specialtypes_type.py:98:5: error[type-assertion-failure] Argument does not have asserted type `tuple[type, ...]`
 specialtypes_type.py:99:17: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
 specialtypes_type.py:100:17: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
 specialtypes_type.py:102:5: error[type-assertion-failure] Argument does not have asserted type `tuple[type, ...]`
-specialtypes_type.py:106:5: error[type-assertion-failure] Argument does not have asserted type `tuple[type, ...]`
 specialtypes_type.py:107:17: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
 specialtypes_type.py:108:17: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
 specialtypes_type.py:110:5: error[type-assertion-failure] Argument does not have asserted type `tuple[type, ...]`
@@ -948,5 +946,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 950 diagnostics
+Found 948 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @dcreager on 2025-10-21 01:43_

> Special-casing things in type inference (instead of core `Type` operations) is something I would wish we would do less of, not more.

+1

---

_Comment by @AlexWaygood on 2025-10-21 12:33_

> Special-casing things in type inference (instead of core `Type` operations) is something I would wish we would do less of, not more.

In general, I absolutely agree. In this specific case, though, I worry that ty's inferred MRO for a class is often pretty different to a class's actual MRO at runtime, because of various fictions typeshed would (for very good reasons) have us believe. For example, we infer the MRO of `tuple` on `main` as `tuple[<class 'tuple[Unknown, ...]'>, <class 'Sequence[_T_co@tuple]'>, <class 'Reversible[_T_co@tuple]'>, <class 'Collection[_T_co@tuple]'>, <class 'Iterable[_T_co@tuple]'>, <class 'Container[_T_co@tuple]'>, typing.Protocol, typing.Generic, <class 'object'>]`. But at runtime:

```pycon
>>> tuple.__mro__
(<class 'tuple'>, <class 'object'>)
```

Not only is typeshed's MRO for `tuple` much longer than the actual one at runtime, the MRO that we display also contains generic aliases in it, which can never appear in an MRO at runtime -- only actual class objects can appear in MROs at runtime. The lengthy MRO typeshed gives us is a white lie from typeshed, because otherwise there'd be no way to get us or other type checkers to understand that `tuple` should be considered a subtype of `Sequence` (`Sequence` is not a `Protocol`, and that's an intentional design decision in the type system). But I think displaying these frankly inaccurate MROs in things like autocomplete tooltips could honestly be pretty confusing for users, and having them be propagated through all type inference by baking the special case into `Type::member()` could have unexpected results in far-off places; to me it seems much better to keep the special case localized to actual attribute expressions in the source code. As the codspeed report shows, keeping the special case localized also seems to result in performance improvements on some microbenchmarks!

Honestly, after writing this all out, I'm not even sure whether we should special-case inference of the `__mro__` attribute at all. Maybe our MRO tests should instead use a custom `ty_extensions.reveal_mro()` helper instead? Precise inference for the `__mro__` attribute here wasn't ever implemented because it was something we felt users needed; it was implemented so that we'd have an easy way to write isolated unit tests for our MRO inference logic.

---

_Comment by @sharkdp on 2025-10-21 13:29_

> Honestly, after writing this all out, I'm not even sure whether we should special-case inference of the `__mro__` attribute at all. Maybe our MRO tests should instead use a custom `ty_extensions.reveal_mro()` helper instead?

I got the same feeling when reading your comment and was glad to find that paragraph at the end :+1: :+1: 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/mro.md`:197 on 2025-10-26 14:00_

this demonstrates the improved semantics from this PR

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:333 on 2025-10-26 14:00_

this demonstrates the improved semantics from this PR

---

_@AlexWaygood reviewed on 2025-10-26 14:00_

Most of the changes to the tests are now a result of switching over to the custom `reveal_mro` assertions, so I'm annotating the changes that show how the semantics are improved as a result of this PR:

---

_@AlexWaygood reviewed on 2025-10-26 14:02_

---

_Review comment by @AlexWaygood on `crates/ty_completion_eval/completion-evaluation-tasks.csv`:23 on 2025-10-26 14:02_

this change is because there is now another `ty_extensions` function (`reveal_mro`) that currently has a higher rank than `typing.reveal_type` in autocomplete suggestions, but would have a lower rank in an ideal world: https://github.com/astral-sh/ruff/blob/64ab79e5721ec6fdd2182fbf9d39a26534ccca43/crates/ty_completion_eval/truth/ty-extensions-lower-stdlib/main.py#L2

---

_Comment by @AlexWaygood on 2025-10-26 14:32_

The beartype primer hit is clearly a false negative going away. The sympy primer hits are a bit more complicated, but also look like true positives to me. The issue is that at line 177, we infer the type of `nargs` as `object | Any`: `Any` is the type of `nargs` before the `for` loop, and `object` is the type assigned to it from inside the `for` loop. `object | Any` just simplifies to `object`.

The conformance suite diff is also good: two false positives going away.

This PR should be ready for another round of review now. 

---

_@sharkdp approved on 2025-10-27 08:42_

Thank you! This looks great now.

---

_Merged by @AlexWaygood on 2025-10-27 11:19_

---

_Closed by @AlexWaygood on 2025-10-27 11:19_

---

_Branch deleted on 2025-10-27 11:19_

---
