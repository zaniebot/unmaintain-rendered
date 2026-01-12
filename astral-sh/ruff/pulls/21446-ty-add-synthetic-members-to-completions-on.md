```yaml
number: 21446
title: "[ty] Add synthetic members to completions on dataclasses"
type: pull_request
state: merged
author: sharkdp
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: david/fix-1542
created_at: 2025-11-14T08:57:35Z
updated_at: 2025-11-14T10:31:22Z
url: https://github.com/astral-sh/ruff/pull/21446
synced_at: 2026-01-12T15:57:24Z
```

# [ty] Add synthetic members to completions on dataclasses

---

_@sharkdp_

## Summary

Add synthetic members to completions on dataclasses and dataclass instances.

Also, while we're at it, add support for `__weakref__` and `__match_args__`.

closes https://github.com/astral-sh/ty/issues/1542

## Test Plan

New Markdown tests


---

_Review requested from @carljm by @sharkdp on 2025-11-14 08:57_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-14 08:57_

---

_Review requested from @dcreager by @sharkdp on 2025-11-14 08:57_

---

_Label `server` added by @sharkdp on 2025-11-14 08:57_

---

_Label `ty` added by @sharkdp on 2025-11-14 08:57_

---

_@sharkdp reviewed on 2025-11-14 08:59_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:127 on 2025-11-14 08:59_

Looks like this was wrong before, because we were extending an instance type with class members. Not a huge deal, but leads to wrong types (function literals instead of bound methods). Fixed here.

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 08:59_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-14 10:03:18.123685573 +0000
+++ new-output.txt	2025-11-14 10:03:21.738714597 +0000
@@ -266,12 +266,6 @@
 dataclasses_kwonly.py:61:1: error[missing-argument] No argument provided for required parameter `c`
 dataclasses_kwonly.py:61:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
 dataclasses_kwonly.py:61:14: error[parameter-already-assigned] Multiple values provided for parameter `b`
-dataclasses_match_args.py:18:1: error[type-assertion-failure] Argument does not have asserted type `tuple[Literal["x"]]`
-dataclasses_match_args.py:18:13: error[unresolved-attribute] Class `DC1` has no attribute `__match_args__`
-dataclasses_match_args.py:26:1: error[type-assertion-failure] Argument does not have asserted type `tuple[Literal["x"]]`
-dataclasses_match_args.py:26:13: error[unresolved-attribute] Class `DC2` has no attribute `__match_args__`
-dataclasses_match_args.py:34:1: error[type-assertion-failure] Argument does not have asserted type `tuple[Literal["x"]]`
-dataclasses_match_args.py:34:13: error[unresolved-attribute] Class `DC3` has no attribute `__match_args__`
 dataclasses_match_args.py:42:1: error[unresolved-attribute] Class `DC4` has no attribute `__match_args__`
 dataclasses_match_args.py:49:1: error[type-assertion-failure] Argument does not have asserted type `tuple[()]`
 dataclasses_order.py:50:4: error[unsupported-operator] Operator `<` is not supported for types `DC1` and `DC2`
@@ -999,5 +993,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1001 diagnostics
+Found 995 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-14 09:00_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:43 on 2025-11-14 09:01_

I think there's some subtlety here where this is only added to a _slotted_ dataclass if `weakref_slot=True` in the dataclass decorator? Not hugely important, though I guess you could add a failing test for it.

Is it worth adding `__match_args__` to this list now ("prematurely")? Then the all_members test for `__match_args__` will just automatically start passing when we add support for the synthesised attribute on the type-inference side 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:45 on 2025-11-14 09:07_

I have a vague memory that there's also a synthesised `__dataclass_params__` attribute...

---

_@AlexWaygood approved on 2025-11-14 09:07_

Thank you!

---

_@sharkdp reviewed on 2025-11-14 09:08_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:436 on 2025-11-14 09:08_

Typed dicts are somewhat special. I tried to incorporate them here, but it only results in way more code.

---

_@AlexWaygood reviewed on 2025-11-14 09:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:127 on 2025-11-14 09:09_

I guess we could add a test to completions.rs that shows that we now display the type correctly for the suggestions? Not essential though 

---

_@sharkdp reviewed on 2025-11-14 09:50_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:43 on 2025-11-14 09:50_

I just added proper support for both `weakref_slot` and `match_args`. All TODOs are gone, now. Added new tests for both features in `dataclasses.md` as well.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:45 on 2025-11-14 09:51_

I added that as a new member of type `Any`.

---

_@sharkdp reviewed on 2025-11-14 09:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2407 on 2025-11-14 09:52_

This would provide a modicum of type safety?

```suggestion
                Some(UnionType::from_elements(db, [Type::any(), Type::none(db)]))
```

---

_@AlexWaygood reviewed on 2025-11-14 09:52_

---

_@AlexWaygood reviewed on 2025-11-14 09:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:462 on 2025-11-14 09:56_

This parameter doesn't exist on 3.9. But maybe not worth worrying about since 3.9 is end-of-life 

---

_@AlexWaygood approved on 2025-11-14 09:57_

---

_@sharkdp reviewed on 2025-11-14 10:01_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:462 on 2025-11-14 10:01_

Fixed. Decided not add a test, though.

---

_@sharkdp reviewed on 2025-11-14 10:02_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:127 on 2025-11-14 10:02_

Decided that this is not important enough.

---

_Merged by @sharkdp on 2025-11-14 10:31_

---

_Closed by @sharkdp on 2025-11-14 10:31_

---

_Branch deleted on 2025-11-14 10:31_

---
