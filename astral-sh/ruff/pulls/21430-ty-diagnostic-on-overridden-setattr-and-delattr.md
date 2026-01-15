```yaml
number: 21430
title: "[ty] diagnostic on overridden `__setattr__` and `__delattr__` in frozen dataclasses"
type: pull_request
state: open
author: thejchap
labels:
  - ty
assignees: []
base: main
head: thejchap/frozen-setattr
created_at: 2025-11-13T14:48:35Z
updated_at: 2026-01-15T12:28:14Z
url: https://github.com/astral-sh/ruff/pull/21430
synced_at: 2026-01-15T12:53:10Z
```

# [ty] diagnostic on overridden `__setattr__` and `__delattr__` in frozen dataclasses

---

_@thejchap_

## Summary
https://github.com/astral-sh/ty/issues/111

this pr adds a `unsound-dataclass-method-override` diagnostic when a custom `__setattr__` or `__delattr__` is defined on a dataclass where `frozen=True` ([docs](https://docs.python.org/3/library/dataclasses.html#frozen-instances))

### Runtime exception
```
Traceback (most recent call last):
  File "/Users/justinchapman/src/ty-playground/main.py", line 4, in <module>
    @dataclass(frozen=True)
     ~~~~~~~~~^^^^^^^^^^^^^
  File "/Users/justinchapman/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/lib/python3.13/dataclasses.py", line 1295, in wrap
    return _process_class(cls, init, repr, eq, order, unsafe_hash,
                          frozen, match_args, kw_only, slots,
                          weakref_slot)
  File "/Users/justinchapman/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/lib/python3.13/dataclasses.py", line 1157, in _process_class
    func_builder.add_fns_to_class(cls)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^
  File "/Users/justinchapman/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/lib/python3.13/dataclasses.py", line 516, in add_fns_to_class
    raise TypeError(error_msg)
TypeError: Cannot overwrite attribute __setattr__ in class A
```

### Diagnostic
```
error[cannot-overwrite-attribute]: Cannot overwrite attribute __setattr__ in class A
 --> /Users/justinchapman/src/ty-playground/main.py:6:5
  |
4 | @dataclass(frozen=True)
5 | class A:
6 |     def __setattr__(self, name: str, value: object) -> None: ...
  |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
info: __setattr__
info: rule `cannot-overwrite-attribute` is enabled by default

Found 1 diagnostic
```

## Test Plan
- new mdtests
- e2e
- the `attrs` mypy primer diff looks to be a [true positive](https://github.com/python-attrs/attrs/blob/main/tests/test_setattr.py#L373) - the other results have been unpredictable and have changed every time i pushed new code, even if the diagnostic logic didn't change...

---

_@thejchap reviewed on 2025-11-13 14:48_

---

_Review comment by @thejchap on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclasses.md`:390 on 2025-11-13 14:48_

TODO: move to method definition

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 14:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 14:51_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ tests/test_setattr.py:374:17: error[invalid-dataclass-override] Cannot overwrite attribute `__setattr__` in class `CustomSetAttr`
- Found 616 diagnostics
+ Found 617 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
- src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, None | Unknown]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
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

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1837 diagnostics
+ Found 1838 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5133 diagnostics
+ Found 5134 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @thejchap on 2025-12-08 00:39_

---

_Review requested from @carljm by @thejchap on 2025-12-08 00:39_

---

_Review requested from @AlexWaygood by @thejchap on 2025-12-08 00:39_

---

_Review requested from @sharkdp by @thejchap on 2025-12-08 00:39_

---

_Review requested from @dcreager by @thejchap on 2025-12-08 00:39_

---

_Review requested from @MichaReiser by @thejchap on 2025-12-08 00:39_

---

_Label `ty` added by @AlexWaygood on 2025-12-08 07:39_

---

_Comment by @sharkdp on 2025-12-09 08:30_

Thank you for your contribution. I did not have time to review this in detail yet. One question upfront: would it be reasonable to use the existing `invalid-override` diagnostic here instead?

---

_Comment by @thejchap on 2025-12-09 14:55_

@sharkdp i am not sure - one thing that seems like it could be nice over time is to have better diagnostic info for this specific case (for example highlighting the `frozen` argument similar to the runtime error) which wouldn't be applicable to the more generic LSP violation that `invalid-method-override` is referring to

i will defer to you though - does seem reasonable!

---

_Comment by @AlexWaygood on 2025-12-09 15:16_

I think it's better to have a different error code here. `invalid-method-override` is specifically about Liskov violations, which this isn't. I've been wondering about renaming it to `unsound`-method-override, since there are obviously lots of other ways in which method overrides can be invalid.

---

_Comment by @AlexWaygood on 2025-12-09 15:19_

I think it would be good to give the error code a name that makes it clear it's specifically about dataclasses, though, which the documentation of the rule makes clear

---

_Comment by @carljm on 2025-12-09 22:12_

We may add similar rules in future for things you're not allowed to override on a tuple subclass. Would we want those to use the same rule as the dataclass case, or a distinct rule for tuples? That may influence how we name this rule.

---

_Comment by @carljm on 2025-12-09 22:13_

(Having said that, I think maybe a different rule, since the tuple case won't be things that raise at runtime, just things that could cause unsoundness. So it would be reasonable to want to disable the one rule but not the other; and they should maybe have different severities. So I think I'd support naming this rule in a dataclass-specific way.)

---

_Converted to draft by @thejchap on 2025-12-11 02:44_

---

_Comment by @thejchap on 2025-12-11 03:23_

@AlexWaygood took another stab at naming based on how i interpreted your and Carl's comments

cc @carljm - i wasn't sure the best way to pull these [comments](https://github.com/astral-sh/ruff/pull/21820/files#diff-a4d813e6b59314ddbc2448d1133969548760611a9de2d07a0c36a0cbe2c93ed3R2352) over from #21820. i did refactor `has_dataclass_param` to be a method on `ClassLiteral` instead of a closure in `own_synthesized_member`, but i didn't change any of the actual logic - if you want to elaborate a bit more on the refactor you had in mind i can either try to implement it here, or just add a comment

---

_Marked ready for review by @thejchap on 2025-12-11 03:23_

---

_Comment by @AlexWaygood on 2025-12-18 16:28_

Thank you! Sorry for the back-and-forth on naming... maybe `invalid-dataclass-override` might be good?
- The name clearly indicates that it's dataclass-related
- But it's a bit shorter than `unsound-dataclass-method-override`
- And I'm not sure "unsound" is the best adjective to use here, because to me it implies that this is detecting sort-of an abstract theoretical issue rather than something that would raise `TypeError` at runtime. That's why I _want_ to rename Liskov violations to `unsound-method-override` -- because the error code that detects Liskov violations _is_ kinda abstract and theoretical. But this error code is _different_ to the Liskov error code -- it's not really abstract and theoretical, it's detecting something that's going to cause `TypeError` to always be raised at runtime. So the stronger adjective `invalid` seems more fitting here.

---

_Comment by @thejchap on 2025-12-22 06:37_

@AlexWaygood ah! thanks for clarifying. sounds good - done

---

_Comment by @carljm on 2025-12-24 02:00_

@thejchap sorry for the slow response time! We've been getting swamped since the beta and this got lost in my notifications. I would say don't worry about those comments for this PR, let's just get the functionality right, better to separate things when separable. Those comments are mostly about making sure we handle various kinds of dataclass-transforms correctly, and that's kind of orthogonal to this PR anyway.

---

_Comment by @thejchap on 2025-12-24 23:32_

Sounds good - I think this is ready for re-review then - let me know if I'm missing anything!

---

_Comment by @MichaReiser on 2025-12-29 09:49_

Thank you for your work on feature. Almost the entire team is out this week. It may take a few days before someone finds time to review your PR. Happy holidays.

---

_Comment by @thejchap on 2025-12-29 19:30_

@MichaReiser thanks! Happy holidays 

---

_Comment by @thejchap on 2026-01-15 01:12_

@AlexWaygood @MichaReiser hi! Just wanted to check on this one - I had been interested in continuing down this road on the diagnostics for overridden comparison dunder methods on order=True dataclasses as well so was curious on any feedback on the direction in this PR

---

_Comment by @AlexWaygood on 2026-01-15 12:28_

Sorry, we still have a bit of a PR backlog from the Christmas holidays :-(

I haven't forgotten about this PR and I promise we will get to it. I've been trying to munch through review of as many PRs as possible this week.

---
