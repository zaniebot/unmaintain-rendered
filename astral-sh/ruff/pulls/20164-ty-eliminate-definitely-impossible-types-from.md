```yaml
number: 20164
title: "[ty]eliminate definitely-impossible types from union in equality narrowing"
type: pull_request
state: merged
author: Renkai
labels:
  - ty
assignees: []
merged: true
base: main
head: renkai-equality-narrowing
created_at: 2025-08-30T02:23:08Z
updated_at: 2025-09-03T15:34:23Z
url: https://github.com/astral-sh/ruff/pull/20164
synced_at: 2026-01-12T15:56:55Z
```

# [ty]eliminate definitely-impossible types from union in equality narrowing

---

_@Renkai_

solves https://github.com/astral-sh/ty/issues/939

---

_Comment by @Renkai on 2025-08-30 02:55_

I want to see more logs to decide where to start, but it seems
```
 TY_LOG=trace cargo test -vvv -p ty_python_semantic -- mdtest__narrow_conditionals_in  --nocapture
```

doesn't show more logs than

```
cargo test -p ty_python_semantic -- mdtest__narrow_conditionals_in
```
Any thing wrong with my parameters?  @AlexWaygood It could be great if you can provide more tricks

---

_Comment by @Renkai on 2025-08-30 07:58_

```
cargo run --bin ty -- check --project ~/renkai-lab/playty -vv
```
can have some logs, can we also make them available for `cargo test`?

---

_Comment by @github-actions[bot] on 2025-09-01 03:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-01 03:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/_check/error/_pep/pep484585/errpep484585container.py:62:9: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[HintSign, range].__getitem__(key: HintSign, /) -> range` cannot be called with key of type `HintSign | None` on object of type `dict[HintSign, range]`
- beartype/_check/error/_pep/pep484585/errpep484585mapping.py:56:9: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[HintSign, range].__getitem__(key: HintSign, /) -> range` cannot be called with key of type `HintSign | None` on object of type `dict[HintSign, range]`
- Found 590 diagnostics
+ Found 588 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/contrib/models/efficient_vit/nn/act.py:43:19: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, @Todo(unsupported type[X] special form)].__getitem__(key: str, /) -> @Todo(unsupported type[X] special form)` cannot be called with key of type `str | None` on object of type `dict[str, @Todo(unsupported type[X] special form)]`
- Found 809 diagnostics
+ Found 808 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/http.py:391:40: error[invalid-argument-type] Argument to function `unquote` is incorrect: Expected `str`, found `None | Unknown | str`
- src/werkzeug/http.py:553:34: error[invalid-argument-type] Argument to function `unquote` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | None`
- Found 369 diagnostics
+ Found 367 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/appinfo.py:458:16: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["bot"]` with `list[str] | None | Any | list[@Todo(list literal element type)]`
- discord/appinfo.py:464:55: warning[possibly-unbound-attribute] Attribute `value` on type `Permissions | None | Any` is possibly unbound
- discord/appinfo.py:478:16: error[unsupported-operator] Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["bot"]` with `list[str] | None | Any | list[@Todo(list literal element type)]`
- discord/appinfo.py:484:54: warning[possibly-unbound-attribute] Attribute `value` on type `Permissions | None | Any` is possibly unbound
- discord/guild.py:3418:37: warning[possibly-unbound-attribute] Attribute `id` on type `Snowflake | None | Any` is possibly unbound
+ discord/http.py:274:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 531 diagnostics
+ Found 527 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_highlevel_serve_listeners.py:52:25: error[invalid-argument-type] Method `__getitem__` of type `bound method Mapping[int, str].__getitem__(key: int, /) -> str` cannot be called with key of type `int | None` on object of type `Mapping[int, str]`
- src/trio/_highlevel_serve_listeners.py:53:37: error[invalid-argument-type] Argument to function `strerror` is incorrect: Expected `int`, found `int | None`
- Found 683 diagnostics
+ Found 681 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/v1/types.py:704:12: error[unsupported-operator] Operator `>=` is not supported for types `str` and `int`, in comparing `int | Literal["n", "N", "F"]` with `Literal[0]`
- pydantic/v1/types.py:706:22: error[unsupported-operator] Operator `+` is unsupported between objects of type `int` and `int | Literal["n", "N", "F"]`
- pydantic/v1/types.py:714:20: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
- pydantic/v1/types.py:715:41: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
- pydantic/v1/types.py:715:41: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
- pydantic/v1/types.py:718:32: error[invalid-argument-type] Argument to function `abs` is incorrect: Expected `SupportsAbs[Unknown]`, found `int | Literal["n", "N", "F"]`
- Found 768 diagnostics
+ Found 762 diagnostics

