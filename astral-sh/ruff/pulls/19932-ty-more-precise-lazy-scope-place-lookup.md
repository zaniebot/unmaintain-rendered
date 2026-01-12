```yaml
number: 19932
title: "[ty] more precise lazy scope place lookup"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: lazy-scope-place-lookup2
created_at: 2025-08-15T18:41:24Z
updated_at: 2025-09-09T01:23:40Z
url: https://github.com/astral-sh/ruff/pull/19932
synced_at: 2026-01-12T15:56:50Z
```

# [ty] more precise lazy scope place lookup

---

_@mtshiba_

## Summary

This is a follow-up to https://github.com/astral-sh/ruff/pull/19321.

~~This PR introduces the concept of "completeness" to `EnclosingSnapshot`, making lookup of nonlocal symbols more precise.~~

~~Here's the new mechanism we'll add: bindings that appear before the lazy nested scope can be analyzed exactly for reachability and narrowing. We record them in enclosing snapshots, and if bindings appear after the lazy nested scope, instead of sweeping the snapshot, we mark it as "incomplete." Then, at load time, we analyze bindings before the nested scope exactly, and always treat bindings after the nested scope as bound.~~

Now lazy snapshots are updated to take into account new bindings on every symbol reassignment.

```python
def outer(x: A | None):
    if x is None:
        x = A()

    reveal_type(x)  # revealed: A

    def inner() -> None:
        # lazy snapshot: {x: A}
        reveal_type(x)  # revealed: A
    inner()

def outer() -> None:
    x = None

    x = 1

    def inner() -> None:
        # lazy snapshot: {x: Literal[1]} -> {x: Literal[1, 2]}
        reveal_type(x)  # revealed: Literal[1, 2]
    inner()

    x = 2
```

Closes astral-sh/ty#559.

## Test Plan

Some TODOs in `public_types.md` now work properly.



---

_Review requested from @carljm by @mtshiba on 2025-08-15 18:41_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-08-15 18:41_

---

_Review requested from @sharkdp by @mtshiba on 2025-08-15 18:41_

---

_Review requested from @dcreager by @mtshiba on 2025-08-15 18:41_

---

