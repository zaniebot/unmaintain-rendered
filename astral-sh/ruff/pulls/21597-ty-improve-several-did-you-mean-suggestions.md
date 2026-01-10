```yaml
number: 21597
title: "[ty] Improve several \"Did you mean?\" suggestions"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/did-you-mean
created_at: 2025-11-23T20:21:27Z
updated_at: 2025-11-25T10:29:03Z
url: https://github.com/astral-sh/ruff/pull/21597
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Improve several "Did you mean?" suggestions

---

_Pull request opened by @AlexWaygood on 2025-11-23 20:21_

## Summary

- Make some diagnostics more concise by moving suggestions from a subdiagnostic to a primary or secondary annotation message
- Make some diagnostic summary lines more concise by moving suggestions from the diagnostic summary line to subdiagnostics
- Consistently use the `help` severity for suggestions when they do appear as subdiagnostics, rather than the `info` severity

## Test Plan

Snapshots


---

_Label `ty` added by @AlexWaygood on 2025-11-23 20:21_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-23 20:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:159 on 2025-11-23 20:23_

we don't have any tests for this subdiagnostic and I'm not convinced it's likely to be helpful. If a user has `int + str` in a type expression, I think it's very unclear what they meant; if they have `int - bool`, I think it's likely that they were trying to write a negation type rather than a union type; etc.

---

_Comment by @astral-sh-bot[bot] on 2025-11-23 20:23_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-23 20:23:14.805065538 +0000
+++ new-output.txt	2025-11-23 20:23:18.493085157 +0000
@@ -78,7 +78,7 @@
 annotations_forward_refs.py:47:10: error[invalid-type-form] Invalid subscript of object of type `list[Unknown | <class 'int'>]` in type expression
 annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
 annotations_forward_refs.py:54:11: error[fstring-type-annotation] Type expressions cannot use f-strings
-annotations_forward_refs.py:55:11: error[invalid-type-form] Variable of type `<module 'types'>` is not allowed in a type expression
+annotations_forward_refs.py:55:11: error[invalid-type-form] Module `types` is not valid in a type expression
 annotations_forward_refs.py:80:14: error[unresolved-reference] Name `ClassF` used when not defined
 annotations_forward_refs.py:82:11: error[invalid-type-form] Variable of type `Literal[""]` is not allowed in a type expression
 annotations_forward_refs.py:87:9: error[invalid-type-form] Variable of type `def int(self) -> None` is not allowed in a type expression
@@ -110,7 +110,7 @@
 annotations_typeexpr.py:99:10: error[invalid-type-form] Unary operations are not allowed in type expressions
 annotations_typeexpr.py:100:10: error[invalid-type-form] Boolean operations are not allowed in type expressions
 annotations_typeexpr.py:101:10: error[fstring-type-annotation] Type expressions cannot use f-strings
-annotations_typeexpr.py:102:10: error[invalid-type-form] Variable of type `<module 'types'>` is not allowed in a type expression
+annotations_typeexpr.py:102:10: error[invalid-type-form] Module `types` is not valid in a type expression
 callables_annotation.py:25:5: error[missing-argument] No argument provided for required parameter 2
 callables_annotation.py:26:11: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Literal[2]`
 callables_annotation.py:27:15: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