ignite (https://github.com/pytorch/ignite)
- tests/ignite/distributed/utils/__init__.py:154:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["xla-tpu"]`
+ tests/ignite/distributed/utils/__init__.py:154:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["xla-tpu"]`
- tests/ignite/distributed/utils/__init__.py:157:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["horovod"]`
+ tests/ignite/distributed/utils/__init__.py:157:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["horovod"]`
- tests/ignite/distributed/utils/__init__.py:258:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["horovod"]`
+ tests/ignite/distributed/utils/__init__.py:258:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["horovod"]`
- tests/ignite/distributed/utils/__init__.py:286:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `str | None` with `Literal["xla-tpu"]`
+ tests/ignite/distributed/utils/__init__.py:286:10: error[unsupported-operator] Operator `in` is not supported for types `None` and `str`, in comparing `None | str` with `Literal["xla-tpu"]`

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
- flake8_pyi/visitor.py:958:43: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | None`
- Found 9 diagnostics
+ Found 8 diagnostics

apprise (https://github.com/caronc/apprise)
- apprise/config/base.py:1171:37: error[invalid-argument-type] Argument to function `_special_token_handler` is incorrect: Expected `str`, found `None | Unknown | str`
- apprise/config/base.py:1184:29: error[invalid-argument-type] Argument to function `_special_token_handler` is incorrect: Expected `str`, found `None | Unknown | str`
- Found 1800 diagnostics
+ Found 1798 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/sources/DataSourceVMware.py:995:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[str, Interface]].__getitem__(key: str, /) -> dict[str, Interface]` cannot be called with key of type `None | Unknown` on object of type `dict[str, dict[str, Interface]]`
- cloudinit/sources/DataSourceVMware.py:1003:17: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[str, Interface]].__getitem__(key: str, /) -> dict[str, Interface]` cannot be called with key of type `None | Unknown` on object of type `dict[str, dict[str, Interface]]`
- Found 632 diagnostics
+ Found 630 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/util/ssl_.py:280:35: error[no-matching-overload] No overload of bound method `get` matches arguments
- src/urllib3/util/ssl_.py:283:35: error[no-matching-overload] No overload of bound method `get` matches arguments
- src/urllib3/util/ssl_.py:301:9: error[invalid-assignment] Object of type `int | (Unknown & ~None)` is not assignable to attribute `minimum_version` of type `TLSVersion`
+ src/urllib3/util/ssl_.py:301:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `minimum_version` of type `TLSVersion`
- src/urllib3/util/ssl_.py:306:9: error[invalid-assignment] Object of type `int | (Unknown & ~None)` is not assignable to attribute `maximum_version` of type `TLSVersion`
+ src/urllib3/util/ssl_.py:306:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `maximum_version` of type `TLSVersion`
- Found 392 diagnostics
+ Found 390 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/webserver/lib/dragonnet.py:82:120: error[invalid-argument-type] Argument to function `verification_storage_location` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/webserver/lib/dragonnet.py:89:79: error[invalid-argument-type] Argument to function `add_receipt` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/webserver/lib/dragonnet.py:93:124: error[invalid-argument-type] Argument to function `set_receieved_verification_for_block_from_chain_sync` is incorrect: Expected `str`, found `str | Unknown | None`
- Found 306 diagnostics
+ Found 303 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/config/__init__.py:498:25: warning[possibly-unbound-attribute] Attribute `replace` on type `str | None` is possibly unbound
- src/_pytest/pathlib.py:128:5: error[call-non-callable] Object of type `None` is not callable
- Found 471 diagnostics
+ Found 469 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/proxy/events.py:92:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Command, type[CommandCompleted]].__getitem__(key: Command, /) -> type[CommandCompleted]` cannot be called with key of type `Any | None` on object of type `dict[Command, type[CommandCompleted]]`
- Found 1825 diagnostics
+ Found 1824 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/plotting/_renderer.py:211:42: error[unsupported-operator] Operator `+` is unsupported between objects of type `str` and `str | None`
- src/bokeh/plotting/_renderer.py:212:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `str` and `str | None`
- src/bokeh/plotting/_renderer.py:216:28: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `str | None` on object of type `dict[str, Any]`
- Found 845 diagnostics
+ Found 842 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/upstream/addbook.py:383:16: warning[possibly-unbound-attribute] Attribute `startswith` on type `str | None` is possibly unbound
- Found 711 diagnostics
+ Found 710 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/context.py:944:36: error[invalid-argument-type] Method `__getitem__` of type `bound method ProfilesCollection.__getitem__(name: str) -> Profile` cannot be called with key of type `Unknown | str | None` on object of type `ProfilesCollection`
+ src/prefect/flows.py:3093:28: warning[redundant-cast] Value is already of type `str`
+ src/prefect/flows.py:3106:28: warning[redundant-cast] Value is already of type `str`
- Found 3020 diagnostics
+ Found 3021 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/redis_utils.py:47:34: error[not-iterable] Object of type `list[Unknown] | dict[Unknown, Unknown] | str | None` may not be iterable
- ddtrace/contrib/internal/redis_utils.py:51:34: error[not-iterable] Object of type `list[Unknown] | dict[Unknown, Unknown] | str | None` may not be iterable
- ddtrace/contrib/internal/valkey_utils.py:56:34: error[not-iterable] Object of type `list[Unknown] | dict[Unknown, Unknown] | str | None` may not be iterable
- ddtrace/contrib/internal/valkey_utils.py:60:34: error[not-iterable] Object of type `list[Unknown] | dict[Unknown, Unknown] | str | None` may not be iterable
- Found 6611 diagnostics
+ Found 6607 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/api/v1/schemas.py:2585:41: warning[possibly-unbound-attribute] Attribute `name` on type `Location | None` is possibly unbound
- Found 1596 diagnostics
+ Found 1595 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/amberelectric/helpers.py:19:16: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, str].__getitem__(key: str, /) -> str` cannot be called with key of type `Unknown | None` on object of type `dict[str, str]`
- homeassistant/components/hue/v1/device_trigger.py:140:23: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[tuple[str, str], dict[str, int]]].__getitem__(key: str, /) -> dict[tuple[str, str], dict[str, int]]` cannot be called with key of type `str | None` on object of type `dict[str, dict[tuple[str, str], dict[str, int]]]`
- homeassistant/components/hue/v1/device_trigger.py:191:29: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[tuple[str, str], dict[str, int]]].__getitem__(key: str, /) -> dict[tuple[str, str], dict[str, int]]` cannot be called with key of type `str | None` on object of type `dict[str, dict[tuple[str, str], dict[str, int]]]`
- homeassistant/components/mqtt/config_flow.py:822:46: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, set[type[StrEnum] | str | None]].__getitem__(key: str, /) -> set[type[StrEnum] | str | None]` cannot be called with key of type `Any | None` on object of type `dict[str, set[type[StrEnum] | str | None]]`
- homeassistant/components/network/__init__.py:90:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | None | @Todo(Subscript expressions on intersections)`
- homeassistant/components/number/__init__.py:469:36: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[NumberDeviceClass, type[BaseUnitConverter]].__getitem__(key: NumberDeviceClass, /) -> type[BaseUnitConverter]` cannot be called with key of type `NumberDeviceClass | None` on object of type `dict[NumberDeviceClass, type[BaseUnitConverter]]`
- homeassistant/components/opentherm_gw/climate.py:157:29: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[5]`
- homeassistant/components/opentherm_gw/climate.py:158:29: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | None` and `Literal[5]`
- homeassistant/components/screenlogic/coordinator.py:39:16: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, dict[str, Any]].__getitem__(key: str, /) -> dict[str, Any]` cannot be called with key of type `str | None` on object of type `dict[str, dict[str, Any]]`
+ homeassistant/components/sonos/media.py:51:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- homeassistant/components/tibber/sensor.py:556:33: warning[possibly-unbound-attribute] Attribute `replace` on type `str | None` is possibly unbound
- Found 13344 diagnostics
+ Found 13335 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/arrays/timedeltas.py:1116:49: error[invalid-argument-type] Argument to function `_ints_to_td64ns` is incorrect: Expected `str`, found `None | @Todo(Inference of subscript on special form)`
- Found 3380 diagnostics
+ Found 3379 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/core/expr.py:3304:36: error[unsupported-operator] Operator `**` is unsupported between objects of type `Unknown | None` and `Unknown | Literal[6]`
- sympy/core/expr.py:3317:37: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `Unknown | None`
- sympy/core/expr.py:3317:69: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `Unknown | None`
- sympy/core/exprtools.py:370:23: warning[possibly-unbound-attribute] Attribute `copy` on type `(Unknown & ~Factors & ~Literal[0] & ~Expr) | None | (Basic & ~Expr)` is possibly unbound
+ sympy/crypto/crypto.py:2257:12: warning[possibly-unbound-attribute] Attribute `join` on type `int | Unknown` is possibly unbound
- sympy/utilities/tests/test_lambdify.py:1176:12: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, tuple[() -> str | None] | tuple[int, int | float | None, list[str], str]].__getitem__(key: str, /) -> tuple[() -> str | None] | tuple[int, int | float | None, list[str], str]` cannot be called with key of type `str | None` on object of type `dict[str, tuple[() -> str | None] | tuple[int, int | float | None, list[str], str]]`
- Found 13404 diagnostics
+ Found 13400 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/integrate/_ivp/ivp.py:623:14: error[call-non-callable] Object of type `Literal["RK45"]` is not callable
- scipy/odr/_odrpack.py:1006:21: error[unsupported-operator] Operator `*` is unsupported between objects of type `@Todo(list literal element type) | None` and `Literal[10000]`
- scipy/odr/_odrpack.py:1006:38: error[unsupported-operator] Operator `*` is unsupported between objects of type `@Todo(list literal element type) | None` and `Literal[1000]`
- scipy/odr/_odrpack.py:1007:21: error[unsupported-operator] Operator `*` is unsupported between objects of type `@Todo(list literal element type) | None` and `Literal[100]`
- scipy/odr/_odrpack.py:1007:36: error[unsupported-operator] Operator `*` is unsupported between objects of type `@Todo(list literal element type) | None` and `Literal[10]`
- scipy/odr/_odrpack.py:1083:48: error[unsupported-operator] Operator `*` is unsupported between objects of type `@Todo(list literal element type) | None` and `Literal[10]`
- scipy/sparse/_compressed.py:507:16: error[unsupported-operator] Operator `%` is unsupported between objects of type `Unknown | None` and `Literal[2]`
- scipy/sparse/linalg/_norm.py:184:17: error[unsupported-operator] Operator `+` is unsupported between objects of type `(Unknown & ~Literal[0] & ~Literal[1]) | None` and `Literal[1]`
- scipy/sparse/linalg/_norm.py:187:57: error[unsupported-operator] Operator `/` is unsupported between objects of type `Literal[1]` and `(Unknown & ~Literal[0] & ~Literal[1]) | None`
- Found 6432 diagnostics
+ Found 6423 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @Renkai on 2025-09-01 12:01_

