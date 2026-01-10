```yaml
number: 22111
title: "[ty] Inlay hint auto import"
type: pull_request
state: open
author: MatthewMckee4
labels:
  - server
  - ty
assignees: []
base: main
head: inlay-hint-auto-import
created_at: 2025-12-20T13:35:19Z
updated_at: 2026-01-09T11:23:23Z
url: https://github.com/astral-sh/ruff/pull/22111
synced_at: 2026-01-10T15:56:07Z
```

# [ty] Inlay hint auto import

---

_Pull request opened by @MatthewMckee4 on 2025-12-20 13:35_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements the auto importing for unimported symbols introduced when "applying" an inlay hint.

[Screencast_20251222_120138.webm](https://github.com/user-attachments/assets/9a719c29-9f5c-42fd-8dd6-51b43cd7e2a1)

There are several things to consider here.

We have all of the information about the symbols we need to import,
from the type display details. 
So we need to get the definition of the type and attempt to import that.
Also from the type display details, we get the name of the symbol that is
displayed in the inlay hint, this is useful for the following reason.
If we have an inlay hint that contains the same symbol name twice, but 
from different symbols, bar.A and baz.A. the label we see in the inlay
hint will be exactly `bar.A` and `baz.A`. From this we know that we should
not try to add imports statement `from bar import A` and `from baz import A`,
because these will conflict. I believe this is all of the information we can
get that can tell us if we should "qualify" a symbol (bar.A or A).

So, we check if the label (like "bar.A") contains ".", this could possibly 
be improved. If it does we force a "import <module>" statement via:

```rs
ImportRequest::import(module_name, definition_name).force()
```

Otherwise, we try to add a "from <module> import <symbol>" statement via:

```rs
ImportRequest::import_from(module_name, definition_name)
```

We don't force here since it is okay if we still end up adding a
"import <module>" statement and we qualify the name via 
"import_action.symbol_text()".

```rs
let qualified_name = |import_action: &ImportAction| {
    if import_action.import().is_some() {
        return None;
    }

    let symbol_text = import_action.symbol_text();

    Some(symbol_text.to_string())
};
```

We also do not consider the symbol being a module. I think this is fine.

Because we store these new imports, and the importer cant work with dynamiclly imported members, we can get two from imports from the same module with two different import statements, like the following.

```py
from ty_extensions import Top
from ty_extensions import Unknown

def f(xyxy: object):
    if isinstance(xyxy, list):
        x: Top[list[Unknown]] = xyxy
```

### Current Issues

I have seen issues with Literal (we don't find the right symbol to import) and None (we import NoneType)

## Test Plan

Update several ty_ide tests and added some more.

I think i will need to add even more.

## Performance Test

```python title="main.py"
import foo

a = foo.C().foo() # 75x
```

```python title="bar.py"
class D[T, U, V]: ...
```

```python title="bar.py"
from typing import Literal
from bar import D
import bar

import pydantic
import numpy as np
import sqlmodel


class A[T]: ...

class B[T]: ...

class C:
    def foo(self) -> B[A[bar.D[np.typing.NDArray, list[sqlmodel.SQLModel | A[B[list[int]]]], pydantic.BaseModel]]]:
        raise NotImplementedError
```

In main.py, we get the following results.

### Before

```text
Inlay hint request returned 75 hints in 23.881372ms
Inlay hint request returned 75 hints in 22.479724ms
Inlay hint request returned 75 hints in 25.541434ms
Inlay hint request returned 75 hints in 23.30347ms
Inlay hint request returned 75 hints in 23.291729ms
```

### After

```text
Inlay hint request returned 75 hints in 38.310759ms
Inlay hint request returned 75 hints in 47.835834ms
Inlay hint request returned 75 hints in 36.666961ms
Inlay hint request returned 75 hints in 36.459227ms
Inlay hint request returned 75 hints in 56.092519ms
```

---

_@MatthewMckee4 reviewed on 2025-12-20 13:36_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:7518 on 2025-12-20 13:36_

Adding this statement is wrong, and I'm not yet sure why

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 13:37_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-20 13:38_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5530 diagnostics
+ Found 5535 diagnostics

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2805 diagnostics
+ Found 2802 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5149 diagnostics
+ Found 5150 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14441 diagnostics
+ Found 14440 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `server` added by @AlexWaygood on 2025-12-20 13:49_

---

_Label `ty` added by @AlexWaygood on 2025-12-20 13:49_

---

_Renamed from "Inlay hint auto import" to "[ty] Inlay hint auto import" by @MatthewMckee4 on 2025-12-20 13:52_

---

_@MatthewMckee4 reviewed on 2025-12-20 14:15_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:7618 on 2025-12-20 14:15_

Having an issue here. We are not importing `Literal` here.

With some printlns we get see that <special-form 'typing.Literal'> is resolving to a definition but we can't find a name for it.

Not to mention us trying import `str`, for the actual literal type.

```text
details.label = "Any | Literal[\"some\"]"
details.targets = [0..3, 6..13, 14..20]

Type: <special-form 'typing.Literal'>
qualified_label_part
type_definition = SpecialForm(Definition { [salsa id]: Id(1486) })
definition = Definition { [salsa id]: Id(1486) }
definition_name = None

Type: Literal["some"]
qualified_label_part
type_definition = Class(Definition { [salsa id]: Id(6135) })
definition = Definition { [salsa id]: Id(6135) }
definition_name = Some("str")
importing "str".
```


---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:338 on 2025-12-20 14:16_

Many tests were broken, because they had multiple inlay hints and the import statements were being put in weird places, happy to remove this, or keep it internal.

---

_@MatthewMckee4 reviewed on 2025-12-20 14:16_

---

_@MatthewMckee4 reviewed on 2025-12-20 14:17_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:7618 on 2025-12-20 14:17_

Not really sure where to go from here, other than marking literals as invalid syntax, which seems wrong.

---

_@MatthewMckee4 reviewed on 2025-12-20 14:25_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:7443 on 2025-12-20 14:25_

Should this annotation that we insert be the one we show in the inlay hint?

It currently isn't, but interested to what what people think

---

_Marked ready for review by @MatthewMckee4 on 2025-12-20 14:51_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-12-20 14:51_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-12-20 14:51_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-12-20 14:51_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-12-20 14:51_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-12-20 14:51_

---

_Comment by @MatthewMckee4 on 2025-12-20 14:51_

Would appreciate some initial feedback. thanks

---

_Comment by @MatthewMckee4 on 2025-12-21 02:05_

Already I have seen issues with `Literal` (we don't find the right symbol to import) and `None` (we import `NoneType`)

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:249 on 2025-12-22 10:08_

Can you say what the use cases are where a user would want to disable auto-import for inlay hints? 



---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:35 on 2025-12-22 10:11_

The arguments here get a bit out of hand and we might also benefit from caching `Stylist::from_tokens`. It could make sense to have a `InlayHintBuilter` or similar that's stateful and holds on to `db`, `file` (`SemanticModel`?)`, `ParsedMOduleRef`, `allow_auto_import` and possibly more

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:49 on 2025-12-22 10:13_

It seems unfortunate that we unconditionally create `Stylist` but only used if `should_auto_import` is true. 


---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:100 on 2025-12-22 10:16_

We should use `module.known` here instead of doing string matching

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:85 on 2025-12-22 10:20_

Have you looked at what we do in completions to import missing symbols?

https://github.com/astral-sh/ruff/blob/fc8122c87d9e778817cbf24437feb7707ea8afee/crates/ty_ide/src/completion.rs#L1293-L1335

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:338 on 2025-12-22 10:21_

Why are those issues specific to tests? Wouldn't we see the same issues with imports being put in weird places in production?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:53 on 2025-12-22 10:24_

How much slower do our inlay hints become due to imports? I wonder if it's time to switch to lazily computing inlay hints.

---

_@MichaReiser reviewed on 2025-12-22 10:26_

Thanks for working on this.

I started reviewing this PR but I don't feel like I've enough context myself to do it successfully. 

Would you mind adding some more details to your summary explaining your approach (and what you looked at)? This would help me avoid some duplicate work

---

_@MatthewMckee4 reviewed on 2025-12-22 11:56_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:338 on 2025-12-22 11:56_

I have removed this setting and fixed the tests, I believe we were just applying the text edits wrong in the tests.

---

_@MatthewMckee4 reviewed on 2025-12-22 11:56_

---

_Review comment by @MatthewMckee4 on `crates/ty_server/src/session/options.rs`:249 on 2025-12-22 11:56_

Have removed it now

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:35 on 2025-12-22 11:57_

I have added a context as the first argument

```rs
struct InlayHintImporterContext<'a, 'db> {
    db: &'db dyn Db,
    file: File,
    dynamic_importer: &'a mut DynamicImporter<'a, 'db>,
}
```

---

_@MatthewMckee4 reviewed on 2025-12-22 11:57_

---

_@MatthewMckee4 reviewed on 2025-12-22 12:05_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:53 on 2025-12-22 12:05_

I have not tested this at all yet.

---

_@MatthewMckee4 reviewed on 2025-12-22 12:09_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:49 on 2025-12-22 12:09_

I will resolve this because I have moved this out of this function and we only make Stylist once.

---

_Comment by @MatthewMckee4 on 2025-12-22 12:21_

@MichaReiser thanks!

I have added more content to the PR description.

---

_@MatthewMckee4 reviewed on 2025-12-22 12:28_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:85 on 2025-12-22 12:28_

Yeah I have.

I took some things from it, but some things are different in that we have less certainty about whether we should qualify the name (i think).

And we want to be sure about whether to "import" of "import from"

---

_@MatthewMckee4 reviewed on 2025-12-22 14:41_

---

_Review comment by @MatthewMckee4 on `crates/ty_ide/src/inlay_hints.rs`:53 on 2025-12-22 14:41_

A small ish test added to the PR description

---

_Review request for @carljm removed by @carljm on 2025-12-23 01:54_

---

_Review requested from @Gankra by @MichaReiser on 2026-01-06 10:18_

---

_@MichaReiser reviewed on 2026-01-09 11:17_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:53 on 2026-01-09 11:17_

The performance result look concerning to me, considering that the file is very small. It might be worth testing with a larger file, to see how that influences the response times.

I still think that, ideally, we do auto imports in a separte resolve request (where we just maintain enough information in the inlay so that we can resolve the auto import later)

---

_Comment by @MichaReiser on 2026-01-09 11:22_

@Gankra you've probably the most context on this PR

---

_Review request for @sharkdp removed by @MichaReiser on 2026-01-09 11:23_

---

_Review request for @dcreager removed by @MichaReiser on 2026-01-09 11:23_

---
