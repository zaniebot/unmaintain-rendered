```yaml
number: 22161
title: "[ty] Add support for dict literals and dict() calls as default values for parameters with TypedDict types"
type: pull_request
state: open
author: Hugo-Polloli
labels:
  - ty
assignees: []
base: main
head: typed-dict-as-call-parameter
created_at: 2025-12-23T13:07:37Z
updated_at: 2026-01-16T16:43:15Z
url: https://github.com/astral-sh/ruff/pull/22161
synced_at: 2026-01-16T16:59:44Z
```

# [ty] Add support for dict literals and dict() calls as default values for parameters with TypedDict types

---

_@Hugo-Polloli_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2161

* Infer parameter default expressions using the parameter annotation as type context.
* Fix `invalid-parameter-default` false positives for `TypedDict` defaults (literals `{...}` and calls `dict(...)`).
* Add mdtests covering valid and invalid `TypedDict` defaults plus a non-`TypedDict` assignability case.

## Test Plan

* Assert no diagnostics for `Foo` defaults via `{"x": 42}` and `dict(x=42)`

* Assert `invalid-parameter-default` for missing key, wrong value type, extra key in `Foo` defaults and non-assignable default (`tuple[int] = ()`)


---

_Comment by @astral-sh-bot[bot] on 2025-12-23 13:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-23 13:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/tkinter/__init__.pyi:584:9: error[invalid-parameter-default] Default value of type `dict[Unknown, Unknown]` is not assignable to annotated parameter type `_GridIndexInfo`
- mypy/typeshed/stdlib/tkinter/__init__.pyi:594:9: error[invalid-parameter-default] Default value of type `dict[Unknown, Unknown]` is not assignable to annotated parameter type `_GridIndexInfo`
- Found 1756 diagnostics
+ Found 1754 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_ki.py:130:13: error[invalid-parameter-default] Default value of type `ReferenceType[Self@__init__]` is not assignable to annotated parameter type `ReferenceType[WeakKeyIdentityDictionary[_KT@WeakKeyIdentityDictionary, _VT@WeakKeyIdentityDictionary]]`
- Found 482 diagnostics
+ Found 481 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- Found 1836 diagnostics
+ Found 1838 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5160 diagnostics
+ Found 5157 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB


```

</details>




---

_Label `ty` added by @AlexWaygood on 2025-12-23 13:13_

---

_Comment by @AlexWaygood on 2025-12-24 11:30_

The ecosystem results look great! I wouldn't worry about the new diagnostics on `rotki` or some of the diagnostics where all that's happening is the message changing slightly. We're a bit nondeterministic right now; those just look like our standard flakes.

---

_Marked ready for review by @Hugo-Polloli on 2025-12-27 14:23_

---

_Review requested from @carljm by @Hugo-Polloli on 2025-12-27 14:23_

---

_Review requested from @AlexWaygood by @Hugo-Polloli on 2025-12-27 14:23_

---

_Review requested from @sharkdp by @Hugo-Polloli on 2025-12-27 14:23_

---

_Review requested from @dcreager by @Hugo-Polloli on 2025-12-27 14:23_

---

_Comment by @Hugo-Polloli on 2025-12-27 14:55_

> The ecosystem results look great! I wouldn't worry about the new diagnostics on `rotki` or some of the diagnostics where all that's happening is the message changing slightly. We're a bit nondeterministic right now; those just look like our standard flakes.

Thanks ! I checked too and from my understanding things are looking ok :)

---

_Comment by @MichaReiser on 2025-12-29 09:22_

Thank you for working on this. Almost the entire team is out this week. It may take a few days before someone finds time to review your PR. Happy holidays.

---

_Comment by @Hugo-Polloli on 2026-01-07 08:49_

Rebased on main as the branch was getting stale, but it looks like everything is still fine with no conflict/regression, so this PR is ready

---

_@ibraheemdev reviewed on 2026-01-07 19:30_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:2626 on 2026-01-07 19:30_

Why do we need to perform standalone inference here? You can infer an expression with type context using `self.infer_expression`.

---

_@Hugo-Polloli reviewed on 2026-01-07 21:15_

---

_Review comment by @Hugo-Polloli on `crates/ty_python_semantic/src/types/infer/builder.rs`:2626 on 2026-01-07 21:15_

I started with `self.infer_expression`, but parameter defaults aren't standalone expressions in the semantic index, so it panics with `no entry found for key`.
The workaround I found is to synthesize an Expression and run standalone inference in the default's enclosing scope.

Here's the test file I used:
```py
class Foo(TypedDict):
    x: int

def ok(default_x: Foo = {"x": 42}): ... # all good
outer_default_x = 42
def not_ok(default_x: Foo = {"x": outer_default_x}): ...  # panics
```
I'm unsure that my fix is the correct one, I just know I could not get infer_expression to work in this case, though I agree it would be cleaner if we could make it work here, I'm open to suggestions/learning about things I missed!

Also in the meantime I pushed a diff that extract this custom handling to a `file_expression_type_with_context` function with comments explaining the reasoning if that can help

---

_@ibraheemdev reviewed on 2026-01-08 03:30_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:2626 on 2026-01-08 03:30_

> parameter defaults aren't standalone expressions in the semantic index

It sounds like you tried `infer_standalone_expression`, I don't think `infer_expression` has that panic path?

---

_@Hugo-Polloli reviewed on 2026-01-08 15:23_

---

_Review comment by @Hugo-Polloli on `crates/ty_python_semantic/src/types/infer/builder.rs`:2626 on 2026-01-08 15:23_

oof sorry, "standalone expression" was rly confusing wording on my part! 🫠 I didn't mean I used `infer_standalone_expression`, I tried `infer_expression`.

The issue is scope, `infer_parameter_definition` runs with the function body scope, but the parameter defaults are indexed in the enclosing scope. `infer_expression` resolves names using the current scope’s `ast_ids` map. So when defaults reference names from the outer scope (like `outer_default_x` in my above example), the lookup hits `ast_ids.use_id(...)` and this is where we panic with `no entry found for key`.

I hope it makes better sense now?

---

_Assigned to @ibraheemdev by @ibraheemdev on 2026-01-16 16:43_

---