---

_Review requested from @carljm by @Renkai on 2025-09-01 12:01_

---

_Review requested from @AlexWaygood by @Renkai on 2025-09-01 12:01_

---

_Review requested from @sharkdp by @Renkai on 2025-09-01 12:01_

---

_Review requested from @dcreager by @Renkai on 2025-09-01 12:01_

---

_Renamed from "[ty][WIP] eliminate definitely-impossible types from union in equality narrowing" to "[ty]eliminate definitely-impossible types from union in equality narrowing" by @Renkai on 2025-09-01 23:31_

---

_Label `ty` added by @AlexWaygood on 2025-09-02 22:06_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:650 on 2025-09-03 00:20_

It looks like we now have three copies of this identical logic -- can we extract a helper function instead?

Actually I wonder if we even need this logic anymore at all, now that our iteration support has improved (and already special cases string literals). It seems like this could all be replaced with something like `rhs_ty.try_iterate(db).ok()?.homogeneous_element_type(db)`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1064 on 2025-09-03 00:21_

We include `bool` and `LiteralString` here as types that make a type be a "union with single valued", but where we actually use this method, we only really handle unions, and we don't do any special handling for `bool` or `LiteralString`. So I don't think there's currently any benefit to including them here.

And probably enums should be considered as well as an effective "union of single-valued types" both here and above.

