```yaml
number: 22436
title: "[ty] Offer completions for `T` when a value has type `Unknown | T`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/fix-dynamic-type-union
created_at: 2026-01-07T12:43:11Z
updated_at: 2026-01-07T15:15:37Z
url: https://github.com/astral-sh/ruff/pull/22436
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Offer completions for `T` when a value has type `Unknown | T`

---

_Pull request opened by @BurntSushi on 2026-01-07 12:43_

Fixes astral-sh/ty#2197


---

_Review requested from @carljm by @BurntSushi on 2026-01-07 12:43_

---

_Review requested from @AlexWaygood by @BurntSushi on 2026-01-07 12:43_

---

_Review requested from @sharkdp by @BurntSushi on 2026-01-07 12:43_

---

_Review requested from @dcreager by @BurntSushi on 2026-01-07 12:43_

---

_Review requested from @MichaReiser by @BurntSushi on 2026-01-07 12:43_

---

_Review requested from @Gankra by @BurntSushi on 2026-01-07 12:43_

---

_Review request for @dcreager removed by @BurntSushi on 2026-01-07 12:43_

---

_Review request for @carljm removed by @BurntSushi on 2026-01-07 12:43_

---

_Review request for @Gankra removed by @BurntSushi on 2026-01-07 12:43_

---

_Review request for @MichaReiser removed by @BurntSushi on 2026-01-07 12:43_

---

_Review request for @sharkdp removed by @BurntSushi on 2026-01-07 12:43_

---

_Comment by @astral-sh-bot[bot] on 2026-01-07 12:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-07 12:46_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
- Found 5363 diagnostics
+ Found 5358 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`


```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood approved on 2026-01-07 12:46_

For completeness, we probably also want similar logic in the `Type::Intersection` branch as well. Just like `Any`, `int & Any` also has infinite attributes available, so `(int & Any) | str` should probably also offer only the attributes on `str` as autocomplete suggestions?

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:7761 on 2026-01-07 12:49_

this test could be a bit fragile, considering that we intend to change our behaviour here in the future -- this won't be inferred as `Unknown | str` at all in the long term. I think it's a useful integration test anyway, but you could also add a direct unit test as well that will continue to directly test the change you're making here even if we change how we infer the elements of unannotated list literals:

```rs
    #[test]
    fn dynamic_type_2() {
        let builder = completion_test_builder(
            r#"
from typing import Any

def f(x: Any | str):
    x.remove<CURSOR>
"#,
        );
        assert_snapshot!(
            builder.build().snapshot(),
            @r"
        removeprefix
        removesuffix
        ",
        );
    }

---

_@AlexWaygood reviewed on 2026-01-07 12:49_

---

_Label `server` added by @MichaReiser on 2026-01-07 12:57_

---

_Label `ty` added by @MichaReiser on 2026-01-07 12:57_

---

_Assigned to @BurntSushi by @BurntSushi on 2026-01-07 13:20_

---

_Comment by @BurntSushi on 2026-01-07 13:27_

> Just like `Any`, `int & Any` also has infinite attributes available, so `(int & Any) | str` should probably also offer only the attributes on `str` as autocomplete suggestions?

This seems to work already without any other changes. See the new test I added.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:7792 on 2026-01-07 13:29_

we display an intersection as `int & Any`, but that syntax isn't standardised yet and we don't actually support it in user annotations -- you want

```suggestion
from typing import Any
from ty_extensions import Intersection

def f(x: Intersection[int, Any] | str):
```

---

_@AlexWaygood reviewed on 2026-01-07 13:29_

---

_@AlexWaygood reviewed on 2026-01-07 13:32_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:7792 on 2026-01-07 13:32_

and the test fails if you make this change ;) I think the test as you had it wasn't testing intersection types -- we were resolving `int & Any` in the user annotation to `Unknown`, because of the fact that we don't support that syntax in user annotations, and because we don't see type-checker diagnostics in these tests that went unnoticed.

---

_@BurntSushi reviewed on 2026-01-07 14:23_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:7792 on 2026-01-07 14:23_

Aye okay, thank you! I pushed an update that I think should fix this.

---

_Unassigned @BurntSushi by @BurntSushi on 2026-01-07 14:23_

---

_@AlexWaygood approved on 2026-01-07 14:27_

LGTM!

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:7788 on 2026-01-07 14:29_

Oh... I guess most pure unit tests of this sort are actually found in https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/ide_support/all_members.md currently? That might be a better home for them. The integration test for the unannotated list seems like a good candidate for this file, though

---

_@AlexWaygood reviewed on 2026-01-07 14:29_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:7788 on 2026-01-07 15:00_

Fair enough. Done. But I left the completion tests there as an explicit check that completions still work as we expect in this case.

---

_@BurntSushi reviewed on 2026-01-07 15:00_

---

_@AlexWaygood approved on 2026-01-07 15:01_

---

_Merged by @BurntSushi on 2026-01-07 15:15_

---

_Closed by @BurntSushi on 2026-01-07 15:15_

---

_Branch deleted on 2026-01-07 15:15_

---