_Comment by @github-actions[bot] on 2025-08-15 18:43_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-15 18:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
jinja (https://github.com/pallets/jinja)
- src/jinja2/runtime.py:941:9: warning[possibly-unbound-attribute] Attribute `warning` on type `Logger | None` is possibly unbound
+ src/jinja2/runtime.py:952:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.py:1306:36: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 30 diagnostics
+ Found 29 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/sdp.py:385:63: warning[possibly-unbound-attribute] Attribute `rtp` on type `MediaDescription | None` is possibly unbound
+ src/aiortc/sdp.py:385:63: warning[possibly-unbound-attribute] Attribute `rtp` on type `None | MediaDescription` is possibly unbound

dulwich (https://github.com/dulwich/dulwich)
- dulwich/client.py:1757:13: warning[possibly-unbound-attribute] Attribute `close` on type `None | socket` is possibly unbound
- dulwich/client.py:1787:48: warning[possibly-unbound-attribute] Attribute `fileno` on type `None | socket` is possibly unbound
- dulwich/porcelain.py:3036:9: warning[possibly-unbound-attribute] Attribute `write` on type `BinaryIO | None` is possibly unbound
- dulwich/porcelain.py:3037:9: warning[possibly-unbound-attribute] Attribute `flush` on type `BinaryIO | None` is possibly unbound
- dulwich/porcelain.py:3070:9: warning[possibly-unbound-attribute] Attribute `write` on type `BinaryIO | None` is possibly unbound
- dulwich/porcelain.py:3071:9: warning[possibly-unbound-attribute] Attribute `flush` on type `BinaryIO | None` is possibly unbound
- Found 194 diagnostics
+ Found 188 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_subprocess.py:764:31: error[call-non-callable] Object of type `None` is not callable
- Found 676 diagnostics
+ Found 675 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/structured_configs/_implementations.py:2523:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str | DataClass_ | type[@Todo(type[T] for protocols)] | Mapping[str, None | Sequence[str]]] | None`
- src/hydra_zen/structured_configs/_implementations.py:3303:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str | DataClass_ | type[@Todo(type[T] for protocols)] | Mapping[str, None | Sequence[str]]] | None`
- Found 561 diagnostics
+ Found 559 diagnostics

alerta (https://github.com/alerta/alerta)
- alerta/database/backends/mongodb/base.py:805:28: warning[possibly-unbound-attribute] Attribute `where` on type `Unknown | None | (Unknown & ~AlwaysFalsy)` is possibly unbound
- alerta/database/backends/mongodb/base.py:843:28: warning[possibly-unbound-attribute] Attribute `where` on type `Unknown | None | (Unknown & ~AlwaysFalsy)` is possibly unbound
- Found 489 diagnostics
+ Found 487 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/abc.py:2026:39: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | None` is possibly unbound
- discord/abc.py:2026:65: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | None` is possibly unbound
- discord/abc.py:2028:54: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | None` is possibly unbound
- discord/abc.py:2030:39: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | None` is possibly unbound
- discord/abc.py:2034:54: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | None` is possibly unbound
- discord/abc.py:2038:54: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | None` is possibly unbound
- discord/client.py:2235:50: warning[possibly-unbound-attribute] Attribute `id` on type `Unknown | None` is possibly unbound
- discord/player.py:591:71: error[call-non-callable] Object of type `None` is not callable
- discord/player.py:601:75: error[call-non-callable] Object of type `None` is not callable
- Found 527 diagnostics
+ Found 518 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/_compat.py:131:31: error[invalid-argument-type] Argument to function `getattr` is incorrect: Expected `str`, found `Unknown | None | (Unknown & ~AlwaysFalsy)`
- Found 340 diagnostics
+ Found 339 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/config/_operations.py:179:20: warning[possibly-unbound-attribute] Attribute `is_empty` on type `FilterSet | None` is possibly unbound
- src/schemathesis/config/_operations.py:181:51: warning[possibly-unbound-attribute] Attribute `match` on type `FilterSet | None` is possibly unbound
- src/schemathesis/config/_operations.py:182:22: warning[possibly-unbound-attribute] Attribute `is_empty` on type `FilterSet | None` is possibly unbound
- src/schemathesis/config/_operations.py:183:28: warning[possibly-unbound-attribute] Attribute `match` on type `FilterSet | None` is possibly unbound
+ src/schemathesis/hooks.py:76:29: warning[redundant-cast] Value is already of type `str`
- src/schemathesis/pytest/lazy.py:182:61: error[invalid-argument-type] Argument to function `get_fixtures` is incorrect: Expected `dict[str, Any]`, found `Unknown | dict[str, Any] | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- Found 271 diagnostics
+ Found 267 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/base.py:77:38: warning[possibly-unbound-attribute] Attribute `items` on type `Unknown | None | dict[@Todo(dict comprehension key type), @Todo(dict comprehension value type)] | (Unknown & ~Top[list[Unknown]] & ~tuple[object, ...] & ~AlwaysFalsy)` is possibly unbound
- Found 198 diagnostics
+ Found 197 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- tests/unittests/sources/test_ec2.py:377:16: warning[possibly-unbound-attribute] Attribute `add` on type `Unknown | None | (Unknown & ~AlwaysFalsy)` is possibly unbound
- tests/unittests/sources/test_gce.py:104:16: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown | None` with `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- tests/unittests/sources/test_gce.py:105:28: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is possibly unbound
- tests/unittests/test_net.py:1464:16: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 630 diagnostics
+ Found 626 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/tools.py:242:19: error[call-non-callable] Object of type `None` is not callable
- Found 185 diagnostics
+ Found 184 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/asyncio/client.py:356:26: error[call-non-callable] Object of type `None` is not callable
- src/websockets/asyncio/server.py:808:26: error[call-non-callable] Object of type `None` is not callable
- src/websockets/asyncio/server.py:982:29: error[call-non-callable] Object of type `None` is not callable
- src/websockets/sync/server.py:754:16: error[call-non-callable] Object of type `None` is not callable
- Found 44 diagnostics
+ Found 40 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/strategy/informative_decorator.py:154:49: error[call-non-callable] Object of type `None` is not callable
- Found 387 diagnostics
+ Found 386 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- lib/Crypto/Protocol/KDF.py:164:32: error[call-non-callable] Object of type `None` is not callable
- Found 1490 diagnostics
+ Found 1489 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ github/Requester.py:1099:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ github/Requester.py:1100:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 313 diagnostics
+ Found 315 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/ext/napoleon/docstring.py:228:52: error[invalid-argument-type] Argument to function `_convert_type_spec_obj` is incorrect: Expected `dict[str, str]`, found `dict[str, str] | None`
- Found 515 diagnostics
+ Found 514 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/core/helpers.py:145:13: error[call-non-callable] Object of type `None` is not callable
- openlibrary/plugins/openlibrary/sentry.py:17:45: warning[possibly-unbound-attribute] Attribute `capture_exception_webpy` on type `Sentry | None` is possibly unbound
+ openlibrary/plugins/upstream/utils.py:1182:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 710 diagnostics
+ Found 709 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/layouts.py:361:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `dict[@Todo(Inference of subscript on special form), Any] | None`
- src/bokeh/layouts.py:362:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- src/bokeh/layouts.py:571:43: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(UIElement, /) -> Unknown`, found `(def traverse(children: list[LayoutDOM], level: int = Literal[0]) -> Unknown) | (def traverse(item: LayoutDOM, top_level: bool = Literal[False]) -> Unknown)`
+ src/bokeh/layouts.py:571:43: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(UIElement, /) -> Unknown`, found `def traverse(item: LayoutDOM, top_level: bool = Literal[False]) -> Unknown`
- Found 848 diagnostics
+ Found 846 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/archive_npy.py:1249:46: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `IndexBase | None`
- static_frame/core/container_util.py:209:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/container_util.py:214:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/frame.py:2022:32: error[not-iterable] Object of type `Cursor | None` may not be iterable
- static_frame/core/frame.py:2023:25: warning[possibly-unbound-attribute] Attribute `append` on type `None | list[@Todo(list literal element type)]` is possibly unbound
- static_frame/core/frame.py:2052:32: error[not-iterable] Object of type `Cursor | None` may not be iterable
- static_frame/core/frame.py:2054:29: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- static_frame/core/frame.py:7514:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/memory_measure.py:203:13: error[invalid-argument-type] Argument to bound method `nested_sizable_elements` is incorrect: Expected `set[int]`, found `None | set[Any]`
- static_frame/core/quilt.py:164:29: error[call-non-callable] Object of type `None` is not callable
- static_frame/core/quilt.py:170:29: error[call-non-callable] Object of type `None` is not callable
- Found 1798 diagnostics
+ Found 1787 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/_internal/compatibility/deprecated.py:196:39: error[call-non-callable] Object of type `None` is not callable
- src/prefect/_internal/compatibility/deprecated.py:253:40: error[call-non-callable] Object of type `None` is not callable
- src/prefect/utilities/collections.py:411:20: error[missing-argument] No argument provided for required parameter 2
- Found 2989 diagnostics
+ Found 2986 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/computation/ops.py:235:34: error[invalid-argument-type] Argument to function `getattr` is incorrect: Expected `str`, found `Unknown | None`
- Found 1679 diagnostics
+ Found 1678 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/datasets/_samples_generator.py:1825:28: warning[possibly-unbound-attribute] Attribute `uniform` on type `Unknown | None` is possibly unbound
- sklearn/utils/_testing.py:516:45: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | list[@Todo(list literal element type)] | (Unknown & ~None)`
- sklearn/utils/_testing.py:570:40: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | list[@Todo(list literal element type)] | (Unknown & ~None)`
- Found 2043 diagnostics
+ Found 2040 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/actions/streams.py:1089:23: error[unresolved-attribute] Type `Stream` has no attribute `id`
- Found 2670 diagnostics
+ Found 2669 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/langgraph/patch.py:177:30: warning[possibly-unbound-attribute] Attribute `__anext__` on type `None | Unknown` is possibly unbound
- tests/ci_visibility/util.py:51:9: error[invalid-assignment] Object of type `set[TestId] | None` is not assignable to attribute `_known_test_ids` on type `CIVisibility | None`
+ tests/ci_visibility/util.py:51:9: error[invalid-assignment] Object of type `set[TestId] | set[Unknown]` is not assignable to attribute `_known_test_ids` on type `CIVisibility | None`
- tests/wait-for-services.py:42:23: warning[possibly-unbound-attribute] Attribute `copy` on type `dict[str, Any] | None` is possibly unbound
- Found 6661 diagnostics
+ Found 6659 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/tests/exchanges/test_binance.py:824:37: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `def get_fiat_deposit_result() -> Unknown`
+ rotkehlchen/tests/exchanges/test_binance.py:824:37: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `(def get_fiat_deposit_result() -> Unknown) | Unknown`
- rotkehlchen/tests/exchanges/test_binance.py:826:37: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `def get_fiat_withdraw_result() -> Unknown`
+ rotkehlchen/tests/exchanges/test_binance.py:826:37: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `(def get_fiat_withdraw_result() -> Unknown) | Unknown`
- rotkehlchen/tests/exchanges/test_kucoin.py:100:64: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `def get_response() -> Unknown`
+ rotkehlchen/tests/exchanges/test_kucoin.py:100:64: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `(def get_response() -> Unknown) | Unknown`
- rotkehlchen/tests/utils/blockchain.py:280:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["transactions"]` with `list[str] | None`
- rotkehlchen/tests/utils/blockchain.py:285:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["logs"]` with `list[str] | None`
- rotkehlchen/tests/utils/blockchain.py:290:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["blocknobytime"]` with `list[str] | None`
- rotkehlchen/tests/utils/blockchain.py:295:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["zerion"]` with `list[str] | None`
- rotkehlchen/tests/utils/blockchain.py:418:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["ethscan"]` with `list[str] | None`
- rotkehlchen/tests/utils/blockchain.py:503:20: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["ethscan"]` with `list[str] | None`
- rotkehlchen/tests/utils/blockchain.py:508:20: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["ethscan"]` with `list[str] | None`
- rotkehlchen/tests/utils/history.py:881:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown | Asset` with `list[Asset] | None`
- rotkehlchen/tests/utils/history.py:884:12: error[unsupported-operator] Operator `in` is not supported for types `tuple[Unknown | Asset, Unknown]` and `None`, in comparing `tuple[Unknown | Asset, Unknown]` with `list[tuple[Asset, Unknown]] | None`
- Found 1598 diagnostics
+ Found 1589 diagnostics

manticore (https://github.com/trailofbits/manticore)
- manticore/native/state.py:319:37: warning[possibly-unbound-attribute] Attribute `address` on type `ConcretizeRegister | MemoryException | Concretize` is possibly unbound
+ manticore/native/state.py:319:37: warning[possibly-unbound-attribute] Attribute `address` on type `MemoryException | Concretize` is possibly unbound
- manticore/native/state.py:319:55: error[unresolved-attribute] Type `ConcretizeRegister | MemoryException | Concretize` has no attribute `size`
+ manticore/native/state.py:319:55: error[unresolved-attribute] Type `MemoryException | Concretize` has no attribute `size`

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/arrays/boolean.py:351:18: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `list[str] | None`
- pandas/core/frame.py:11698:41: error[parameter-already-assigned] Multiple values provided for parameter `method` of bound method `corr`
- pandas/core/frame.py:11698:56: error[invalid-argument-type] Argument to bound method `corr` is incorrect: Expected `int`, found `int | None`
- pandas/core/strings/object_array.py:218:27: warning[possibly-unbound-attribute] Attribute `sub` on type `str | Pattern[Unknown]` is possibly unbound
- pandas/core/strings/object_array.py:367:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Literal[""]` and `Unknown | None | Literal[""]`
- pandas/tests/io/test_parquet.py:215:16: error[unsupported-operator] Operator `in` is not supported for types `str` and `None`, in comparing `Literal["string_with_nan"]` with `Unknown | None`
- pandas/tests/io/test_parquet.py:216:17: warning[possibly-unbound-attribute] Attribute `loc` on type `Unknown | None` is possibly unbound
- Found 3368 diagnostics
+ Found 3361 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/assist_pipeline/pipeline.py:1221:47: warning[possibly-unbound-attribute] Attribute `get` on type `Queue[Unknown] | None | Queue[str | None]` is possibly unbound
- homeassistant/components/assist_pipeline/pipeline.py:2087:9: warning[possibly-unbound-attribute] Attribute `pop` on type `Unknown | dict[str, PipelineConversationData] | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is possibly unbound
- homeassistant/components/conversation/chat_log.py:94:13: warning[possibly-unbound-attribute] Attribute `pop` on type `Unknown | dict[str, ChatLog] | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is possibly unbound
- homeassistant/components/mediaroom/media_player.py:85:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | None | list[@Todo(list literal element type)]`
- homeassistant/components/mediaroom/media_player.py:90:9: warning[possibly-unbound-attribute] Attribute `append` on type `Unknown | None | list[@Todo(list literal element type)]` is possibly unbound
- homeassistant/components/scsgate/__init__.py:47:9: warning[possibly-unbound-attribute] Attribute `stop` on type `None | SCSGate` is possibly unbound
- homeassistant/components/waze_travel_time/coordinator.py:88:20: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Collection[str] | None`
- homeassistant/components/waze_travel_time/coordinator.py:108:36: error[not-iterable] Object of type `Collection[str] | None` may not be iterable
- homeassistant/helpers/llm.py:869:17: error[unsupported-operator] Operator `in` is not supported for types `Any` and `None`, in comparing `Any` with `Unknown | dict[str, dict[str, tuple[str | None, Unknown]]] | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- homeassistant/helpers/llm.py:871:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- homeassistant/helpers/llm.py:873:17: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 13417 diagnostics
+ Found 13406 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/core/exprtools.py:1346:19: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | None | (Unknown & ~AlwaysFalsy) | Literal["mask"]` and `Literal["0"]`
- sympy/core/tests/test_function.py:154:31: error[unresolved-attribute] Type `myfunc | myfunc | myfunc` has no attribute `nargs`
+ sympy/core/tests/test_function.py:154:31: error[unresolved-attribute] Type `myfunc | myfunc` has no attribute `nargs`
- sympy/interactive/session.py:481:55: warning[possibly-unbound-attribute] Attribute `run_cell` on type `None | Unknown` is possibly unbound
- sympy/ntheory/factor_.py:557:20: warning[possibly-unbound-attribute] Attribute `func` on type `Unknown | int` is possibly unbound
+ sympy/ntheory/factor_.py:557:20: warning[possibly-unbound-attribute] Attribute `func` on type `(Unknown & Rational & ~Integer) | int` is possibly unbound
- sympy/ntheory/tests/test_factor_.py:346:31: error[unsupported-operator] Operator `**` is unsupported between objects of type `int | Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]` and `Literal[3]`
+ sympy/ntheory/tests/test_factor_.py:346:31: error[unsupported-operator] Operator `**` is unsupported between objects of type `Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]` and `Literal[3]`
- sympy/ntheory/tests/test_factor_.py:347:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `int | Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
+ sympy/ntheory/tests/test_factor_.py:347:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `Unknown | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- sympy/ntheory/tests/test_residue.py:307:54: error[unsupported-operator] Operator `**` is unsupported between objects of type `set[@Todo(set comprehension element type)] | Symbol` and `set[@Todo(set comprehension element type)] | Symbol`
- sympy/ntheory/tests/test_residue.py:308:54: error[unsupported-operator] Operator `**` is unsupported between objects of type `set[@Todo(set comprehension element type)] | Symbol` and `int | Symbol`
- sympy/ntheory/tests/test_residue.py:309:58: error[unsupported-operator] Operator `**` is unsupported between objects of type `set[@Todo(set comprehension element type)] | Symbol` and `Literal[2]`
- sympy/physics/units/util.py:128:20: warning[possibly-unbound-attribute] Attribute `get_quantity_scale_factor` on type `Unknown | Literal["SI"]` is possibly unbound
- sympy/polys/matrices/tests/test_domainmatrix.py:1058:33: warning[possibly-unbound-attribute] Attribute `lu` on type `DomainMatrix | list[@Todo(list literal element type)]` is possibly unbound
- sympy/simplify/cse_main.py:641:16: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `(Unknown & Basic & ~RootOf & ~MatrixSymbol & ~MatrixElement) | (Unknown & Unevaluated & ~RootOf & ~MatrixSymbol & ~MatrixElement)` with `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- sympy/simplify/cse_main.py:642:24: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- sympy/simplify/cse_main.py:676:12: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `None`, in comparing `(Unknown & Basic) | (Unknown & Unevaluated)` with `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- sympy/simplify/cse_main.py:677:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- sympy/solvers/ode/ode.py:1293:34: error[not-iterable] Object of type `Unknown | None | list[Unknown]` may not be iterable
- sympy/solvers/solveset.py:3312:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None | (Unknown & ~AlwaysFalsy)`
- sympy/strategies/branch/core.py:37:9: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | None | TextIO` is possibly unbound
- sympy/strategies/branch/core.py:38:9: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | None | TextIO` is possibly unbound
- sympy/strategies/core.py:78:13: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | None | TextIO` is possibly unbound
- sympy/strategies/core.py:79:13: warning[possibly-unbound-attribute] Attribute `write` on type `Unknown | None | TextIO` is possibly unbound
- sympy/testing/runtests.py:978:17: error[no-matching-overload] No overload of function `sum` matches arguments
- sympy/testing/runtests.py:984:31: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None | list[@Todo(list comprehension element type)]`
- sympy/testing/runtests.py:989:33: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None | list[@Todo(list comprehension element type)]`
- sympy/utilities/iterables.py:2415:17: error[call-non-callable] Object of type `None` is not callable
- sympy/utilities/lambdify.py:1238:13: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is possibly unbound
- sympy/utilities/lambdify.py:1241:17: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is possibly unbound
- sympy/vector/coordsysrect.py:160:53: error[call-non-callable] Object of type `None` is not callable
- sympy/vector/coordsysrect.py:160:53: error[call-non-callable] Object of type `Str` is not callable
- sympy/vector/coordsysrect.py:160:53: error[call-non-callable] Object of type `Tuple` is not callable
- Found 13411 diagnostics
+ Found 13385 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/_lib/_array_api.py:776:9: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | None | dict[@Todo(dict literal key type), @Todo(dict literal value type)] | (Unknown & ~None)` is possibly unbound
- scipy/_lib/deprecation.py:220:28: warning[possibly-unbound-attribute] Attribute `intersection` on type `Unknown | None | set[Unknown]` is possibly unbound
- scipy/_lib/deprecation.py:237:33: error[unsupported-operator] Operator `-` is unsupported between objects of type `set[@Todo(list literal element type)]` and `Unknown | None | set[Unknown]`
- scipy/_lib/deprecation.py:254:29: error[unsupported-operator] Operator `-` is unsupported between objects of type `set[@Todo(list literal element type)]` and `Unknown | None | set[Unknown]`
- scipy/differentiate/_differentiate.py:1125:32: error[unsupported-operator] Operator `/` is unsupported between objects of type `@Todo(dict literal value type) | None | (@Todo(dict literal value type) & ~None)` and `Literal[100]`
- scipy/optimize/_constraints.py:525:22: error[call-non-callable] Object of type `None` is not callable
- scipy/optimize/_constraints.py:547:26: error[call-non-callable] Object of type `None` is not callable
- scipy/optimize/_lsq/least_squares.py:245:19: error[call-non-callable] Object of type `None` is not callable
- scipy/optimize/_lsq/least_squares.py:245:19: error[missing-argument] No arguments provided for required parameters `rho`, `cost_only` of function `huber`
- scipy/optimize/_lsq/least_squares.py:245:19: error[missing-argument] No arguments provided for required parameters `rho`, `cost_only` of function `soft_l1`
- scipy/optimize/_lsq/least_squares.py:245:19: error[missing-argument] No arguments provided for required parameters `rho`, `cost_only` of function `cauchy`
- scipy/optimize/_lsq/least_squares.py:245:19: error[missing-argument] No arguments provided for required parameters `rho`, `cost_only` of function `arctan`
- scipy/optimize/tests/test_bracket.py:309:13: error[unresolved-attribute] Type `(def f(x) -> Unknown) | (def f(x) -> Unknown) | (def f(x, c) -> Unknown) | (def f(x) -> Unknown)` has no attribute `count`
+ scipy/optimize/tests/test_bracket.py:309:13: error[unresolved-attribute] Type `(def f(x, c) -> Unknown) | (def f(x) -> Unknown)` has no attribute `count`
- scipy/signal/_peak_finding.py:1191:12: error[unsupported-operator] Operator `<` is not supported for types `int` and `None`, in comparing `int` with `Unknown | None`
- scipy/signal/windows/_windows.py:1932:22: warning[possibly-unbound-attribute] Attribute `matmul` on type `Unknown | None` is possibly unbound
- scipy/signal/windows/_windows.py:1932:36: warning[possibly-unbound-attribute] Attribute `cos` on type `Unknown | None` is possibly unbound
- scipy/signal/windows/_windows.py:1933:15: warning[possibly-unbound-attribute] Attribute `pi` on type `Unknown | None` is possibly unbound
- scipy/signal/windows/_windows.py:1933:27: warning[possibly-unbound-attribute] Attribute `newaxis` on type `Unknown | None` is possibly unbound
- scipy/signal/windows/_windows.py:2304:30: warning[possibly-unbound-attribute] Attribute `arange` on type `Unknown | None` is possibly unbound
- scipy/signal/windows/_windows.py:2304:52: warning[possibly-unbound-attribute] Attribute `float64` on type `Unknown | None` is possibly unbound
- scipy/sparse/_construct.py:1335:25: warning[possibly-unbound-attribute] Attribute `uniform` on type `Unknown | None` is possibly unbound
- scipy/sparse/_construct.py:1336:25: warning[possibly-unbound-attribute] Attribute `uniform` on type `Unknown | None` is possibly unbound
- scipy/sparse/tests/test_base.py:129:42: error[unsupported-operator] Unary operator `-` is unsupported for type `Unknown | None | signedinteger[_64Bit]`
- scipy/stats/_axis_nan_policy.py:508:45: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- scipy/stats/_axis_nan_policy.py:549:49: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- scipy/stats/_axis_nan_policy.py:558:47: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- scipy/stats/_axis_nan_policy.py:580:23: error[call-non-callable] Object of type `None` is not callable
- scipy/stats/_axis_nan_policy.py:605:45: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- scipy/stats/_axis_nan_policy.py:612:23: error[call-non-callable] Object of type `None` is not callable
- scipy/stats/_axis_nan_policy.py:627:28: error[call-non-callable] Object of type `None` is not callable
- scipy/stats/_axis_nan_policy.py:631:23: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- scipy/stats/_axis_nan_policy.py:641:28: error[call-non-callable] Object of type `None` is not callable
- scipy/stats/_axis_nan_policy.py:650:28: error[call-non-callable] Object of type `None` is not callable
- scipy/stats/_distn_infrastructure.py:2529:28: error[call-non-callable] Object of type `None` is not callable
- scipy/stats/_distribution_infrastructure.py:414:43: warning[possibly-unbound-attribute] Attribute `integers` on type `Unknown | None | Generator` is possibly unbound
- scipy/stats/tests/test_stats.py:5155:13: warning[possibly-unbound-attribute] Attribute `asarray` on type `Unknown | None | ModuleType | (Unknown & ~None)` is possibly unbound
+ scipy/stats/tests/test_stats.py:5155:13: warning[possibly-unbound-attribute] Attribute `asarray` on type `ModuleType | (Unknown & ~None)` is possibly unbound
- scipy/stats/tests/test_stats.py:5156:14: warning[possibly-unbound-attribute] Attribute `mean` on type `Unknown | None | ModuleType | (Unknown & ~None)` is possibly unbound
+ scipy/stats/tests/test_stats.py:5156:14: warning[possibly-unbound-attribute] Attribute `mean` on type `ModuleType | (Unknown & ~None)` is possibly unbound
- scipy/stats/tests/test_stats.py:5157:15: warning[possibly-unbound-attribute] Attribute `std` on type `Unknown | None | ModuleType | (Unknown & ~None)` is possibly unbound
+ scipy/stats/tests/test_stats.py:5157:15: warning[possibly-unbound-attribute] Attribute `std` on type `ModuleType | (Unknown & ~None)` is possibly unbound
- Found 6437 diagnostics
+ Found 6403 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ty` added by @ntBre on 2025-08-15 18:51_

---

_Comment by @codspeed-hq[bot] on 2025-08-15 18:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Alazy-scope-place-lookup2?runnerMode=Instrumentation)

### Merging #19932 will **not alter performance**

<sub>Comparing <code>mtshiba:lazy-scope-place-lookup2</code> (878ff59) with <code>main</code> (aa5d665)</sub>



### Summary

`✅ 43` untouched benchmarks  





---

_Comment by @codspeed-hq[bot] on 2025-08-15 18:53_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Alazy-scope-place-lookup2?runnerMode=WallTime)

