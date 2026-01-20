```yaml
number: 22771
title: "[ty] Support `Unpack[TypedDict]` for precise **kwargs typing (PEP 692)"
type: pull_request
state: open
author: bxff
labels:
  - ty
assignees: []
base: main
head: ty/support-typing-unpack
created_at: 2026-01-20T17:57:41Z
updated_at: 2026-01-20T18:19:53Z
url: https://github.com/astral-sh/ruff/pull/22771
synced_at: 2026-01-20T18:40:22Z
```

# [ty] Support `Unpack[TypedDict]` for precise **kwargs typing (PEP 692)

---

_@bxff_

## Summary

Closes https://github.com/astral-sh/ty/issues/1746 Partially addresses https://github.com/astral-sh/ty/issues/154 ("Support for `Unpack`" checkbox)

This PR implements PEP 692 support for using `Unpack[TypedDict]` to provide more precise type annotations for `**kwargs` parameters. Previously, `**kwargs: str` meant all keyword arguments are strings. With this change, you can specify exactly which keyword arguments are expected:

```python
class Movie(TypedDict):
    name: str
    year: int

def foo(**kwargs: Unpack[Movie]) -> None:
    reveal_type(kwargs)  # revealed: Movie
```

Implementation details:
- Add `KnownInstanceType::UnpackedTypedDict(TypedDictType)` variant
- Parse `Unpack[TypedDict]` in type expressions, emit errors for invalid usage
- Type `kwargs` as the TypedDict itself inside function bodies
- Validate keyword arguments against TypedDict fields at call sites
- Report `unknown-argument` for kwargs not in the TypedDict
- Report `missing-argument` for required TypedDict fields not provided
- Check argument types against specific TypedDict field types
- Handle `**typed_dict_var` passthrough with field-by-field type checking

Not yet implemented (out of scope, separate PEP 646 features):
- `Unpack[TypeVarTuple]` - returns `todo_type!`
- `Unpack[tuple[...]]` - returns `todo_type!`

## Test Plan

New mdtest file `annotations/unpack_kwargs.md` with comprehensive coverage:
- Basic usage and `reveal_type` verification
- Required/NotRequired field handling
- Invalid Unpack usage errors (int, regular TypeVar)
- Call binding: valid calls, unknown kwargs, missing required fields
- Type checking for individual field types
- Passing compatible/incompatible TypedDict via `**`
- Passing TypedDict with extra fields via `**`

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

---

_Review requested from @carljm by @bxff on 2026-01-20 17:57_

---

_Review requested from @AlexWaygood by @bxff on 2026-01-20 17:57_

---

_Review requested from @sharkdp by @bxff on 2026-01-20 17:57_

---

_Review requested from @dcreager by @bxff on 2026-01-20 17:57_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 18:00_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/) improved 🎉

The percentage of diagnostics emitted that were expected errors increased from 77.42% to 77.63%. The percentage of expected errors that received a diagnostic increased from 60.49% to 61.21%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 672 | 680 | +8 | ⏫ (✅) |
| False Positives | 196 | 196 | +0 |  |
| False Negatives | 439 | 431 | -8 | ⏬ (✅) |
| Total Diagnostics | 868 | 876 | +8 | ⏫ |
| Precision | 77.42% | 77.63% | +0.21% | ⏫ (✅) |
| Recall | 60.49% | 61.21% | +0.72% | ⏫ (✅) |