@@ -1001,6 +1001,6 @@
 typeddicts_usage.py:24:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal["1982"]`
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
-typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
+typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
 Found 1004 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_@AlexWaygood reviewed on 2025-11-23 20:23_

---

_Marked ready for review by @AlexWaygood on 2025-11-23 20:23_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-23 20:23_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-23 20:23_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-23 20:23_

---

_Comment by @astral-sh-bot[bot] on 2025-11-23 20:25_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
rich (https://github.com/Textualize/rich)
- tests/test_console.py:290:35: error[invalid-type-form] Variable of type `<module 'datetime'>` is not allowed in a type expression
+ tests/test_console.py:290:35: error[invalid-type-form] Module `datetime` is not valid in a type expression: Did you mean to use the module's member `datetime.datetime`?

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:701:24: error[invalid-type-form] Variable of type `<module 'datetime'>` is not allowed in a type expression
- src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:702:10: error[invalid-type-form] Variable of type `<module 'datetime'>` is not allowed in a type expression
- src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:742:63: error[invalid-type-form] Variable of type `<module 'datetime'>` is not allowed in a type expression
- src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:742:76: error[invalid-type-form] Variable of type `<module 'datetime'>` is not allowed in a type expression
+ src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:701:24: error[invalid-type-form] Module `datetime` is not valid in a type expression: Did you mean to use the module's member `datetime.datetime`?
+ src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:702:10: error[invalid-type-form] Module `datetime` is not valid in a type expression: Did you mean to use the module's member `datetime.datetime`?
+ src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:742:63: error[invalid-type-form] Module `datetime` is not valid in a type expression: Did you mean to use the module's member `datetime.datetime`?
+ src/integrations/prefect-snowflake/prefect_snowflake/experimental/workers/spcs.py:742:76: error[invalid-type-form] Module `datetime` is not valid in a type expression: Did you mean to use the module's member `datetime.datetime`?

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/snowflake/tests/test_udf.py:67:22: error[invalid-type-form] Variable of type `<module 'json'>` is not allowed in a type expression
+ ibis/backends/snowflake/tests/test_udf.py:67:22: error[invalid-type-form] Module `json` is not valid in a type expression
- ibis/expr/datatypes/tests/test_core.py:150:8: error[invalid-type-form] Variable of type `<module 'decimal'>` is not allowed in a type expression
+ ibis/expr/datatypes/tests/test_core.py:150:8: error[invalid-type-form] Module `decimal` is not valid in a type expression


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-11-23 20:27_

Screenshot of some of the new diagnostics, since snapshots don't have colour: 
<img width="1832" height="1324" alt="image" src="https://github.com/user-attachments/assets/f1fc30ec-a288-40b8-8b18-4e9684755d87" />


---

_Comment by @MichaReiser on 2025-11-24 07:42_

> Make some diagnostics more concise by moving suggestions from a subdiagnostic to a primary or secondary annotation message

I think I've a preference towards not doing that because it makes the `concise` output less concise and I also find it a bit confusing that the description of some underlined item is a question. 

---

_@MichaReiser reviewed on 2025-11-24 07:43_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:8643 on 2025-11-24 07:43_

This should be "easy" once my unused suppression PR lands.

---

_@MichaReiser reviewed on 2025-11-24 07:44_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:159 on 2025-11-24 07:44_

We could say something about that only `|` is allowed to define a union.

---

_@MichaReiser reviewed on 2025-11-24 07:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:8650 on 2025-11-24 07:45_

I think I know what it is but it might not be clear to all users what a concrete `TypedDict` is.

---

_@AlexWaygood reviewed on 2025-11-24 07:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:159 on 2025-11-24 07:46_

We could, but we already link to the full type expression grammar in a separate subdiagnostic (which covers that), and I think it's very unclear that they were actually trying too define a union at all 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8650 on 2025-11-24 07:53_

A type that actually is a TypedDict, rather than the special form typing.TypedDict itself.

```py
from typing import TypedDict

class MyTypedDict(TypedDict):
    x: int

obj: MyTypedDict
```

instead of 

```py
from typing import TypedDict

obj: TypedDict
```

I take your point that this might be unclear to some people, but I'm not sure what clearer wording here might be... "An actual TypedDict" feels just as bad. And I can't put that whole example in the subdiagnostic!

---

_@AlexWaygood reviewed on 2025-11-24 07:53_

---

_@MichaReiser reviewed on 2025-11-24 08:07_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:8650 on 2025-11-24 08:07_

Okay, I actually assumed it's something different 😅 

But I agree it's tricky. I tried to produce a similar error in TypeScript but they don't have this issue because it doesn't have marker types similar to `TypedDict` 

---

_Comment by @sharkdp on 2025-11-24 08:46_

> > Make some diagnostics more concise by moving suggestions from a subdiagnostic to a primary or secondary annotation message
> 
> I think I've a preference towards not doing that because it makes the `concise` output less concise and I also find it a bit confusing that the description of some underlined item is a question.

If we are confident in our suggestion, I like the change here. I always look at the primary annotation before I even read the main diagnostic message, and if it's directly "actionable", that's perfect.

---

_Comment by @AlexWaygood on 2025-11-24 10:02_

Yeah, I agree with @sharkdp. If we have a direct suggestion for how to fix the diagnostic, that feels like one of the most important and useful parts of the diagnostic display to me. I think it's really useful to have that highlighted prominently, and I like that with secondary annotations we can clearly indicate that we might only be suggesting to change _part_ of the diagnostic range, not necessarily the whole range.

> it makes the `concise` output less concise

That's true of course, but to me it seems like very useful information that I'd want as part of the concise diagnostic. And I don't think the new concise diagnostic messages we see in mypy_primer are _that_ verbose?

---

_Comment by @MichaReiser on 2025-11-24 10:15_

I wonder if the solution here really is to just attach a `Fix` 

---

_Review request for @carljm removed by @carljm on 2025-11-24 22:34_

---

_Comment by @AlexWaygood on 2025-11-25 10:28_

I'll land this for now as an incremental improvement. It's easy to make further adjustments later and we can easily re-evaluate if and when we add fixes for some or all of these diagnostics

---

_Merged by @AlexWaygood on 2025-11-25 10:29_

---

_Closed by @AlexWaygood on 2025-11-25 10:29_

---

_Branch deleted on 2025-11-25 10:29_

---
