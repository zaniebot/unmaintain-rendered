```yaml
number: 20126
title: "[ty] implement `typing.NewType`"
type: pull_request
state: closed
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: jack/newtype3
created_at: 2025-08-28T07:11:33Z
updated_at: 2025-10-31T02:27:21Z
url: https://github.com/astral-sh/ruff/pull/20126
synced_at: 2026-01-10T16:59:49Z
```

# [ty] implement `typing.NewType`

---

_Pull request opened by @oconnor663 on 2025-08-28 07:11_

https://github.com/astral-sh/ty/issues/742

This is an initial draft of a working implementation of `NewType`. New test cases are in `new_types.md`, in particular:

```py
from typing_extensions import NewType

Foo = NewType("Foo", int)
Bar = NewType("Bar", Foo)

Foo(42)
Foo(Foo(42))       # allowed: `Foo` is a subtype of `int`.
Foo(Bar(Foo(42)))  # allowed: `Bar` is a subtype of `int`.
Foo(True)          # allowed: `bool` is a subtype of `int`.
Foo("fourty-two")  # error: [invalid-argument-type]

def f(_: int): ...
f(42)
f(Foo(42))
f(Bar(Foo(42)))

def g(_: Foo): ...
g(42)  # error: [invalid-argument-type]
g(Foo(42))
g(Bar(Foo(42)))

def h(_: Bar): ...
h(42)       # error: [invalid-argument-type]
h(Foo(42))  # error: [invalid-argument-type]
h(Bar(Foo(42)))
```

@AlexWaygood suggested a substantially different approach involving a new `Type::NewTypeInstance` variant rather than what this PR currently does, which is adding a new `NominalInstanceType::NewType` variant. I haven't yet tried that, and I don't have much intuition of my own about which is better, but I had this close to working, and I wanted to get some early feedback on it before I tried to do an overhaul.

That said, my baby daughter was born this evening (happy, healthy :heart:),  so it will be a few days or weeks before I get back to this.

---

_Comment by @github-actions[bot] on 2025-08-28 07:13_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-28 07:13:12.704519896 +0000
+++ new-output.txt	2025-08-28 07:13:15.228529548 +0000
@@ -46,10 +46,15 @@
 aliases_implicit.py:119:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
 aliases_implicit.py:128:1: error[type-assertion-failure] Argument does not have asserted type `list[str]`
 aliases_implicit.py:133:6: error[call-non-callable] Object of type `UnionType` is not callable
-aliases_newtype.py:15:1: error[type-assertion-failure] Argument does not have asserted type `int`
-aliases_newtype.py:18:1: error[invalid-assignment] Object of type `NewType` is not assignable to `type`
-aliases_newtype.py:26:21: error[invalid-base] Invalid class base with type `NewType`
-aliases_newtype.py:63:43: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 3, got 4
+aliases_newtype.py:11:8: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["user"]`
+aliases_newtype.py:12:1: error[invalid-assignment] Object of type `Literal[42]` is not assignable to `UserId`
+aliases_newtype.py:18:1: error[invalid-assignment] Object of type `UserId` is not assignable to `type`
+aliases_newtype.py:26:21: error[invalid-base] Invalid class base with type `UserId`
+aliases_newtype.py:41:6: error[invalid-type-form] `GoodNewType1` is a `NewType` and cannot be specialized
+aliases_newtype.py:47:15: error[invalid-newtype] The second argument to `typing.NewType` must be a class or another `NewType`
+aliases_newtype.py:54:15: error[invalid-newtype] The second argument to `typing.NewType` must be a class or another `NewType`
+aliases_newtype.py:63:43: error[too-many-positional-arguments] Too many positional arguments to class `NewType`: expected 2, got 3
+aliases_newtype.py:65:15: error[invalid-newtype] The second argument to `typing.NewType` must be a class or another `NewType`
 aliases_type_statement.py:17:1: error[unresolved-attribute] Type `typing.TypeAliasType` has no attribute `bit_count`
 aliases_type_statement.py:19:1: error[call-non-callable] Object of type `TypeAliasType` is not callable
 aliases_type_statement.py:23:7: error[unresolved-attribute] Type `typing.TypeAliasType` has no attribute `other_attrib`
@@ -515,7 +520,7 @@
 generics_typevartuple_basic.py:12:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:16:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_basic.py:23:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_basic.py:42:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[@Todo(PEP 646), ...]`, found `Literal[1]`
