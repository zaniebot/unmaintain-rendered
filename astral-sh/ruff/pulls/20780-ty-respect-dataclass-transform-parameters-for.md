```yaml
number: 20780
title: "[ty] Respect `dataclass_transform` parameters for metaclass-based models"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/dataclass_transform-params-for-metaclass-models
created_at: 2025-10-09T08:24:53Z
updated_at: 2025-10-09T13:24:23Z
url: https://github.com/astral-sh/ruff/pull/20780
synced_at: 2026-01-12T15:57:09Z
```

# [ty] Respect `dataclass_transform` parameters for metaclass-based models

---

_@sharkdp_

## Summary

Respect parameters such as `frozen_default` for metaclass-based `@dataclass_transformer` models.

Related to: https://github.com/astral-sh/ty/issues/1260

## Typing conformance changes

Those are all correct (new true positives)

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-10-09 08:24_

---

_@sharkdp reviewed on 2025-10-09 08:25_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2170 on 2025-10-09 08:25_

This is the key part: we also fall back to the `dataclass_transform` parameters, if necessary.

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-09 08:26_

---

_Comment by @github-actions[bot] on 2025-10-09 08:27_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-09 13:21:29.281810829 +0000
+++ new-output.txt	2025-10-09 13:21:32.604839205 +0000
@@ -292,8 +292,12 @@
 dataclasses_transform_func.py:77:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@create_model_frozen`
 dataclasses_transform_func.py:97:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
 dataclasses_transform_meta.py:60:36: error[unknown-argument] Argument `other_name` does not match any known parameter
+dataclasses_transform_meta.py:66:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_meta.py:73:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
 dataclasses_transform_meta.py:79:6: error[unsupported-operator] Operator `<` is not supported for types `Customer2` and `Customer2`
+dataclasses_transform_meta.py:83:8: error[missing-argument] No argument provided for required parameter `id`
+dataclasses_transform_meta.py:83:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
+dataclasses_transform_meta.py:103:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
 dataclasses_usage.py:50:6: error[missing-argument] No argument provided for required parameter `unit_price`
 dataclasses_usage.py:51:28: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["price"]`
 dataclasses_usage.py:52:36: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
@@ -896,5 +900,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 897 diagnostics
+Found 901 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-10-09 08:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-09 08:31_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-dataclass-transform-pa.ecosystem-663.pages.dev/diff)** ([timing results](https://david-dataclass-transform-pa.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-09 08:56_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-09 08:56_

---

_Marked ready for review by @sharkdp on 2025-10-09 09:29_

---

_Review requested from @carljm by @sharkdp on 2025-10-09 09:29_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-09 09:29_

---

_Review requested from @dcreager by @sharkdp on 2025-10-09 09:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:207 on 2025-10-09 11:42_

just to check for accidental `Unknown`s creeping in?

```suggestion
reveal_type(TestWithMeta(1) < TestWithMeta(2))  # revealed: bool
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:223 on 2025-10-09 11:43_

```suggestion
# TODO: No errors here, should reveal `bool`
# error: [too-many-positional-arguments]
# error: [too-many-positional-arguments]
# error: [unsupported-operator]
reveal_type(TestWithBase(1) < TestWithBase(2))  # revealed: Unknown
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:482 on 2025-10-09 11:49_

```suggestion
t = TemperatureSensor(key=1, name="Temperature Sensor")
reveal_type(t.key)  # revealed: int
reveal_type(t.name)  # revealed: str
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:239 on 2025-10-09 11:53_

you could also write this as

```suggestion
        CodeGeneratorKind::from_class(db, class).is_some_and(|generator_kind| {
            matches!(
                (generator_kind, self),
                (Self::DataclassLike(_), Self::DataclassLike(_))
                    | (Self::NamedTuple, Self::NamedTuple)
                    | (Self::TypedDict, Self::TypedDict)
            )
        })
```

(which is slightly longer but looks slightly prettier to me)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2288 on 2025-10-09 11:55_

```suggestion
                if !has_dataclass_param(DataclassParams::INIT) {
```

---

_@AlexWaygood approved on 2025-10-09 11:56_

LGTM!

---

_@sharkdp reviewed on 2025-10-09 13:09_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:239 on 2025-10-09 13:09_

I don't really like either of these, clippy wanted me to change it to a `matches!(…)`. I liked my simple `match` best.

---

_@AlexWaygood reviewed on 2025-10-09 13:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:239 on 2025-10-09 13:17_

I don't feel strongly; I'm happy with anything here

---

_Merged by @sharkdp on 2025-10-09 13:24_

---

_Closed by @sharkdp on 2025-10-09 13:24_

---

_Branch deleted on 2025-10-09 13:24_

---
