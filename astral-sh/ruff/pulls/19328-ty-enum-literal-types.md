```yaml
number: 19328
title: "[ty] Enum literal types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/enum-literals
created_at: 2025-07-14T13:12:50Z
updated_at: 2025-07-15T19:31:55Z
url: https://github.com/astral-sh/ruff/pull/19328
synced_at: 2026-01-12T15:56:36Z
```

# [ty] Enum literal types

---

_@sharkdp_

## Summary

Add a new `Type::EnumLiteral(…)` variant and infer this type for member accesses on enums.

**Example**: No more `@Todo` types here:
```py
from enum import Enum

class Answer(Enum):
    YES = 1
    NO = 2

    def is_yes(self) -> bool:
        return self == Answer.YES

reveal_type(Answer.YES)  # revealed: Literal[Answer.YES]
reveal_type(Answer.YES == Answer.NO)  # revealed: Literal[False]
reveal_type(Answer.YES.is_yes())  # revealed: bool
```

## Test Plan

* Many new Markdown tests for the new type variant
* Added enum literal types to property tests, ran property tests

## Ecosystem analysis

<details>

<summary>Lots of false positives removed. All of the new diagnostics are either new true positives (the majority) or known problems. Click for detailed analysis</summary>

```diff
AutoSplit (https://github.com/Toufool/AutoSplit)
+ error[call-non-callable] src/capture_method/__init__.py:137:9: Method `__getitem__` of type `bound method CaptureMethodDict.__getitem__(key: Never, /) -> type[CaptureMethodBase]` is not callable on object of type `CaptureMethodDict`
+ error[call-non-callable] src/capture_method/__init__.py:147:9: Method `__getitem__` of type `bound method CaptureMethodDict.__getitem__(key: Never, /) -> type[CaptureMethodBase]` is not callable on object of type `CaptureMethodDict`
+ error[call-non-callable] src/capture_method/__init__.py:148:1: Method `__getitem__` of type `bound method CaptureMethodDict.__getitem__(key: Never, /) -> type[CaptureMethodBase]` is not callable on object of type `CaptureMethodDict`
```

New true positives. That `__getitem__` method is apparently annotated with `Never` to prevent developers from using it.


```diff
dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ error[invalid-assignment] ddtrace/vendor/psutil/_common.py:29:5: Object of type `None` is not assignable to `Literal[AddressFamily.AF_INET6]`
+ error[invalid-assignment] ddtrace/vendor/psutil/_common.py:33:5: Object of type `None` is not assignable to `Literal[AddressFamily.AF_UNIX]`
```

Arguably true positives: https://github.com/DataDog/dd-trace-py/blob/e0a772c28bc3897a4b5df73125274999e0f58f30/ddtrace/vendor/psutil/_common.py#L29

```diff
ignite (https://github.com/pytorch/ignite)
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:190:34: Argument to bound method `__call__` is incorrect: Expected `((...) -> Unknown) | None`, found `Literal["123"]`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:220:37: Argument to function `default_event_filter` is incorrect: Expected `Engine`, found `None`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:220:43: Argument to function `default_event_filter` is incorrect: Expected `int`, found `None`
+ error[call-non-callable] tests/ignite/engine/test_custom_events.py:561:9: Object of type `CustomEvents` is not callable
+ error[invalid-argument-type] tests/ignite/metrics/test_frequency.py:50:38: Argument to bound method `attach` is incorrect: Expected `Events`, found `CallableEventWithFilter`
```

All true positives. Some of them are inside `pytest.raises(TypeError, …)` blocks :upside_down_face: 

```diff
meson (https://github.com/mesonbuild/meson)
+ error[invalid-argument-type] unittests/internaltests.py:243:51: Argument to bound method `__init__` is incorrect: Expected `bool`, found `Literal[MachineChoice.HOST]`
+ error[invalid-argument-type] unittests/internaltests.py:271:51: Argument to bound method `__init__` is incorrect: Expected `bool`, found `Literal[MachineChoice.HOST]`
```

New true positives. Enum literals can not be assigned to `bool`, even if their value types are `0` and `1`.

