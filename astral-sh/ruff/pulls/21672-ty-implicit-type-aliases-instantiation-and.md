```yaml
number: 21672
title: "[ty] Implicit type aliases: Instantiation and attribute access"
type: pull_request
state: open
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: david/implicit-type-aliases-instantiation
created_at: 2025-11-28T08:42:02Z
updated_at: 2025-11-28T09:12:47Z
url: https://github.com/astral-sh/ruff/pull/21672
synced_at: 2026-01-12T15:57:30Z
```

# [ty] Implicit type aliases: Instantiation and attribute access

---

_@sharkdp_

## Summary

Allow `Annotated[MyClass, …]` instances to be instantiated.

## Test Plan

New Markdown tests.

---

_Label `ty` added by @sharkdp on 2025-11-28 08:42_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-28 08:42_

---

_Comment by @astral-sh-bot[bot] on 2025-11-28 08:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-28 08:43:48.579149249 +0000
+++ new-output.txt	2025-11-28 08:43:51.915166455 +0000
@@ -877,8 +877,6 @@
 qualifiers_annotated.py:79:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[Unknown]`, found `<typing.Annotated special form>`
 qualifiers_annotated.py:80:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[Unknown]`, found `<typing.Annotated special form>`
 qualifiers_annotated.py:86:1: error[call-non-callable] Object of type `typing.Annotated` is not callable
-qualifiers_annotated.py:87:1: error[call-non-callable] Object of type `GenericAlias` is not callable
-qualifiers_annotated.py:88:1: error[call-non-callable] Object of type `GenericAlias` is not callable
 qualifiers_final_annotation.py:18:7: error[invalid-type-form] Type qualifier `typing.Final` expected exactly 1 argument, got 2
 qualifiers_final_annotation.py:54:9: error[invalid-assignment] Cannot assign to final attribute `ID5` in `__init__` because it already has a value at class level
 qualifiers_final_annotation.py:65:9: error[invalid-assignment] Cannot assign to final attribute `ID7` on type `Self@method1`
@@ -1029,4 +1027,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1031 diagnostics
+Found 1029 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-28 08:45_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 43 diagnostics
+ Found 42 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/accounting/structures/processed_event.py:85:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/api/rest.py:1039:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2038 diagnostics
+ Found 2040 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/functional_serializers.py:457:36: error[call-non-callable] Object of type `GenericAlias` is not callable
- pydantic/functional_validators.py:824:36: error[call-non-callable] Object of type `GenericAlias` is not callable
- Found 6711 diagnostics
+ Found 6709 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-28 08:50_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `call-non-callable` | 0 | 2 | 0 |
| **Total** | **0** | **2** | **0** |

**[Full report with detailed diff](https://david-implicit-type-aliases-zjug.ecosystem-663.pages.dev/diff)** ([timing results](https://david-implicit-type-aliases-zjug.ecosystem-663.pages.dev/timing))




---

_Marked ready for review by @sharkdp on 2025-11-28 08:54_

---

_Review requested from @carljm by @sharkdp on 2025-11-28 08:54_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-28 08:54_

---

_Review requested from @dcreager by @sharkdp on 2025-11-28 08:54_

---

_Comment by @AlexWaygood on 2025-11-28 09:08_

Oh, the typing conformance suite results seem to suggest that we shouldn't do this: apparently the two lines where diagnostics are going away are places we're _supposed_ to emit diagnostics. But it does make sense to me to just emulate what happens at runtime; I don't know why that rule exists in the spec (https://typing.python.org/en/latest/spec/qualifiers.html#annotated)

---

_Comment by @sharkdp on 2025-11-28 09:12_

> I don't know why that rule exists in the spec ([typing.python.org/en/latest/spec/qualifiers.html#annotated](https://typing.python.org/en/latest/spec/qualifiers.html#annotated))

Oh. I only looked at the `typing.Annotated` documentation and forgot to check the typing spec. This one: "An attempt to call Annotated (whether parameterized or not) should be treated as a type error by type checkers"? That's strange, yes. Moving this to draft for now.

---

_Converted to draft by @sharkdp on 2025-11-28 09:12_

---
