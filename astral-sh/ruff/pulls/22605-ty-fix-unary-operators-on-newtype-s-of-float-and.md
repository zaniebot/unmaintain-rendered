```yaml
number: 22605
title: "[ty] fix unary operators on `NewType`s of `float` and `complex`"
type: pull_request
state: open
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: newtype_unary
created_at: 2026-01-15T17:42:25Z
updated_at: 2026-01-20T18:40:15Z
url: https://github.com/astral-sh/ruff/pull/22605
synced_at: 2026-01-20T18:47:35Z
```

# [ty] fix unary operators on `NewType`s of `float` and `complex`

---

_@oconnor663_

Closes https://github.com/astral-sh/ty/issues/2499.

---

_Review requested from @carljm by @oconnor663 on 2026-01-15 17:42_

---

_Review requested from @AlexWaygood by @oconnor663 on 2026-01-15 17:42_

---

_Review requested from @sharkdp by @oconnor663 on 2026-01-15 17:42_

---

_Review requested from @dcreager by @oconnor663 on 2026-01-15 17:42_

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 17:44_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-15 17:46_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
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
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
- Found 5411 diagnostics
+ Found 5406 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`


```

</details>


No memory usage changes detected ✅



---

_Converted to draft by @oconnor663 on 2026-01-15 17:46_

---

_Comment by @oconnor663 on 2026-01-15 17:47_

Actually this still needs some more work. ~~The following example isn't supported yet~~ (edit: now supported):

```py
from typing import reveal_type, NewType

Foo = NewType('Foo', float)

def f(x: float | Foo):
    print(-x)
```

---

_@chatgpt-codex-connector[bot] reviewed on 2026-01-15 17:50_


### 💡 Codex Review

Here are some automated review suggestions for this pull request.

**Reviewed commit:** `8537aec04d`
    

<details> <summary>ℹ️ About Codex in GitHub</summary>
<br/>

[Your team has set up Codex to review pull requests in this repo](http://chatgpt.com/codex/settings/general). Reviews are triggered when you
- Open a pull request for review
- Mark a draft as ready
- Comment "@codex review".

If Codex has suggestions, it will comment; otherwise it will react with 👍.




Codex can also answer questions or update the PR. Try commenting "@codex address that feedback".
            
</details>

---

_Review comment by @chatgpt-codex-connector[bot] on `crates/ty_python_semantic/src/types/infer/builder.rs`:11749 on 2026-01-15 17:50_

**<sub><sub>![P2 Badge](https://img.shields.io/badge/P2-yellow?style=flat)</sub></sub>  Preserve NewType self-binding for unary dunders**

This new arm always delegates unary ops on `NewType` instances to `concrete_base_type`, which changes how `Self` is bound for dunder methods: `-x` now binds `Self` to the base class, while a direct call `x.__neg__()` still binds `Self` to the `NewType` instance via normal member lookup. For a `NewType` wrapping a class that annotates `__neg__`/`__pos__`/`__invert__` with `typing.Self`, this makes `-x` return the base type instead of the `NewType`, diverging from `x.__neg__()` and from the pre-change behavior. Example: `class C: def __neg__(self) -> Self: ...; NT = NewType('NT', C)`; `NT(C()).__neg__()` remains `NT` but `-NT(C())` would now infer `C`.

Useful? React with 👍 / 👎.

---

_@oconnor663 reviewed on 2026-01-15 18:23_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:11749 on 2026-01-15 18:23_

Hmm, I'm not sure Codex is correct above (need to double check), but it is pointing me to a different issue. This works, because we explicitly added binary operation support:

```py
Foo = NewType("Foo", float)
x = Foo(3.14)
x + x
```

But _this_ doesn't currently work:

```py
x.__add__(x)  # error[invalid-argument-type]: Argument to bound method `__add__` is incorrect
```

---

_@oconnor663 reviewed on 2026-01-17 05:25_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer/builder.rs`:11749 on 2026-01-17 05:25_

Actually, this isn't a `NewType` issue. It doesn't work with `float` either on `main` today ([playground](https://play.ty.dev/bf8006cc-6b5b-4c0d-ae96-21ea80b34dd1)):

```py
def _(x: float, y: float):
    # This is fine.
    x + y

    # error[invalid-argument-type]: Argument to bound method `__add__` is incorrect
    x.__add__(y)
    #         ^ Expected `int`, found `int | float`
```

---

_@oconnor663 reviewed on 2026-01-17 05:28_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types.rs`:3231 on 2026-01-17 05:28_

The previous version of this fix was in `infer_unary_expression_type`, but that failed to handle more complex cases like:

```py
Foo = NewType("Foo", float)

def _(x: Foo | float):
    -x
```

This version of the fix here in `member_lookup_with_policy` handles all cases I'm aware of.

---

_Label `ty` added by @oconnor663 on 2026-01-17 05:29_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2026-01-17 05:29_

---

_Marked ready for review by @oconnor663 on 2026-01-17 05:29_

---

_@chatgpt-codex-connector[bot] reviewed on 2026-01-17 05:33_


### 💡 Codex Review

Here are some automated review suggestions for this pull request.

**Reviewed commit:** `5a6b34d80a`
    

<details> <summary>ℹ️ About Codex in GitHub</summary>
<br/>

[Your team has set up Codex to review pull requests in this repo](http://chatgpt.com/codex/settings/general). Reviews are triggered when you
- Open a pull request for review
- Mark a draft as ready
- Comment "@codex review".

If Codex has suggestions, it will comment; otherwise it will react with 👍.




Codex can also answer questions or update the PR. Try commenting "@codex address that feedback".
            
</details>

---

_Review comment by @chatgpt-codex-connector[bot] on `crates/ty_python_semantic/src/types.rs`:3231 on 2026-01-17 05:33_

**<sub><sub>![P2 Badge](https://img.shields.io/badge/P2-yellow?style=flat)</sub></sub>  Preserve NewType self-binding during member lookup**

Delegating `Type::NewTypeInstance` member lookup to `concrete_base_type` means the descriptor protocol now binds the instance as the base type instead of the `NewType`, so methods/properties annotated with `typing.Self` (or `TypeVar` bound to `Self`) will now resolve to the base class. This is a regression for nominal `NewType` behavior: e.g., with `class C: def copy(self) -> Self: ...; NT = NewType("NT", C)`, `NT(C()).copy()` will now infer `C` instead of `NT`. This only shows up when the base type uses `Self`-style return typing, but it changes type inference for regular attribute access beyond the float/complex unary operator fix.

Useful? React with 👍 / 👎.

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 05:34_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 1 | 4 | 5 |
| `invalid-argument-type` | 1 | 3 | 4 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-assignment` | 0 | 0 | 5 |
| `possibly-missing-attribute` | 0 | 3 | 1 |
| `unused-ignore-comment` | 3 | 0 | 0 |
| `invalid-await` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| **Total** | **5** | **12** | **24** |


**[Full report with detailed diff](https://46a4c18a.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://46a4c18a.ty-ecosystem-ext.pages.dev/timing))



---

_@oconnor663 reviewed on 2026-01-20 18:35_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types.rs`:3231 on 2026-01-20 18:35_

This was a good catch. I added a failing test case in a1c1863bd397cb54e6c732c30b9e3550f50123e0 and fixed it in 82cdbfc32e722bcf75a48b50cfb4d41f3110d0f1. In short, I think this special case should _only_ apply to the `float` and `complex` unions.

---

_Comment by @oconnor663 on 2026-01-20 18:38_

This should be ready for proper review now :)

---