+generics_typevartuple_basic.py:42:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[@Todo(PEP 646), ...]`, found `Height`
 generics_typevartuple_basic.py:65:27: error[unknown-argument] Argument `covariant` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:66:27: error[too-many-positional-arguments] Too many positional arguments to function `__new__`: expected 2, got 4
 generics_typevartuple_basic.py:67:27: error[unknown-argument] Argument `bound` does not match any known parameter of function `__new__`
@@ -859,5 +864,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 860 diagnostics
+Found 865 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-28 07:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
paroxython (https://github.com/laowantong/paroxython)
+ paroxython/__init__.py:55:31: error[invalid-argument-type] Argument to function `main` is incorrect: Expected `Source`, found `str`
- Found 5 diagnostics
+ Found 6 diagnostics

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
+ test/test_generated_mypy.py:484:5: error[type-assertion-failure] Argument does not have asserted type `UserId`
- test/test_generated_mypy.py:486:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[Unknown, Email]`
+ test/test_generated_mypy.py:486:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[UserId, Email]`
+ test/test_generated_mypy.py:495:5: error[type-assertion-failure] Argument does not have asserted type `UserId`
- test/test_generated_mypy.py:497:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[Unknown, Email]`
+ test/test_generated_mypy.py:497:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[UserId, Email]`
- Found 56 diagnostics
+ Found 58 diagnostics

rich (https://github.com/Textualize/rich)
+ tests/test_progress.py:57:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:66:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:71:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:78:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:88:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:92:28: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:125:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:138:17: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:154:22: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
+ tests/test_progress.py:162:22: error[invalid-argument-type] Argument is incorrect: Expected `TaskID`, found `Literal[1]`
- Found 310 diagnostics
+ Found 320 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/server.py:110:17: error[unresolved-attribute] Type `(ServerProtocol, Sequence[Unknown], /) -> Unknown | None` has no attribute `__get__`
+ src/websockets/server.py:110:17: error[unresolved-attribute] Type `(ServerProtocol, Sequence[Subprotocol], /) -> Subprotocol | None` has no attribute `__get__`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/embed/util.py:199:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[Model, Unknown]`, found `(list[Model] & ~list[Unknown]) | dict[Model, Unknown] | dict[Unknown, Unknown]`
+ src/bokeh/embed/util.py:199:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[Model, ID]`, found `(list[Model] & ~list[Unknown]) | dict[Model, ID] | dict[Unknown, Unknown]`

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/interpreter/interpreter.py:168:98: error[invalid-argument-type] Argument to bound method `single_use` is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/interpreter/interpreter.py:171:76: error[invalid-argument-type] Argument to bound method `single_use` is incorrect: Expected `SubProject`, found `str`
+ mesonbuild/interpreter/interpreter.py:174:82: error[invalid-argument-type] Argument to bound method `single_use` is incorrect: Expected `SubProject`, found `str`
- mesonbuild/interpreterbase/decorators.py:45:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[BaseNode, list[@Todo(Support for `typing.TypeAlias`)], Unknown, Unknown]`, found `tuple[Any, None | Any, None | Any, Any]`
+ mesonbuild/interpreterbase/decorators.py:45:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[BaseNode, list[@Todo(Support for `typing.TypeAlias`)], Unknown, SubProject]`, found `tuple[Any, None | Any, None | Any, Any]`
+ unittests/datatests.py:249:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SubProject`, found `Literal[""]`
+ unittests/platformagnostictests.py:41:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SubProject`, found `Literal[""]`
+ unittests/platformagnostictests.py:75:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SubProject`, found `Literal[""]`
- Found 807 diagnostics
+ Found 813 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/accounting/pot.py:451:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/api/rest.py:2750:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/assets/asset.py:59:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/assets/asset.py:597:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/assets/asset.py:603:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/assets/asset.py:773:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/interfaces/ammswap/ammswap.py:96:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/ethereum/modules/eth2/eth2.py:649:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/ethereum/modules/yearn/decoder.py:145:58: error[invalid-argument-type] Argument to function `_get_vault_token_name` is incorrect: Expected `ChecksumAddress`, found `Unknown | ChecksumAddress | None`
+ rotkehlchen/chain/ethereum/modules/yearn/decoder.py:170:62: error[invalid-argument-type] Argument to function `_get_vault_token_name` is incorrect: Expected `ChecksumAddress`, found `Unknown | ChecksumAddress | None`
- rotkehlchen/chain/ethereum/modules/yearn/decoder.py:403:13: error[invalid-return-type] Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["yearn-v1", "yearn-v2"]]`
+ rotkehlchen/chain/ethereum/modules/yearn/decoder.py:403:13: error[invalid-return-type] Return type does not match returned value: expected `dict[ChecksumAddress, str]`, found `dict[Unknown, Literal["yearn-v1", "yearn-v2"]]`
- rotkehlchen/chain/evm/decoding/aave/v2/decoder.py:137:16: error[invalid-return-type] Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["aave-v2"]]`
+ rotkehlchen/chain/evm/decoding/base.py:141:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[HistoryEventType, HistoryEventSubType, str | None, ChecksumAddress, str, str] | None`, found `tuple[HistoryEventType, HistoryEventSubType, str | None, ChecksumAddress | None, str, str] | None`
- rotkehlchen/chain/evm/decoding/beefy_finance/decoder.py:251:16: error[invalid-return-type] Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["beefy_finance"]]`
+ rotkehlchen/chain/evm/decoding/beefy_finance/decoder.py:251:16: error[invalid-return-type] Return type does not match returned value: expected `dict[ChecksumAddress, str]`, found `dict[Unknown, Literal["beefy_finance"]]`
- rotkehlchen/chain/evm/decoding/compound/v3/decoder.py:407:16: error[invalid-return-type] Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["compound-v3"]]`
+ rotkehlchen/chain/evm/decoding/compound/v3/decoder.py:407:16: error[invalid-return-type] Return type does not match returned value: expected `dict[ChecksumAddress, str]`, found `dict[ChecksumAddress, Literal["compound-v3"]]`
- rotkehlchen/chain/evm/decoding/llamazip/decoder.py:81:16: error[invalid-return-type] Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["llamazip"]]`
+ rotkehlchen/chain/evm/decoding/llamazip/decoder.py:81:16: error[invalid-return-type] Return type does not match returned value: expected `dict[ChecksumAddress, str]`, found `dict[Unknown, Literal["llamazip"]]`
- rotkehlchen/chain/evm/decoding/morpho/decoder.py:331:16: error[invalid-return-type] Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["morpho"]]`
+ rotkehlchen/chain/evm/decoding/morpho/decoder.py:331:16: error[invalid-return-type] Return type does not match returned value: expected `dict[ChecksumAddress, str]`, found `dict[Unknown, Literal["morpho"]]`
- rotkehlchen/chain/evm/decoding/uniswap/v3/decoder.py:575:16: error[invalid-return-type] Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["uniswap-v3"]]`
+ rotkehlchen/chain/evm/decoding/uniswap/v3/decoder.py:575:16: error[invalid-return-type] Return type does not match returned value: expected `dict[ChecksumAddress, str]`, found `dict[Unknown, Literal["uniswap-v3"]]`
- rotkehlchen/chain/evm/decoding/zerox/decoder.py:294:16: error[invalid-return-type] Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["0x"]]`
+ rotkehlchen/chain/evm/decoding/zerox/decoder.py:294:16: error[invalid-return-type] Return type does not match returned value: expected `dict[ChecksumAddress, str]`, found `dict[Unknown, Literal["0x"]]`
- rotkehlchen/chain/evm/names.py:63:94: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/evm/names.py:67:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/data_migrations/migrations/migrations_18.py:56:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/externalapis/etherscan.py:451:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/externalapis/etherscan.py:533:84: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/history/events/structures/evm_event.py:206:63: error[invalid-argument-type] Argument to function `deserialize_optional` is incorrect: Expected `(Unknown | str | None, /) -> Unknown`, found `def string_to_evm_address(value: str) -> Unknown`
+ rotkehlchen/history/events/structures/evm_event.py:206:63: error[invalid-argument-type] Argument to function `deserialize_optional` is incorrect: Expected `(Unknown | str | None, /) -> Unknown`, found `def string_to_evm_address(value: str) -> ChecksumAddress`
- rotkehlchen/history/events/structures/evm_swap.py:104:63: error[invalid-argument-type] Argument to function `deserialize_optional` is incorrect: Expected `(Unknown | str | None, /) -> Unknown`, found `def string_to_evm_address(value: str) -> Unknown`
+ rotkehlchen/history/events/structures/evm_swap.py:104:63: error[invalid-argument-type] Argument to function `deserialize_optional` is incorrect: Expected `(Unknown | str | None, /) -> Unknown`, found `def string_to_evm_address(value: str) -> ChecksumAddress`
+ rotkehlchen/inquirer.py:1049:16: error[invalid-return-type] Return type does not match returned value: expected `Price | None`, found `int | float`
- rotkehlchen/tests/api/test_ens.py:62:94: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/tests/api/test_ens.py:66:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/tests/db/test_db.py:354:9: error[invalid-argument-type] Argument to bound method `add_queried_address_for_module` is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/db/test_db.py:359:13: error[invalid-argument-type] Argument to bound method `save_tokens_for_address` is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/db/test_db.py:365:13: error[invalid-argument-type] Argument to bound method `save_tokens_for_address` is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/db/test_db.py:377:13: error[no-matching-overload] No overload of bound method `set_dynamic_cache` matches arguments
+ rotkehlchen/tests/db/test_db.py:436:13: error[invalid-argument-type] Argument to bound method `get_tokens_for_address` is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/db/test_db.py:442:13: error[invalid-argument-type] Argument to bound method `get_tokens_for_address` is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/db/test_db.py:607:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `int`
+ rotkehlchen/tests/db/test_db.py:724:13: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[1451606401]`
+ rotkehlchen/tests/db/test_db.py:725:13: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[1485907100]`
+ rotkehlchen/tests/db/test_db.py:737:13: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[1451606300]`
+ rotkehlchen/tests/db/test_db.py:738:13: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[1485907000]`
+ rotkehlchen/tests/db/test_db.py:1145:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp | None`, found `Literal[1451606400]`
+ rotkehlchen/tests/db/test_db.py:1146:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1451616500]`
+ rotkehlchen/tests/db/test_db.py:1156:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp | None`, found `Literal[1451626500]`
+ rotkehlchen/tests/db/test_db.py:1157:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1451636500]`
+ rotkehlchen/tests/db/test_db.py:1167:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp | None`, found `Literal[1452636501]`
+ rotkehlchen/tests/db/test_db.py:1168:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1459836501]`
+ rotkehlchen/tests/db/test_db.py:1275:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1281:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1299:17: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1305:17: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1325:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1331:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1337:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676729]`
+ rotkehlchen/tests/db/test_db.py:1343:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676829]`
+ rotkehlchen/tests/db/test_db.py:1349:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590677829]`
+ rotkehlchen/tests/db/test_db.py:1355:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590677829]`
+ rotkehlchen/tests/db/test_db.py:1361:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590777829]`
+ rotkehlchen/tests/db/test_db.py:1367:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590877829]`
+ rotkehlchen/tests/db/test_db.py:1373:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590877829]`
+ rotkehlchen/tests/db/test_db.py:1379:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590877829]`
+ rotkehlchen/tests/db/test_db.py:1393:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590877829]`
+ rotkehlchen/tests/db/test_db.py:1406:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1411:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676729]`
+ rotkehlchen/tests/db/test_db.py:1416:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590677829]`
+ rotkehlchen/tests/db/test_db.py:1420:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590777829]`
+ rotkehlchen/tests/db/test_db.py:1425:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590877829]`
+ rotkehlchen/tests/db/test_db.py:1454:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1460:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1473:59: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[0]`
+ rotkehlchen/tests/db/test_db.py:1473:70: error[invalid-argument-type] Argument to bound method `query_timed_balances` is incorrect: Expected `Timestamp | None`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1478:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1482:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1590676728]`
+ rotkehlchen/tests/db/test_db.py:1566:59: error[invalid-argument-type] Argument is incorrect: Expected `ApiKey`, found `str`
+ rotkehlchen/tests/db/test_db.py:1591:13: error[invalid-argument-type] Argument to bound method `add_queried_address_for_module` is incorrect: Expected `ChecksumAddress`, found `Literal["0xd36029d76af6fE4A356528e4Dc66B2C18123597D"]`
- rotkehlchen/tests/db/test_db.py:1594:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["0xd36029d76af6fE4A356528e4Dc66B2C18123597D"]` with `tuple[Unknown, ...] | None`
+ rotkehlchen/tests/db/test_db.py:1594:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["0xd36029d76af6fE4A356528e4Dc66B2C18123597D"]` with `tuple[ChecksumAddress, ...] | None`
+ rotkehlchen/tests/db/test_ens.py:77:17: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Unknown | int`
+ rotkehlchen/tests/db/test_ens.py:95:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Unknown | int`
+ rotkehlchen/tests/db/test_ens.py:100:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Unknown | int`
+ rotkehlchen/tests/db/test_ens.py:176:13: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Unknown | int`
+ rotkehlchen/tests/db/test_evmtx.py:116:87: error[invalid-argument-type] Argument to bound method `make` is incorrect: Expected `EVMTxHash | None`, found `Literal[b"dsadsad"]`
+ rotkehlchen/tests/db/test_history_events.py:43:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `EVMTxHash`
+ rotkehlchen/tests/db/test_history_events.py:58:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `EVMTxHash`
+ rotkehlchen/tests/db/test_history_events.py:67:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `EVMTxHash`
+ rotkehlchen/tests/db/test_history_events.py:81:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `EVMTxHash`
+ rotkehlchen/tests/db/test_ranges.py:13:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/db/test_ranges.py:13:75: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[2]`
+ rotkehlchen/tests/db/test_ranges.py:15:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[8]`
+ rotkehlchen/tests/db/test_ranges.py:15:75: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[17]`
+ rotkehlchen/tests/db/test_ranges.py:17:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[19]`
+ rotkehlchen/tests/db/test_ranges.py:17:76: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[23]`
+ rotkehlchen/tests/db/test_ranges.py:19:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[22]`
+ rotkehlchen/tests/db/test_ranges.py:19:76: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[57]`
+ rotkehlchen/tests/db/test_ranges.py:21:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[26]`
+ rotkehlchen/tests/db/test_ranges.py:21:76: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[125]`
+ rotkehlchen/tests/db/test_ranges.py:24:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[3]`
+ rotkehlchen/tests/db/test_ranges.py:24:75: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[9]`
+ rotkehlchen/tests/db/test_ranges.py:26:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[9]`
+ rotkehlchen/tests/db/test_ranges.py:26:75: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[17]`
+ rotkehlchen/tests/db/test_ranges.py:28:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[19]`
+ rotkehlchen/tests/db/test_ranges.py:28:76: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[23]`
+ rotkehlchen/tests/db/test_ranges.py:30:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[120]`
+ rotkehlchen/tests/db/test_ranges.py:30:77: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[250]`
+ rotkehlchen/tests/db/test_ranges.py:32:72: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[126]`
+ rotkehlchen/tests/db/test_ranges.py:32:77: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[170]`
+ rotkehlchen/tests/db/test_ranges.py:47:77: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[12]`
+ rotkehlchen/tests/db/test_ranges.py:47:87: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[90]`
+ rotkehlchen/tests/db/test_ranges.py:57:77: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[250]`
+ rotkehlchen/tests/db/test_ranges.py:57:87: error[invalid-argument-type] Argument to bound method `get_location_query_ranges` is incorrect: Expected `Timestamp`, found `Literal[500]`
+ rotkehlchen/tests/exchanges/test_binance.py:57:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_binance.py:57:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_binance_us.py:27:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_binance_us.py:27:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_bitcoinde.py:46:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_bitcoinde.py:46:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_bitfinex.py:131:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_bitfinex.py:131:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_bitmex.py:33:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_bitmex.py:33:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_bitstamp.py:37:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_bitstamp.py:37:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_cryptocom.py:30:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_cryptocom.py:30:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_cryptocom.py:37:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_cryptocom.py:37:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_iconomi.py:31:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_iconomi.py:31:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_independentreserve.py:24:58: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_independentreserve.py:24:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_independentreserve.py:31:58: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_independentreserve.py:31:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_kraken.py:85:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_kraken.py:85:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"YQ=="]`
+ rotkehlchen/tests/exchanges/test_kraken.py:815:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_kraken.py:815:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"YW55IGNhcm5hbCBwbGVhc3VyZS4="]`
+ rotkehlchen/tests/exchanges/test_kraken.py:1079:9: error[invalid-argument-type] Argument to function `query_api_create_and_get_report` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/exchanges/test_kraken.py:1080:9: error[invalid-argument-type] Argument to function `query_api_create_and_get_report` is incorrect: Expected `Timestamp`, found `Literal[1640493377]`
+ rotkehlchen/tests/exchanges/test_kraken.py:1094:9: error[invalid-argument-type] Argument to function `query_api_create_and_get_report` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/exchanges/test_kraken.py:1095:9: error[invalid-argument-type] Argument to function `query_api_create_and_get_report` is incorrect: Expected `Timestamp`, found `Literal[1640493377]`
+ rotkehlchen/tests/exchanges/test_kucoin.py:38:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_kucoin.py:39:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_okx.py:23:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_okx.py:23:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_poloniex.py:53:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_poloniex.py:53:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/exchanges/test_woo.py:25:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Literal["a"]`
+ rotkehlchen/tests/exchanges/test_woo.py:25:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Literal[b"a"]`
+ rotkehlchen/tests/external_apis/test_blockscout.py:107:36: error[invalid-argument-type] Argument to bound method `has_activity` is incorrect: Expected `ChecksumAddress`, found `Literal["0x3C69Bc9B9681683890ad82953Fe67d13Cd91D5EE"]`
+ rotkehlchen/tests/external_apis/test_blockscout.py:108:36: error[invalid-argument-type] Argument to bound method `has_activity` is incorrect: Expected `ChecksumAddress`, found `Literal["0x014cd0535b2Ea668150a681524392B7633c8681c"]`
+ rotkehlchen/tests/external_apis/test_blockscout.py:109:36: error[invalid-argument-type] Argument to bound method `has_activity` is incorrect: Expected `ChecksumAddress`, found `Literal["0x6c66149E65c517605e0a2e4F707550ca342f9c1B"]`
+ rotkehlchen/tests/external_apis/test_cryptocompare.py:101:9: error[invalid-argument-type] Argument to bound method `query_and_store_historical_data` is incorrect: Expected `Timestamp`, found `Literal[1287957545]`
+ rotkehlchen/tests/external_apis/test_cryptocompare.py:125:9: error[invalid-argument-type] Argument to bound method `query_and_store_historical_data` is incorrect: Expected `Timestamp`, found `Literal[1289159945]`
+ rotkehlchen/tests/external_apis/test_cryptocompare.py:174:9: error[invalid-argument-type] Argument to bound method `query_and_store_historical_data` is incorrect: Expected `Timestamp`, found `Literal[1287957545]`
+ rotkehlchen/tests/external_apis/test_defillama.py:85:9: error[invalid-argument-type] Argument to bound method `query_historical_price` is incorrect: Expected `Timestamp`, found `Literal[1597024800]`
+ rotkehlchen/tests/external_apis/test_etherscan.py:52:82: error[invalid-argument-type] Argument is incorrect: Expected `ApiKey`, found `str & ~AlwaysFalsy`
+ rotkehlchen/tests/external_apis/test_etherscan.py:123:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1439048640]`
+ rotkehlchen/tests/external_apis/test_etherscan.py:125:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x5153493bB1E1642A63A098A65dD3913daBB6AE24"]`
+ rotkehlchen/tests/external_apis/test_etherscan.py:208:13: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0xC951900c341aBbb3BAfbf7ee2029377071Dbc36A"]`
+ rotkehlchen/tests/external_apis/test_etherscan.py:209:13: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2910543Af39abA0Cd09dBb2D50200b3E800A63D2"]`
+ rotkehlchen/tests/external_apis/test_etherscan.py:225:13: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC951900c341aBbb3BAfbf7ee2029377071Dbc36A"]`
+ rotkehlchen/tests/external_apis/test_gnosispay.py:73:44: error[invalid-argument-type] Argument to bound method `get_data_for_transaction` is incorrect: Expected `EVMTxHash`, found `Literal[""]`
+ rotkehlchen/tests/external_apis/test_gnosispay.py:73:56: error[invalid-argument-type] Argument to bound method `get_data_for_transaction` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/external_apis/test_google_calendar.py:325:13: error[invalid-argument-type] Argument is incorrect: Expected `HexColorCode | None`, found `Literal["FF0000"]`
+ rotkehlchen/tests/external_apis/test_google_calendar.py:338:13: error[invalid-argument-type] Argument is incorrect: Expected `HexColorCode | None`, found `Literal["00FF00"]`
+ rotkehlchen/tests/external_apis/test_google_calendar.py:376:13: error[invalid-argument-type] Argument is incorrect: Expected `HexColorCode | None`, found `Literal["0000FF"]`
+ rotkehlchen/tests/external_apis/test_xratescom.py:29:9: error[invalid-argument-type] Argument to function `get_historical_xratescom_exchange_rates` is incorrect: Expected `Timestamp`, found `Literal[1459585352]`
+ rotkehlchen/tests/external_apis/test_xratescom.py:39:9: error[invalid-argument-type] Argument to function `get_historical_xratescom_exchange_rates` is incorrect: Expected `Timestamp`, found `Literal[1459585352]`
+ rotkehlchen/tests/external_apis/test_xratescom.py:52:13: error[invalid-argument-type] Argument to function `get_historical_xratescom_exchange_rates` is incorrect: Expected `Timestamp`, found `Literal[512814152]`
+ rotkehlchen/tests/fixtures/exchanges/bitmex.py:28:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiKey`, found `Unknown | Literal["XY98JYVL15Zn-iU9f7OsJeVf"]`
+ rotkehlchen/tests/fixtures/exchanges/bitmex.py:29:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ApiSecret`, found `Unknown | Literal[b"671tM6f64bt6KhteDakj2uCCNBt7HhZVEE7H5x16Oy4zb1ag"]`
+ rotkehlchen/tests/fixtures/exchanges/cryptocom.py:15:9: error[invalid-argument-type] Argument to function `create_test_cryptocom` is incorrect: Expected `ApiKey | None`, found `Literal["ddddddd"]`
+ rotkehlchen/tests/fixtures/exchanges/cryptocom.py:16:9: error[invalid-argument-type] Argument to function `create_test_cryptocom` is incorrect: Expected `ApiSecret | None`, found `Literal[b"secret"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:54:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"\xb8\rF;\xbc\xf8J\x87m\xb6\xcf\x80_\x1d\x88k`\xe7\xab\r9!4\xb3t\xe2\xea\xb3\xa1\x93/\xe1"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:58:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:59:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x957A50DA7B25Ce98B7556AfEF1d4e5F5C60fC818"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:65:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"1*\xb01\xb3PL\xde\xc45G\x95\x17l\xcc\xadL\xaf8\xa1P\xd5\xd3\x10\xc9^\x93I\x9bY\xee\xd1"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:69:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x9531C059098e3d194fF87FebB587aB07B30B1306"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:70:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:76:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"\xe8wB<\xc4\xf2F\x13H\x96\xbfZf\xcc\x922\xa8\xbeM\xc7\x1au\xd7\xeap>\x10]\xfd\x8e{\x82"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:80:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x9531C059098e3d194fF87FebB587aB07B30B1306"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:81:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:93:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b'\xe8"\x81\xa8\\"\xc7r\xeb5\x17p5\xd9<\xdb\x7fU\x9b\xafp\xe0,\t\x00\xf5\x08\xe7#=\x1d\x0f']`
+ rotkehlchen/tests/integration/test_zksynclite.py:97:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:98:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x4D9339dd97db55e3B9bCBE65dE39fF9c04d1C2cd"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:110:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"\x83\x00\x1f\x1cU\x80\xd9\r4Wy\xcd\x10v/\xc7\x1cL\x90  %Q\xbcH\x031\xd7\rT|\xc7"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:114:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:121:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"3\x1f\xccI\xdc<\nw.\x0b^E\x185\x0f=\x9a\\Uv\xb4\xe8\xdb\xc7\xc5k,Y\xca\xa29\xbb"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:125:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x9531C059098e3d194fF87FebB587aB07B30B1306"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:126:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:132:9: error[invalid-argument-type] Argument is incorrect: Expected `EVMTxHash`, found `Literal[b"\xbdr;Z_\x87\xe4\x85\xa4x\xbc}\x1f6]\xb7\x94@\xb6\xe90[\xff;\x16\xa0\xe0\xab\x83\xe5\x19p"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:136:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/integration/test_zksynclite.py:137:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2B888954421b424C5D3D9Ce9bB67c9bD47537d12"]`
+ rotkehlchen/tests/unit/accounting/evm/test_transactions.py:32:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Timestamp`
+ rotkehlchen/tests/unit/accounting/test_basic.py:63:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_basic.py:63:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1495751688]`
+ rotkehlchen/tests/unit/accounting/test_exchanges.py:49:77: error[invalid-argument-type] Argument to function `accounting_create_and_process_history` is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/accounting/test_exchanges.py:49:89: error[invalid-argument-type] Argument to function `accounting_create_and_process_history` is incorrect: Expected `Timestamp`, found `Literal[1611426233]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:52:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:52:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1495751688]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:105:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:105:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:183:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_prefork_assets.py:183:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1569693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:56:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:56:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:71:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:71:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:86:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:86:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:123:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:123:65: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1619693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:153:44: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:153:56: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:182:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp | None`, found `Literal[1484438400]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:183:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1484629704]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:192:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp | None`, found `Literal[1487116800]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:193:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1487289600]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:204:9: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1436979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:205:9: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:263:9: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1466979735]`
+ rotkehlchen/tests/unit/accounting/test_settings.py:264:9: error[invalid-argument-type] Argument to function `accounting_history_process` is incorrect: Expected `Timestamp`, found `Literal[1519693374]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:207:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1605789951000]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:219:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1605789951000]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:232:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1605789951000]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:245:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1605789951000]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:272:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:315:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:329:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:358:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:401:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:415:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:444:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:507:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:521:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:536:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:694:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:756:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:770:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aave_v2.py:785:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_aerodrome.py:252:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2223F9FE624F69Da4D8256A7bCc9104FBA7F8f75"]`
+ rotkehlchen/tests/unit/decoders/test_aerodrome.py:266:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2223F9FE624F69Da4D8256A7bCc9104FBA7F8f75"]`
+ rotkehlchen/tests/unit/decoders/test_aerodrome.py:280:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x2223F9FE624F69Da4D8256A7bCc9104FBA7F8f75"]`
+ rotkehlchen/tests/unit/decoders/test_arbitrum_one_bridge.py:271:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x6D2457a4ad276000A615295f7A80F79E48CcD318"]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:168:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1655675488]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:414:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:476:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:490:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:506:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:529:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:531:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x95c5582D781d507A084c9E5f885C77BabACf8EeA"]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:618:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:632:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:656:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:709:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:723:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:747:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:792:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:806:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:830:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:884:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_convex.py:898:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[0]`
+ rotkehlchen/tests/unit/decoders/test_cowswap.py:104:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC92E8bdf79f0507f65a392b0ab4667716BFE0110"]`
+ rotkehlchen/tests/unit/decoders/test_cowswap.py:354:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC92E8bdf79f0507f65a392b0ab4667716BFE0110"]`
+ rotkehlchen/tests/unit/decoders/test_cowswap.py:1000:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC92E8bdf79f0507f65a392b0ab4667716BFE0110"]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:107:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1650276061]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:244:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1650276061]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:246:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x767B35b9F06F6e28e5ed05eE7C27bDf992eba5d2"]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:520:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1650276061]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:522:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0xa8005630caE7b7d2AFADD38FD3B3040d13cbE2BC"]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:571:70: error[invalid-argument-type] Argument to bound method `add_evm_internal_transactions` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xa8005630caE7b7d2AFADD38FD3B3040d13cbE2BC"]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:635:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1650276061]`
+ rotkehlchen/tests/unit/decoders/test_curve.py:637:9: error[invalid-argument-type] Argument is incorrect: Expected `ChecksumAddress`, found `Literal["0x2fac74A3a04B031F240923621a578724C40678af"]`
+ rotkehlchen/tests/unit/decoders/test_eigenlayer.py:243:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xaCB55C530Acdb2849e6d4f36992Cd8c9D50ED8F7"]`
+ rotkehlchen/tests/unit/decoders/test_enriched.py:36:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1646375440]`
+ rotkehlchen/tests/unit/decoders/test_enriched.py:132:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Literal[1646375440]`
+ rotkehlchen/tests/unit/decoders/test_ens.py:257:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x084b1c3C81545d370f3634392De611CaaBFf8148"]`
+ rotkehlchen/tests/unit/decoders/test_ens.py:713:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Unknown | Literal["0x34207C538E39F2600FE672bB84A90efF190ae4C7", "0x4bBa290826C253BD854121346c370a9886d1bC26"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin.py:85:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xC8CA7F1C1a391CAfE43cf7348a2E54930648a0D4"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin.py:121:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x3A5bd1E37b099aE3386D13947b6a90d97675e5e3"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin.py:134:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xde21F729137C5Af1b01d73aF1dC21eFfa2B8a0d6"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin.py:161:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x6e08E6e2D0deeb294fd53e9708f53b0fBedc06d5"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:37:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xf0C2007aD05a8d66e98be932C698c232292eC8eA"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:60:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xc191a29203a83eec8e846c26340f828C68835715"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:154:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x8e1bD5Da87C14dd8e08F7ecc2aBf9D1d558ea174"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:167:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x8e1bD5Da87C14dd8e08F7ecc2aBf9D1d558ea174"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:202:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xe575282b376E3c9886779A841A2510F1Dd8C2CE4"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:237:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x03506eD3f57892C85DB20C36846e9c808aFe9ef4"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:272:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x15fa08599EB017F89c1712d0Fe76138899FdB9db"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:305:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xEb33BB3705135e99F7975cDC931648942cB2A96f"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:328:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xebaF311F318b5426815727101fB82f0Af3525d7b"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:354:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x6017B1d17f4D7547dC4aac88fbD0AA1826e7e6CE"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:380:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x3d1f546F05834423Acc7e4CA1169ae320cee9AF0"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:418:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xa1D52F9b5339792651861329A046dD912761E9A9"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:445:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xcD9a4e7C2ad6AAae7Ac25c2139d71739d9Fa2284"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:471:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x830862F98399520f351273B12FD3C622a226bDfE"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:508:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x8e1bD5Da87C14dd8e08F7ecc2aBf9D1d558ea174"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:520:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x8e1bD5Da87C14dd8e08F7ecc2aBf9D1d558ea174"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:533:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xb9ecee9a0e273d8A1857F3B8EeA30e5dD3cb6335"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:546:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xE6D7b9Fb31B93E542f57c7B6bfa0a5a48EfC9D0f"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:584:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x698386C93513d6D0C58f296633A7A3e529bd4026"]`
+ rotkehlchen/tests/unit/decoders/test_gitcoin_v2.py:597:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0xfcBf17200C64E860F6639aa12B525015d115F863"]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:35:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1591043988000]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:47:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1591043988000]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:59:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1591043988000]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:87:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1644182638000]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:99:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1644182638000]`
+ rotkehlchen/tests/unit/decoders/test_kyber.py:111:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TimestampMS`, found `Literal[1644182638000]`
+ rotkehlchen/tests/unit/decoders/test_liquity_v2.py:45:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x3Dd5BbB839f8AE9B64c73780e89Fdd1181Bf5205"]`
+ rotkehlchen/tests/unit/decoders/test_liquity_v2.py:58:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x3Dd5BbB839f8AE9B64c73780e89Fdd1181Bf5205"]`
+ rotkehlchen/tests/unit/decoders/test_liquity_v2.py:77:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x807DEf5E7d057DF05C796F4bc75C3Fe82Bd6EeE1"]`
+ rotkehlchen/tests/unit/decoders/test_liquity_v2.py:90:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ChecksumAddress | None`, found `Literal["0x...*[Comment body truncated]*

---

_Label `ty` added by @AlexWaygood on 2025-08-28 09:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:6904 on 2025-08-28 10:09_

I think this should be something like

```suggestion
                        write!(f, "<NewType pseudo-class '{}'>, declaration.name(self.db))
```

So that the display of a `NewType` pseudo-class clearly differentiates it from an "instance of that pseudo-class". Otherwise the output of `reveal_type` for something like this is quite confusing:

```py
from typing import NewType, reveal_type

X = NewType("X", int)
reveal_type(X)         # We reveal `X` here
reveal_type(X(42))     # W also reveal `X` here currently, but the object returned by `X(42)`
                       # inhabits a very different type in our model to the object `X` itself!
``` 

---

_@AlexWaygood reviewed on 2025-08-28 10:09_

---

_Comment by @AlexWaygood on 2025-08-28 10:12_

the typing conformance diff all looks good! New diagnostics being emitted on lines that have `# E:` on them

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-28 10:24_

---

_Comment by @github-actions[bot] on 2025-08-28 10:33_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 472 | 0 | 3 |
| `unused-ignore-comment` | 0 | 15 | 0 |
| `invalid-return-type` | 2 | 1 | 8 |
| `unsupported-operator` | 2 | 0 | 4 |
| `type-assertion-failure` | 2 | 0 | 2 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `no-matching-overload` | 1 | 0 | 0 |
| **Total** | **479** | **16** | **19** |

**[Full report with detailed diff](https://jack-newtype3.ecosystem-663.pages.dev/diff)**


---

_Comment by @AlexWaygood on 2025-08-28 11:31_

It's a big ecosystem diff but all the ones I've looked at so far are true positives, which is sort-of shocking -- I guess lots of people out there are just misusing `NewType`?!

---

_@AlexWaygood reviewed on 2025-08-28 11:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:895 on 2025-08-28 11:33_

I think if I were a user I would immediately have the question here "...but _why_ must I follow these requirements?" 😆 I think we should either explain a bit more clearly here why the rules around `NewType` are the way they are, or link to some docs elsewhere that give that explanation

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7290 on 2025-08-28 12:45_

I think it would be better to simply name this struct `NewType`, since it can both appear in the type `Type::KnownInstance(KnownInstanceType::NewType(..))` and in the type `Type::NominalInstance(NominalInstanceType { inner: NominalInstanceInner::NewType(..) })`.

The first of those is a type which means "a set of Python objects with exactly length one, where the sole inhabitant of the set is an object returned by a specific all to `NewType()`". The second of those is a type which means "a set of Python objects with unknown size, where all inhabitants of the set are said to be 'instances' of a specific `NewType` pseudo-class". The first is therefore similar conceptually to a `Type::ClassLiteral` type, and the second is similar conceptually to a "normal" nominal-instance type, but using the word "Instance" in the name of this struct implies that it can only ever be used as the wrapped data in instance types, which isn't true.

I think there's also enough code here that it's worth moving the struct into a `newtypes.rs` submodule -- `types.rs` is already huge, and I think we'd like to make it smaller.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7314 on 2025-08-28 12:48_

could shave off a few lines here by making it recursive?

```suggestion
        match self.base(db) {
            NewTypeBase::ClassType(class) => return class,
            NewTypeBase::NewType(newtype) => newtype.base_class_type(db),
        }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7381 on 2025-08-28 12:50_

similarly here, I'd be tempted to use recursion, unless you think this will be faster? We generally use recursion for this kind of thing elsewhere

```suggestion
        if self == other {
            // Either `self` is `other`, or it's a (transitive) NewType wrapper of it.
            return true;
        }
        match self.base(db) {
            NewTypeBase::NewType(base_newtype) => base_new_type.is_subtype_of(db, other),
            // Classes can't inherit from NewTypes, so if we reach the base `ClassType` without
            // seeing `other`, then we can't be a subtype of it.
            NewTypeBase::ClassType(_) => false,
        }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7418 on 2025-08-28 12:54_

```suggestion
    fn to_instance(self, db: &'db dyn Db) -> Type<'db> {
```

to make it clear that this method creates the type that represents "the set containing all possible instances of this newtypebase", not the type "the singleton set that only contains the runtime object representing this newtypebase itself"

---

_@AlexWaygood reviewed on 2025-08-28 12:56_

Nice! I'm afraid I still suspect that having a `Type::NewTypeInstance()` variant will probably be cleaner and more robust here, but it looks like a lot of the logic here would probably be reusable if we decide to go with that approach

---

_Comment by @oconnor663 on 2025-10-31 02:27_

Closing in favor of https://github.com/astral-sh/ruff/pull/21157.

---

_Closed by @oconnor663 on 2025-10-31 02:27_

---