Maybe we could at least add some tests that have `bool`, `LiteralString` and enum types on the LHS -- I think that would illuminate cases that we'd want to handle. They could stay TODOs in this PR, if they aren't easy to handle.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:626 on 2025-09-03 00:26_

I think that our handling of `==` narrowing is also missing this same logic, when the LHS isn't all single-valued types, but does contain some single-valued types that could be eliminated.

Really, `==` and `in` narrowing are closely related: `==` is equivalent to `in` with just a single element on the RHS. Right now are implementations of them are totally separate. Is it possible that we could add support for this case to our `==` narrowing, and then re-implement our `in` narrowing in terms of repeated application of our `==` narrowing?

One last thought: rather than using all of `is_single_valued` and `is_union_of_single_valued` and now also `is_union_with_single_valued`, where the latter two each also have complicated semantics involving special handling of `bool` and `LiteralString` (and probably also _should_ include enum types, but don't), I think it might be clearer if we just eliminate those methods and use direct matches on `lhs_ty` here?

---

_@carljm requested changes on 2025-09-03 00:42_

This looks great, thank you! I have a few comments: some that I think should definitely be addressed in this PR (avoiding tripled code that can be simplified, adding at least some tests for bool / LiteralString and enums) and some that could be addressed in this PR but could also be follow-ups (actually supporting bool/LiteralString/enum, the unification with `==` narrowing).

---

_@Renkai reviewed on 2025-09-03 13:31_

---

_Review comment by @Renkai on `crates/ty_python_semantic/src/types/narrow.rs`:626 on 2025-09-03 13:31_

I tried use the in/not_in to replace the eq/not_eq, got many errors(11) include
```
  crates/ty_python_semantic/resources/mdtest/scopes/global.md:84 unexpected error: [invalid-assignment] "Object of type `int | None` is not assignable to `int`"
  
    crates/ty_python_semantic/resources/mdtest/type_compendium/integer_literals.md:77 unmatched assertion: revealed: int & ~Literal[54165]
  crates/ty_python_semantic/resources/mdtest/type_compendium/integer_literals.md:77 unexpected error: 21 [revealed-type] "Revealed type: `int`"

  crates/ty_python_semantic/resources/mdtest/narrow/while.md:57 unmatched assertion: revealed: Literal[3]
  crates/ty_python_semantic/resources/mdtest/narrow/while.md:57 unexpected error: 21 [revealed-type] "Revealed type: `Literal[1, 2, 3]`"

  ```
  Maybe we shall create a new thread to discuss about how to combine in and eq, and the proposed behavior change of existing eq function.

---

_@Renkai reviewed on 2025-09-03 13:53_

---

_Review comment by @Renkai on `crates/ty_python_semantic/src/types.rs`:1064 on 2025-09-03 13:53_

https://github.com/astral-sh/ruff/pull/20164/files#diff-4d769c34388fdd9011945264bed8fb2199b60ffefcbaa09d0804e4e6c347fd4dR134
added some TODOs in the in.md. If the tests looks good to you, I will take a look tomorrow to see if it's doable for me in the near future.

---

_@carljm reviewed on 2025-09-03 15:07_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/in.md`:135 on 2025-09-03 15:07_

We don't use HTML comments for TODOs, we are fine with TODOs showing up in the rendered output. In fact we use HTML comments for special markers like `<!-- snapshot-diagnostics -->`, and for that reason to avoid typos we make unknown HTML comments an error, which is why tests are failing on this PR.

I'll fix this so you can see how we usually handle TODOs.

---

_@carljm approved on 2025-09-03 15:30_

Looks good, thank you! Pushed some updates and will go ahead and merge.

I think the highest priority follow-up would be to add enum support and address those TODOs. If you are interested, I think this would not be too hard. In principle it is just adding enums (along with `bool` and `LiteralString`) in `is_union_of_single_valued` and `is_union_with_single_valued`, and in the union logic in `evaluate_expr_in` and `evaluate_expr_not_in`. The one wrinkle is that we only consider enums single-valued if they don't override `__eq__` or `__ne__`. This logic is currently buried inside the `EnumLiteral` branch of `Type::is_single_valued`; we would need to pull it out into a top-level `Type::overrides_eq_or_ne` method, since we would also need to call it from `is_union_of_single_valued` etc. (Should probably also add some tests showing we don't do this narrowing for an enum class that does override `__eq__` or `__ne__`.)

---

_Merged by @carljm on 2025-09-03 15:34_

---

_Closed by @carljm on 2025-09-03 15:34_

---