### Merging #19932 will **not alter performance**

<sub>Comparing <code>mtshiba:lazy-scope-place-lookup2</code> (878ff59) with <code>main</code> (aa5d665)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Converted to draft by @mtshiba on 2025-08-15 18:55_

---

_@mtshiba reviewed on 2025-08-25 08:24_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/infer.rs`:6943 on 2025-08-25 08:24_

Not so related to the main point of this PR, but I realized that this branch is actually unnecessary. For the condition here to be met, the enclosing scope needs to be non-function-like, i.e., class scope or module scope, and in that case, this case cannot be reached in the first place.
This is because in module scopes, snapshots are not recorded, and in class scopes, constraints are recorded instead of bindings.

---

_@mtshiba reviewed on 2025-08-25 08:27_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/builder.rs`:508 on 2025-08-25 08:27_

This is just a shortcut for performance optimization.
`infer_name_load` only references the enclosing snapshot if `!self.is_deferred()` is true.

---

_@mtshiba reviewed on 2025-08-25 08:37_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/builder.rs`:785 on 2025-08-25 08:37_

We compare terminal (!= end-of-scope) bindings with snapshotted (captured) bindings to determine the completeness of the snapshot. We need to call `check_lazy_snapshots` even when the scope becomes unreachable.

---

_Marked ready for review by @mtshiba on 2025-08-25 08:41_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-25 09:18_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:508 on 2025-08-25 17:48_

This looked a little odd to me, and I realized we are using `DeferredExpressionState::Deferred` wrongly in this case. Pushed https://github.com/astral-sh/ruff/pull/20086 to fix, which means I think this shortcut should be removed in this PR. I assume this shouldn't be a major performance point, since PEP 695 type aliases aren't common in the ecosystem.

---

_@carljm reviewed on 2025-08-25 23:39_

Thank you!

Took a quick look, made one comment -- I want to take a closer look at some of the changes in use-def map, will try to do that tomorrow.

---

_Review request for @sharkdp removed by @sharkdp on 2025-08-26 08:40_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:768 on 2025-09-03 05:03_

There's a doc comment just above here on the `EnclosingSnapshots` type alias that I think is no longer accurate?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/use_def/place_state.rs`:260 on 2025-09-03 19:22_