```diff
poetry (https://github.com/python-poetry/poetry)
+ error[invalid-assignment] src/poetry/console/exceptions.py:101:5: Object of type `Literal[""]` is not assignable to `InitVar[str]`
```

New false positive, missing support for `InitVar`.

```diff
prefect (https://github.com/PrefectHQ/prefect)
+ error[invalid-argument-type] src/integrations/prefect-dask/tests/test_task_runners.py:193:17: Argument is incorrect: Expected `StateType`, found `Literal[StateType.COMPLETED]`
```

This is confusing. There are two definitions ([one](https://github.com/PrefectHQ/prefect/blob/74d8cd93eef68600d0c193fa8c4513cf9ac7fdb8/src/prefect/client/schemas/objects.py#L89-L100), [two](https://github.com/PrefectHQ/prefect/blob/main/src/prefect/server/schemas/states.py#L40)) of the `StateType` enum. Here, we're trying to assign one to the other. I don't think that should be allowed, so this is a true positive (?).

```diff
python-htmlgen (https://github.com/srittau/python-htmlgen)
+ error[invalid-assignment] test_htmlgen/form.py:51:9: Object of type `str` is not assignable to attribute `autocomplete` of type `Autocomplete | None`
+ error[invalid-assignment] test_htmlgen/video.py:38:9: Object of type `str` is not assignable to attribute `preload` of type `Preload | None`
```

True positives. [The stubs are wrong](https://github.com/srittau/python-htmlgen/blob/01e3b911ac282d067850bb324b913e3594bc0e23/htmlgen/form.pyi#L8-L10). These should not contain type annotations, but rather just `OFF = ...`.

```diff
rotki (https://github.com/rotki/rotki)
+ error[invalid-argument-type] rotkehlchen/tests/unit/test_serialization.py:62:30: Argument to bound method `deserialize` is incorrect: Expected `str`, found `Literal[15]`
```

New true positive.

```diff
vision (https://github.com/pytorch/vision)
+ error[unresolved-attribute] test/test_extended_models.py:302:17: Type `type[WeightsEnum]` has no attribute `DEFAULT`
+ error[unresolved-attribute] test/test_extended_models.py:302:58: Type `type[WeightsEnum]` has no attribute `DEFAULT`
```

Also new true positives. No `DEFAULT` member exists on `WeightsEnum`.

</details>

---

_Label `ty` added by @sharkdp on 2025-07-14 13:12_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-14 13:12_

---

_Comment by @github-actions[bot] on 2025-07-14 13:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
python-htmlgen (https://github.com/srittau/python-htmlgen)
+ error[invalid-assignment] test_htmlgen/form.py:51:9: Object of type `str` is not assignable to attribute `autocomplete` of type `Autocomplete | None`
+ error[invalid-assignment] test_htmlgen/video.py:38:9: Object of type `str` is not assignable to attribute `preload` of type `Preload | None`
- Found 26 diagnostics
+ Found 28 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- warning[unused-ignore-comment] src/hydra_zen/structured_configs/_implementations.py:492:89: Unused blanket `type: ignore` directive
- Found 572 diagnostics
+ Found 571 diagnostics

trio (https://github.com/python-trio/trio)
- error[unresolved-attribute] src/trio/_abc.py:223:17: Type `def socket(self, family: Unknown | int = @Todo(Attribute access on enum classes), type: Unknown | int = @Todo(Attribute access on enum classes), proto: int = Literal[0]) -> SocketType` has no attribute `AddressFamily`
- error[unresolved-attribute] src/trio/_abc.py:224:15: Type `def socket(self, family: Unknown | int = @Todo(Attribute access on enum classes), type: Unknown | int = @Todo(Attribute access on enum classes), proto: int = Literal[0]) -> SocketType` has no attribute `SocketKind`
+ error[unresolved-attribute] src/trio/_abc.py:223:17: Type `def socket(self, family: Unknown | int = Literal[AddressFamily.AF_INET], type: Unknown | int = Literal[SocketKind.SOCK_STREAM], proto: int = Literal[0]) -> SocketType` has no attribute `AddressFamily`
+ error[unresolved-attribute] src/trio/_abc.py:224:15: Type `def socket(self, family: Unknown | int = Literal[AddressFamily.AF_INET], type: Unknown | int = Literal[SocketKind.SOCK_STREAM], proto: int = Literal[0]) -> SocketType` has no attribute `SocketKind`

jinja (https://github.com/pallets/jinja)
- warning[unused-ignore-comment] src/jinja2/nodes.py:758:45: Unused blanket `type: ignore` directive
- Found 197 diagnostics
+ Found 196 diagnostics

ignite (https://github.com/pytorch/ignite)
- warning[unused-ignore-comment] ignite/contrib/engines/common.py:194:96: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:190:34: Argument to bound method `__call__` is incorrect: Expected `((...) -> Unknown) | None`, found `Literal["123"]`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:220:37: Argument to function `default_event_filter` is incorrect: Expected `Engine`, found `None`
+ error[invalid-argument-type] tests/ignite/engine/test_custom_events.py:220:43: Argument to function `default_event_filter` is incorrect: Expected `int`, found `None`
+ error[call-non-callable] tests/ignite/engine/test_custom_events.py:561:9: Object of type `CustomEvents` is not callable
+ error[invalid-argument-type] tests/ignite/metrics/test_frequency.py:50:38: Argument to bound method `attach` is incorrect: Expected `Events`, found `CallableEventWithFilter`
- Found 2125 diagnostics
+ Found 2129 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ error[invalid-assignment] src/poetry/console/exceptions.py:101:5: Object of type `Literal[""]` is not assignable to `InitVar[str]`
- Found 936 diagnostics
+ Found 937 diagnostics

vision (https://github.com/pytorch/vision)
+ error[unresolved-attribute] test/test_extended_models.py:302:17: Type `type[WeightsEnum]` has no attribute `DEFAULT`
+ error[unresolved-attribute] test/test_extended_models.py:302:58: Type `type[WeightsEnum]` has no attribute `DEFAULT`
- Found 1472 diagnostics
+ Found 1474 diagnostics

yarl (https://github.com/aio-libs/yarl)
- error[invalid-return-type] yarl/_url.py:360:20: Return type does not match returned value: expected `URL`, found `str | SplitResult | URL | UndefinedType | @Todo(Attribute access on enum classes)`
+ error[invalid-return-type] yarl/_url.py:360:20: Return type does not match returned value: expected `URL`, found `str | SplitResult | URL | UndefinedType`

urllib3 (https://github.com/urllib3/urllib3)
- error[invalid-return-type] src/urllib3/_collections.py:386:20: Return type does not match returned value: expected `list[str] | _DT`, found `_Sentinel | _DT | @Todo(Attribute access on enum classes)`
+ error[invalid-return-type] src/urllib3/_collections.py:386:20: Return type does not match returned value: expected `list[str] | _DT`, found `(_Sentinel & ~Literal[_Sentinel.not_passed]) | (_DT & ~Literal[_Sentinel.not_passed])`

AutoSplit (https://github.com/Toufool/AutoSplit)
+ error[call-non-callable] src/capture_method/__init__.py:137:9: Method `__getitem__` of type `bound method CaptureMethodDict.__getitem__(key: Never, /) -> type[CaptureMethodBase]` is not callable on object of type `CaptureMethodDict`
+ error[call-non-callable] src/capture_method/__init__.py:147:9: Method `__getitem__` of type `bound method CaptureMethodDict.__getitem__(key: Never, /) -> type[CaptureMethodBase]` is not callable on object of type `CaptureMethodDict`
+ error[call-non-callable] src/capture_method/__init__.py:148:1: Method `__getitem__` of type `bound method CaptureMethodDict.__getitem__(key: Never, /) -> type[CaptureMethodBase]` is not callable on object of type `CaptureMethodDict`
- Found 38 diagnostics
+ Found 41 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-argument-type] src/_pytest/fixtures.py:157:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Scope`
- error[invalid-argument-type] src/_pytest/fixtures.py:218:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Scope`
+ error[invalid-argument-type] src/_pytest/fixtures.py:157:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Scope & ~Literal[Scope.Function] & ~Literal[Scope.Class] & ~Literal[Scope.Module] & ~Literal[Scope.Package] & ~Literal[Scope.Session]`
+ error[invalid-argument-type] src/_pytest/fixtures.py:218:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Scope & ~Literal[Scope.Function] & ~Literal[Scope.Session] & ~Literal[Scope.Package] & ~Literal[Scope.Module] & ~Literal[Scope.Class]`
- error[invalid-argument-type] src/_pytest/pathlib.py:585:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `ImportMode`
+ error[invalid-argument-type] src/_pytest/pathlib.py:585:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `ImportMode & ~Literal[ImportMode.importlib] & ~Literal[ImportMode.append] & ~Literal[ImportMode.prepend]`
- error[invalid-argument-type] testing/test_cacheprovider.py:1303:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Action`
+ error[invalid-argument-type] testing/test_cacheprovider.py:1303:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Action & ~Literal[Action.MKDIR] & ~Literal[Action.SET]`
- error[invalid-argument-type] testing/test_cacheprovider.py:1315:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Action`
+ error[invalid-argument-type] testing/test_cacheprovider.py:1315:22: Argument to function `assert_never` is incorrect: Expected `Never`, found `Action & ~Literal[Action.MKDIR] & ~Literal[Action.SET]`

django-stubs (https://github.com/typeddjango/django-stubs)
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:15:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:17:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/_enums.py:23:1: Argument does not have asserted type `Literal[True]`
+ error[subclass-of-final-class] tests/assert_type/db/models/test_enums.py:90:19: Class `VoidChoices` cannot inherit from final class `BaseEmptyChoices`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:139:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:140:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:141:1: Argument does not have asserted type `list[int]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:142:1: Argument does not have asserted type `list[tuple[int, @Todo(Support for `typing.TypeAlias`)]]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:147:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:150:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:151:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:152:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:153:1: Argument does not have asserted type `list[tuple[str, @Todo(Support for `typing.TypeAlias`)]]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:158:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:162:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:163:1: Argument does not have asserted type `list[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:170:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:176:1: Argument does not have asserted type `list[str]`
+ error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:171:1: Argument does not have asserted type `@Todo(Support for `typing.TypeAlias`)`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:184:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:189:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:191:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:197:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:202:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:210:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:216:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:224:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:230:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:238:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:243:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:245:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:251:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:255:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:257:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:263:1: Argument does not have asserted type `Literal[True]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:267:1: Argument does not have asserted type `list[str]`
- error[type-assertion-failure] tests/assert_type/db/models/test_enums.py:269:1: Argument does not have asserted type `list[str]`
- Found 464 diagnostics
+ Found 431 diagnostics

meson (https://github.com/mesonbuild/meson)
+ error[invalid-argument-type] unittests/internaltests.py:243:51: Argument to bound method `__init__` is incorrect: Expected `bool`, found `Literal[MachineChoice.HOST]`
+ error[invalid-argument-type] unittests/internaltests.py:271:51: Argument to bound method `__init__` is incorrect: Expected `bool`, found `Literal[MachineChoice.HOST]`
- Found 901 diagnostics
+ Found 903 diagnostics

zulip (https://github.com/zulip/zulip)
+ error[invalid-assignment] zerver/actions/message_send.py:1834:5: Invalid assignment to data descriptor attribute `type` on type `Message` with custom `__set__` method
- Found 7265 diagnostics
+ Found 7266 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ error[invalid-assignment] ddtrace/vendor/psutil/_common.py:29:5: Object of type `None` is not assignable to `Literal[AddressFamily.AF_INET6]`
+ error[invalid-assignment] ddtrace/vendor/psutil/_common.py:33:5: Object of type `None` is not assignable to `Literal[AddressFamily.AF_UNIX]`
- Found 6822 diagnostics
+ Found 6824 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ error[invalid-argument-type] src/integrations/prefect-dask/tests/test_task_runners.py:193:17: Argument is incorrect: Expected `StateType`, found `Literal[StateType.COMPLETED]`
- Found 3728 diagnostics
+ Found 3729 diagnostics

rotki (https://github.com/rotki/rotki)
+ error[invalid-assignment] rotkehlchen/assets/asset.py:77:5: Object of type `Literal[False]` is not assignable to `InitVar[bool]`
- warning[unused-ignore-comment] rotkehlchen/chain/aggregator.py:582:83: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/exchanges/kraken.py:953:57: Unused blanket `type: ignore` directive
- error[invalid-argument-type] rotkehlchen/tests/api/test_exchanges.py:524:52: Argument to function `patch_poloniex_balances_query` is incorrect: Expected `Poloniex`, found `Binance`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:673:12: Type `Binance` has no attribute `account_type`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:713:12: Type `Binance` has no attribute `account_type`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:788:12: Type `Binance` has no attribute `api_passphrase`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:801:12: Type `Binance` has no attribute `api_passphrase`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:823:12: Type `Binance` has no attribute `account_type`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:824:12: Type `Binance` has no attribute `call_limit`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:825:12: Type `Binance` has no attribute `reduction_every_secs`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:834:12: Type `Binance` has no attribute `account_type`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:835:12: Type `Binance` has no attribute `call_limit`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:836:12: Type `Binance` has no attribute `reduction_every_secs`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:846:12: Type `Binance` has no attribute `account_type`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:847:12: Type `Binance` has no attribute `call_limit`
- error[unresolved-attribute] rotkehlchen/tests/api/test_exchanges.py:848:12: Type `Binance` has no attribute `reduction_every_secs`
- warning[unused-ignore-comment] rotkehlchen/tests/api/test_exchanges.py:869:78: Unused blanket `type: ignore` directive
- warning[possibly-unbound-attribute] rotkehlchen/tests/api/test_exchanges.py:872:21: Attribute `name` on type `Binance | None` is possibly unbound
- warning[possibly-unbound-attribute] rotkehlchen/tests/api/test_exchanges.py:874:31: Attribute `name` on type `Binance | None` is possibly unbound
- warning[possibly-unbound-attribute] rotkehlchen/tests/api/test_exchanges.py:886:20: Attribute `api_key` on type `Binance | None` is possibly unbound
- warning[possibly-unbound-attribute] rotkehlchen/tests/api/test_exchanges.py:888:24: Attribute `secret` on type `Binance | None` is possibly unbound
- warning[possibly-unbound-attribute] rotkehlchen/tests/api/test_exchanges.py:893:54: Attribute `session` on type `Binance | None` is possibly unbound
- warning[unused-ignore-comment] rotkehlchen/tests/api/test_exchanges.py:897:78: Unused blanket `type: ignore` directive
- warning[possibly-unbound-attribute] rotkehlchen/tests/api/test_exchanges.py:900:21: Attribute `name` on type `Binance | None` is possibly unbound
- warning[possibly-unbound-attribute] rotkehlchen/tests/api/test_exchanges.py:902:31: Attribute `name` on type `Binance | None` is possibly unbound
- warning[possibly-unbound-attribute] rotkehlchen/tests/api/test_exchanges.py:914:20: Attribute `api_key` on type `Binance | None` is possibly unbound
- warning[possibly-unbound-attribute] rotkehlchen/tests/api/test_exchanges.py:916:24: Attribute `secret` on type `Binance | None` is possibly unbound
- warning[possibly-unbound-attribute] rotkehlchen/tests/api/test_exchanges.py:921:54: Attribute `session` on type `Binance | None` is possibly unbound
- error[invalid-assignment] rotkehlchen/tests/exchanges/test_kraken.py:933:5: Object of type `Literal[False]` is not assignable to attribute `random_ledgers_data` on type `Binance | None`
+ error[invalid-assignment] rotkehlchen/tests/exchanges/test_kraken.py:933:5: Object of type `Literal[False]` is not assignable to attribute `random_ledgers_data` on type `Kraken | None`
- warning[possibly-unbound-attribute] rotkehlchen/tests/exchanges/test_kraken.py:935:9: Attribute `query_history_events` on type `Binance | None` is possibly unbound
+ warning[possibly-unbound-attribute] rotkehlchen/tests/exchanges/test_kraken.py:935:9: Attribute `query_history_events` on type `Kraken | None` is possibly unbound
+ error[invalid-argument-type] rotkehlchen/tests/unit/test_serialization.py:62:30: Argument to bound method `deserialize` is incorrect: Expected `str`, found `Literal[15]`
- error[invalid-argument-type] rotkehlchen/tests/utils/rotkehlchen.py:181:52: Argument to function `patch_poloniex_balances_query` is incorrect: Expected `Poloniex`, found `Binance & ~AlwaysFalsy`
- Found 1606 diagnostics
+ Found 1579 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~20MB
+     memo metadata = ~21MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-07-14 13:21_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 1 | 35 | 0 |
| `unresolved-attribute` | 2 | 13 | 2 |
| `invalid-argument-type` | 8 | 2 | 5 |
| `possibly-unbound-attribute` | 0 | 10 | 1 |
| `invalid-assignment` | 7 | 0 | 1 |
| `unused-ignore-comment` | 0 | 7 | 0 |
| `call-non-callable` | 4 | 0 | 0 |
| `invalid-return-type` | 0 | 0 | 2 |
| `subclass-of-final-class` | 1 | 0 | 0 |
| **Total** | **23** | **67** | **11** |

**[Full report with detailed diff](https://david-enum-literals.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-14 14:10_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-14 14:10_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-14 14:53_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-14 14:53_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-14 18:16_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-14 18:16_

---

_@sharkdp reviewed on 2025-07-15 07:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/call/overloads.md`:405 on 2025-07-15 07:34_

I have not implemented expansion of enums yet. I didn't see any ecosystem impact (no new `no-matching-overload` diagnostics or similar), so I guess this change is fine. Returning `Unknown` instead of `A` seems better anyway.

---

_Comment by @github-actions[bot] on 2025-07-15 07:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-15 08:46_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-15 08:46_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-15 09:12_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-15 09:12_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-15 09:40_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-15 09:40_

---

_Marked ready for review by @sharkdp on 2025-07-15 09:57_

---

_Review requested from @carljm by @sharkdp on 2025-07-15 09:57_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-15 09:57_

---

_Review requested from @dcreager by @sharkdp on 2025-07-15 09:57_

---

_Review requested from @MichaReiser by @sharkdp on 2025-07-15 09:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:442 on 2025-07-15 10:04_

you can even do this if you so desire:

```pycon
>>> Answer.YES.NO
<Answer.NO: 2>
>>> Answer.YES.NO.YES.NO
<Answer.NO: 2>
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:454 on 2025-07-15 10:05_

do we internally simplify `type[Answer]` to a class-literal type, the same as we do for `type[C]` where `C` is explicitly decorated with `@final`? Is it worth adding a test for that?

Do we have any tests for enum classes that are explicitly decorated with `@final`? Even though it's unnecessary, it should probably be allowed

---

_@sharkdp reviewed on 2025-07-15 10:05_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/enums.rs`:49 on 2025-07-15 10:05_

Does this need `heap_size=get_size2::GetSize::get_heap_size`? Do we have some instructions on how to apply these new size-tracking attributes/traits? Similar question for the new `EnumLiteralType` in `types.rs`.

cc @ibraheemdev 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/eq.md`:42 on 2025-07-15 10:07_

Note that the proposed narrowing behaviour here would be unsafe if the enum class (or any of its superclasses) defined a custom `__eq__` and/or `__ne__` method

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/truthiness.md`:1 on 2025-07-15 10:10_

Can you also add a test for an enum class that defines (or inherits from a class that defines) a custom `__bool__` method? We can only consider enum members always-truthy if `__bool__` is either undefined or it is defined and it returns `Literal[True]`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/is_single_valued.md`:1 on 2025-07-15 10:16_

Enum literals are very weird because they're always singletons (you can always narrow them using `is`) but I think we can only safely treat them as single-valued providing `__eq__` and `__ne__` are not defined:

```pycon
>>> class Unfortunate(Enum):
...     X = 2
...     Y = 3
...     def __eq__(self, other):
...         return isinstance(other, Unfortunate)
...         
>>> Unfortunate.X is Unfortunate.Y
False
>>> Unfortunate.X == Unfortunate.Y
True
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3252 on 2025-07-15 10:20_

```suggestion
                                    Type::instance(db, ClassType::NonGeneric(enum_class)),
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5652 on 2025-07-15 10:23_

Ideally I guess we'd jump to the definition of the actual enum member here, right? But I know that this would be much harder. Worth a TODO?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8313 on 2025-07-15 10:25_

Do we need to store a `Type` here or could we store a `ClassLiteral` instead (which we could lazily turn into a `Type` using `Type::instance()` on demand)? (It feels like it would be more strongly typed, and might use a tiny bit less memory? It would also mean that `EnumLiteralType` would unambiguously be a non-atomic type that the type-visitor in `visitor.rs` would not have to recurse into.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/enums.rs`:20 on 2025-07-15 10:29_

nit: we could enforce immutability here and maybe save a tiny bit of memory

```suggestion
    pub(crate) members: Box<[Name]>,
    pub(crate) aliases: FxHashMap<Name, Name>,
}

impl EnumMetadata {
    fn empty() -> Self {
        EnumMetadata {
            members: Box::default(),
```

---

_@AlexWaygood reviewed on 2025-07-15 10:32_

Very nice!!

---

_@AlexWaygood reviewed on 2025-07-15 10:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8313 on 2025-07-15 10:38_

Hmm, but even if we made this change, maybe the type-visitor should materialize the instance fallback anyway and recurse into it, similar to PEP-695 `TypeAliasType`s.

---

_@ibraheemdev reviewed on 2025-07-15 11:27_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/enums.rs`:49 on 2025-07-15 11:27_

You should add `heap_size=get_size2::GetSize::get_heap_size` to any tracked function to ensure accurate memory reports, otherwise salsa assumes the type does not contain any heap allocations.

The empty `GetSize` implementation for `EnumLiteralType` is correct, we don't have a way to report the heap size of the fields yet.

---

_@sharkdp reviewed on 2025-07-15 11:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:8313 on 2025-07-15 11:34_

Excellent suggestions that also solves a few other problems here — thank you!

---

_@sharkdp reviewed on 2025-07-15 12:28_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/enums.md`:454 on 2025-07-15 12:28_

I fixed this by properly modeling the implicit final property of enum classes with members. This also resolved a few other TODOs (intersections collapse to Never, emit a diagnostic if you attempt to subclass an enum).

---

_@sharkdp reviewed on 2025-07-15 13:15_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/is_single_valued.md`:1 on 2025-07-15 13:15_

> Enum literals are very weird because they're always singletons (you can always narrow them using `is`) but I think we can only safely treat them as single-valued providing `__eq__` and `__ne__` are not defined:

Hm, I don't think your example shows this? `Unfortunate.Y` is not of type `Literal[Unfortunate.X]`.

The only way in which a singleton type could not be a single-valued type is if the single inhabitant does not compare equal to itself, I think?

```py
from enum import Enum

class Weird(Enum):
    A = 1
    B = 2

    def __eq__(self, other):
        return False
```

Anyway, I'll make single-valuedness dependent on the custom `__eq__`/`__ne__`

---

_@sharkdp reviewed on 2025-07-15 13:46_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/truthiness.md`:1 on 2025-07-15 13:46_

Done.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-15 14:27_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-15 14:28_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-15 14:29_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-15 14:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:535 on 2025-07-15 14:31_

these might be useful additions to this test:

```suggestion
reveal_type(type(Answer.YES))  # revealed: <class 'Answer'>

class NoMembers(Enum): ...

def _(x: Answer, y: NoMembers):
    reveal_type(type(x))  # revealed: <class 'Answer'>
    reveal_type(type(y))  # revealed: type[NoMembers]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/eq.md`:54 on 2025-07-15 14:34_

```suggestion
    def __ne__(self, other) -> bool:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/conditionals/eq.md`:62 on 2025-07-15 14:35_

is it worth also adding a test for an enum that _inherits_ from a class with a custom `__eq__` method, e.g.

```py
from enum import Enum

class Mixin:
    def __eq__(self, other) -> bool:
        return True

class AlsoAmbiguous(Mixin, Enum):
    NO = 0
    YES = 1

def _(answer: AlsoAmbiguous):
    if answer == AlsoAmbiguous.NO:
        reveal_type(answer)  # revealed: AlsoAmbiguous
    else:
        reveal_type(answer)  # revealed: AlsoAmbiguous

---

_Comment by @codspeed-hq[bot] on 2025-07-15 14:40_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fenum-literals?runnerMode=Instrumentation)

### Merging #19328 will **degrade performances by 5.69%**

<sub>Comparing <code>david/enum-literals</code> (d0060c2) with <code>main</code> (a0d4e1f)</sub>



### Summary

`⚡ 1` improvements  
`❌ 2 (👁 2)` regressions  
`✅ 37` untouched benchmarks  
`🆕 1` new benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` ty_micro[complex_constrained_attributes_1] `` | 58 ms | 61.5 ms | -5.69% |
| 👁 | `` ty_micro[complex_constrained_attributes_2] `` | 57.7 ms | 60.8 ms | -5.12% |
| 🆕 | `` ty_micro[many_enum_members] `` | N/A | 112.4 ms | N/A |
| ⚡ | `` ty_micro[many_string_assignments] `` | 78 ms | 74.8 ms | +4.28% |


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3501 on 2025-07-15 14:42_

hmmm... it looks like overriding `__len__` on an enum class also affects the truthiness of the enum members:

```pycon
>>> class Foo(Enum):
...     def __len__(self):
...         return 0
...     A = 42
...     
>>> bool(Foo.A)
False
```

I'm also not sure we should be doing anything for enum instances specifically that we wouldn't do for other instances of `@final` classes. E.g. it seems like it would be sound to infer instances of `Bar` here as being always truthy, since we can clearly see that no classes in its MRO define `__bool__` or `__len__`:

```py
from typing import final

@final
class Bar: ...
```

I'd be okay with inferring an ambiguous truthiness for enum members for now and deferring this question, since it seems like there's a few things to think through here.

---

_@AlexWaygood approved on 2025-07-15 14:45_

Great work!

---

_Comment by @AlexWaygood on 2025-07-15 14:50_

hmm, could the codspeed complaints be due to the changes to `try_bool`? IIRC that's a very hot funciton

---

_@sharkdp reviewed on 2025-07-15 17:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:3501 on 2025-07-15 17:24_

Makes sense, thanks. Changed to infer `Ambiguous` in all cases for now.

---

_@sharkdp reviewed on 2025-07-15 17:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:3501 on 2025-07-15 17:24_

… but I kept a bunch of test cases with TODOs. Also added one for custom `__len__`.

---

_Comment by @sharkdp on 2025-07-15 17:39_

> hmm, could the codspeed complaints be due to the changes to `try_bool`? IIRC that's a very hot funciton

No, I'm afraid it might be due to the new `is_final` changes? Looking into it.

---

_Comment by @sharkdp on 2025-07-15 19:29_

> |     | Benchmark | `BASE` | `HEAD` | Change |
> | --- | --------- | ----------------------- | ------------------- | ------ |
> | ❌ | `` ty_micro[complex_constrained_attributes_1] `` | 58 ms | 61.5 ms | -5.69% |
> | ❌ | `` ty_micro[complex_constrained_attributes_2] `` | 57.7 ms | 60.8 ms | -5.12% |
> | 🆕 | `` ty_micro[many_enum_members] `` | N/A | 112.4 ms | N/A |
> | ⚡ | `` ty_micro[many_string_assignments] `` | 78 ms | 74.8 ms | +4.28% |

I did find a way to reduce the regression from -8% to -5% in the `complex_constrained_attributes_*` benchmarks. In the process, `many_string_assignments` is now suddenly a `+4.28%` improvement — I do not understand why. All of these benchmarks are extremely short (they are regression tests for cases where we initially took a long time). And we do need to perform some more work on this branch, because whenever we check if a type is final, we now also check if it's an enum — which requires a MRO traversal.

I don't think that small regressions on these benchmarks are a huge deal, and all other benchmarks are below -2%, so I'm not going to investigate much further. Not because I'm out of ideas, but because it starts to feel like there are more important things to do. If someone disagrees, please let me know. Happy to take another look.

---

_Merged by @sharkdp on 2025-07-15 19:31_

---

_Closed by @sharkdp on 2025-07-15 19:31_

---

_Branch deleted on 2025-07-15 19:31_

---