### True positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [callables_kwargs.py:101:19](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L101) | invalid-assignment | Object of type `def func1(**kwargs: Unpack[TD2]) -> None` is not assignable to `TDProtocol3` |
| [callables_kwargs.py:102:19](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L102) | invalid-assignment | Object of type `def func1(**kwargs: Unpack[TD2]) -> None` is not assignable to `TDProtocol4` |
| [callables_kwargs.py:122:21](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L122) | invalid-type-form | `Unpack` must be used with a TypedDict, TypeVarTuple, or tuple type, got `T@func6` |
| [callables_kwargs.py:46:5](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L46) | missing-argument | Missing required keyword arguments `v1`, `v3` for function `func1` |
| [callables_kwargs.py:51:32](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L51) | unknown-argument | Argument `v4` does not match any known parameter of function `func1` |
| [callables_kwargs.py:58:5](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L58) | missing-argument | Missing required keyword arguments `v1`, `v3` for function `func1` |
| [typeddicts_extra_items.py:143:48](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/typeddicts_extra_items.py#L143) | unknown-argument | Argument `year` does not match any known parameter of function `unpack_no_extra` |
| [typeddicts_readonly_kwargs.py:33:12](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/typeddicts_readonly_kwargs.py#L33) | invalid-assignment | Cannot assign to key "key1" on TypedDict `ReadOnlyArgs`: key is marked read-only |


</details>

### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [callables_kwargs.py:24:5](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L24) | type-assertion-failure | Type `int` does not match asserted type `@Todo(`Unpack[]` special form)` |
| [callables_kwargs.py:32:9](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L32) | type-assertion-failure | Type `str` does not match asserted type `@Todo(`Unpack[]` special form)` |
| [callables_kwargs.py:35:5](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L35) | type-assertion-failure | Type `str` does not match asserted type `@Todo(`Unpack[]` special form)` |
| [callables_kwargs.py:41:5](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L41) | type-assertion-failure | Type `TD1` does not match asserted type `dict[str, @Todo(`Unpack[]` special form)]` |


</details>

### False positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [callables_kwargs.py:100:19](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L100) | invalid-assignment | Object of type `def func1(**kwargs: Unpack[TD2]) -> None` is not assignable to `TDProtocol2` |
| [callables_kwargs.py:99:19](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L99) | invalid-assignment | Object of type `def func1(**kwargs: Unpack[TD2]) -> None` is not assignable to `TDProtocol1` |
| [typeddicts_extra_items.py:144:45](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/typeddicts_extra_items.py#L144) | unknown-argument | Argument `year` does not match any known parameter of function `unpack_extra` |
| [typeddicts_readonly_kwargs.py:36:16](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/typeddicts_readonly_kwargs.py#L36) | invalid-assignment | Object of type `def impl(**kwargs: Unpack[ReadOnlyArgs]) -> None` is not assignable to `Function` |


</details>

### Optional Diagnostics Added

<details>

| Location | Name | Message |
|----------|------|---------|
| [callables_kwargs.py:61:5](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/tests/callables_kwargs.py#L61) | missing-argument | Missing required keyword arguments `v1`, `v3` for function `func1` |


</details>



---

_Comment by @AlexWaygood on 2026-01-20 18:13_

Hey @bxff -- really appreciate the PRs you've been sending recently! I love how quickly you're knocking down issues, but we still have a bit of a PR backlog from the beta release and the Christmas break. Would you mind holding off on opening any more until we've had a chance to finish reviewing the ones you've already opened? It'll probably make it a better experience for you, because your PRs are less likely to sit unreviewed for extended periods of time (which can lead to annoying merge conflicts for you) :-)

---

_Label `ty` added by @AlexWaygood on 2026-01-20 18:14_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 18:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/context_manager.py:40:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | None`
+ pyinstrument/context_manager.py:40:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | int | float | str | None`
- pyinstrument/context_manager.py:40:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Literal["enabled", "disabled", "strict"]`, found `Unknown | None`
+ pyinstrument/context_manager.py:40:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Literal["enabled", "disabled", "strict"]`, found `Unknown | int | float | str | None`
- pyinstrument/context_manager.py:41:9: error[invalid-assignment] Object of type `dict[str, @Todo]` is not assignable to attribute `options` of type `ProfileContextOptions`
+ pyinstrument/context_manager.py:40:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | None`, found `Unknown | int | float | str | None`

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ aiohttp_devtools/runserver/watch.py:120:55: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `SSLContext | bool | Fingerprint`, found `None | SSLContext`
+ aiohttp_devtools/runserver/watch.py:155:57: error[invalid-argument-type] Argument to bound method `get` is incorrect: Expected `SSLContext | bool | Fingerprint`, found `None | SSLContext`
- Found 35 diagnostics
+ Found 37 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:238:95: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/fields.py:277:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pydantic/fields.py:668:16: error[no-matching-overload] No overload of function `Field` matches arguments
- pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:949:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:989:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1032:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1072:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1115:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1154:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1194:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1286:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/fields.py:1290:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/fields.py:1300:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
- pydantic/fields.py:1310:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/fields.py:1320:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/fields.py:1330:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/fields.py:1343:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/fields.py:1358:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/fields.py:1573:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
- Found 3155 diagnostics
+ Found 3146 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/bot.py:307:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/bot.py:331:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ext/commands/core.py:602:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Unpack[_CommandKwargs]`, found `list[str] | tuple[str, ...] | str | ... omitted 6 union elements`
- discord/ext/commands/core.py:1551:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/core.py:1608:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ext/commands/core.py:1859:12: error[no-matching-overload] No overload of function `command` matches arguments
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `arguments_heading` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `commands_heading` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `default_argument_description` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `dm_help` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `dm_help_threshold` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `indent` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `no_category` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `paginator` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `show_parameter_descriptions` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `sort_commands` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1087:26: error[unknown-argument] Argument `width` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1378:26: error[unknown-argument] Argument `aliases_heading` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1378:26: error[unknown-argument] Argument `commands_heading` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1378:26: error[unknown-argument] Argument `dm_help` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1378:26: error[unknown-argument] Argument `dm_help_threshold` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1378:26: error[unknown-argument] Argument `no_category` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1378:26: error[unknown-argument] Argument `paginator` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/help.py:1378:26: error[unknown-argument] Argument `sort_commands` does not match any known parameter of bound method `__init__`
+ discord/ext/commands/hybrid.py:836:9: error[invalid-method-override] Invalid override of method `command`: Definition is incompatible with `GroupMixin.command`
- discord/ext/commands/hybrid.py:853:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/ext/commands/hybrid.py:860:9: error[invalid-method-override] Invalid override of method `group`: Definition is incompatible with `GroupMixin.group`
- discord/ext/commands/hybrid.py:877:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/hybrid.py:932:92: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/hybrid.py:965:90: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/permissions.py:215:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/permissions.py:483:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/shard.py:379:50: error[unknown-argument] Argument `shard_connect_timeout` does not match any known parameter of bound method `__init__`
+ discord/shard.py:379:50: error[unknown-argument] Argument `shard_ids` does not match any known parameter of bound method `__init__`
- Found 542 diagnostics
+ Found 556 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_subprocess.py:496:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/type_tests/subprocesses.py:15:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/type_tests/subprocesses.py:22:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/type_tests/subprocesses.py:23:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 487 diagnostics
+ Found 483 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
+ lib/Crypto/Protocol/HPKE.py:181:39: error[invalid-argument-type] Argument to function `key_agreement` is incorrect: Expected `Unpack[RequestParams[Unknown]]`, found `Unknown | (EccKey & ~AlwaysFalsy)`
+ lib/Crypto/Protocol/HPKE.py:221:39: error[invalid-argument-type] Argument to function `key_agreement` is incorrect: Expected `Unpack[RequestParams[Unknown]]`, found `Unknown | (EccKey & ~AlwaysFalsy)`
- Found 1326 diagnostics
+ Found 1328 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/tests/cli/configs/test_snowflake.py:100:9: error[unknown-argument] Argument `schema` does not match any known parameter
+ src/integrations/prefect-gitlab/prefect_gitlab/repositories.py:89:32: error[no-matching-overload] No overload of function `Field` matches arguments
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
+ src/prefect/tasks.py:2188:9: error[invalid-method-override] Invalid override of method `with_options`: Definition is incompatible with `Task.with_options`
- Found 5406 diagnostics
+ Found 5412 diagnostics

altair (https://github.com/vega/altair)
- altair/datasets/_constraints.py:57:24: error[invalid-return-type] Return type does not match returned value: expected `Metadata`, found `dict[str, @Todo]`
- altair/datasets/_constraints.py:111:33: error[invalid-argument-type] Argument to bound method `from_metadata` is incorrect: Expected `Metadata`, found `dict[str, @Todo]`
- altair/utils/core.py:676:18: warning[possibly-missing-attribute] Attribute `schema` may be missing on object of type `NativeDataFrame | DataFrameLike | Unknown | None`
- altair/utils/core.py:678:22: error[not-subscriptable] Cannot subscript object of type `NativeDataFrame` with no `__getitem__` method
- altair/utils/core.py:678:22: error[not-subscriptable] Cannot subscript object of type `DataFrameLike` with no `__getitem__` method
- altair/utils/core.py:678:22: error[not-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- altair/utils/core.py:682:39: warning[possibly-missing-attribute] Attribute `to_native` may be missing on object of type `NativeDataFrame | DataFrameLike | Unknown | None`
- altair/utils/data.py:208:28: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(NativeDataFrame & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (narwhals.stable.v1.typing.DataFrameLike & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (Unknown & ~None & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (SupportsGeoInterface & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (altair.utils.core.DataFrameLike & ~DataFrame & ~Top[dict[Unknown, Unknown]])`
- altair/utils/data.py:209:39: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(NativeDataFrame & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (narwhals.stable.v1.typing.DataFrameLike & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (Unknown & ~None & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (SupportsGeoInterface & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (altair.utils.core.DataFrameLike & ~DataFrame & ~Top[dict[Unknown, Unknown]])`
- altair/utils/data.py:415:12: warning[possibly-missing-attribute] Attribute `write_csv` may be missing on object of type `(NativeDataFrame & ~SupportsGeoInterface & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (narwhals.stable.v1.typing.DataFrameLike & ~SupportsGeoInterface & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (Unknown & ~SupportsGeoInterface & ~DataFrame & ~Top[dict[Unknown, Unknown]]) | (altair.utils.core.DataFrameLike & ~SupportsGeoInterface & ~DataFrame & ~Top[dict[Unknown, Unknown]])`
- altair/utils/schemapi.py:509:12: error[invalid-return-type] Return type does not match returned value: expected `list[Any]`, found `object`
- tests/test_transformed_data.py:97:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Any & ~None) | DataFrameLike`
- tests/test_transformed_data.py:98:35: warning[possibly-missing-attribute] Attribute `columns` may be missing on object of type `(Any & ~None) | DataFrameLike`
- Found 1055 diagnostics
+ Found 1042 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
+ hydpy/models/rconc/rconc_control.py:250:23: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 669 diagnostics
+ Found 670 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

pywin32 (https://github.com/mhammond/pywin32)
+ com/win32comext/shell/demos/servers/folder_view.py:863:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `bool`, found `Literal[0]`
+ com/win32comext/shell/demos/servers/shell_view.py:968:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `bool`, found `Literal[0]`
- Found 2694 diagnostics
+ Found 2696 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/io/parsers/readers.py:1574:41: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(@Todo & ~Literal[False] & ~_NoDefault) | None`
+ pandas/io/parsers/readers.py:1574:41: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Hashable & ~Literal[False] & ~_NoDefault`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4454 diagnostics
+ Found 4456 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/db/dbhandler.py:884:65: error[no-matching-overload] No overload of bound method `get_db_key` matches arguments
+ rotkehlchen/db/dbhandler.py:897:59: error[no-matching-overload] No overload of bound method `get_db_key` matches arguments
+ rotkehlchen/db/dbhandler.py:1052:14: error[no-matching-overload] No overload of bound method `get_db_key` matches arguments
+ rotkehlchen/tasks/events.py:507:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/tasks/events.py:510:13: error[invalid-argument-type] Argument to bound method `set_dynamic_cache` is incorrect: Expected `int`, found `Unknown | int | None`
+ rotkehlchen/tasks/events.py:511:13: error[invalid-argument-type] Argument to bound method `set_dynamic_cache` is incorrect: Expected `int`, found `Unknown | int | None`
+ rotkehlchen/tests/db/test_db.py:396:20: error[no-matching-overload] No overload of bound method `get_dynamic_cache` matches arguments
+ rotkehlchen/tests/db/test_db.py:436:20: error[no-matching-overload] No overload of bound method `get_dynamic_cache` matches arguments
- Found 2057 diagnostics
+ Found 2065 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14491 diagnostics
+ Found 14492 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~3MB
+     struct fields = ~4MB
-     memo fields = ~49MB
+     memo fields = ~52MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~108MB
+     memo fields = ~113MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct fields = ~20MB
+     struct fields = ~22MB
-     memo fields = ~176MB
+     memo fields = ~185MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~690MB
+ TOTAL MEMORY USAGE: ~725MB
-     struct fields = ~54MB
+     struct fields = ~60MB
-     memo fields = ~424MB
+     memo fields = ~445MB


```

</details>




---