Making this distinction on the basis of binding id numbering is not right. It's using source-order incorrectly as a proxy for control flow, and it means we get inconsistent results on a case like this:

```py
def outer(flag: bool) -> None:
    if flag:
        x = 1
        return
    else:
        x = None

    def inner() -> None:
        reveal_type(x)  # revealed: None | Literal[2]
    inner()

    x = 2
```

The above assertion works, but if we just swap the order of the if/else clauses (which shouldn't change anything), now we get a different result:

```py
def outer(flag: bool) -> None:
    if flag:
        x = None
    else:
        x = 1
        return

    def inner() -> None:
        reveal_type(x)  # revealed: None | Literal[1, 2]
    inner()

    x = 2
```

Now we wrongly consider `x = 1` as a possibly-reachable binding, just because there's no live binding after it in source order in the snapshot bindings.

The question we really want to ask here is whether binding `id` is reachable from the point in control flow where the snapshot was taken.

---

_@carljm requested changes on 2025-09-03 19:28_

Thanks for your work on this, and sorry it took me a while to complete review! I think this is an important issue to solve, and this definitely improves the behavior. That said, I'm not convinced that the approach taken is really correct. I commented inline on the specific problematic point. Currently this PR marks snapshots as "incomplete" and then later tries to figure out how to "merge" that incomplete snapshot with all-reachable-bindings. This makes it quite difficult to figure out which of the "all-reachable" bindings should be considered vs ignored.

I think a more robust approach here might be to actually update the snapshot with new bindings, as we encounter new bindings after the snapshot was created.

---

_Converted to draft by @mtshiba on 2025-09-08 02:19_

---

_Marked ready for review by @mtshiba on 2025-09-08 17:39_

---

_Review requested from @carljm by @mtshiba on 2025-09-08 17:47_

---

_@carljm approved on 2025-09-08 21:05_

Looks good, thank you!

---

_Merged by @carljm on 2025-09-08 21:08_

---

_Closed by @carljm on 2025-09-08 21:08_

---

_Branch deleted on 2025-09-09 01:23_

---
