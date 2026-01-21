```yaml
number: 22777
title: "[ty] Use a simpler ordering for BDD variables"
type: pull_request
state: open
author: dcreager
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: dcreager/simpler-ordering
created_at: 2026-01-20T21:28:03Z
updated_at: 2026-01-21T00:02:50Z
url: https://github.com/astral-sh/ruff/pull/22777
synced_at: 2026-01-21T00:50:46Z
```

# [ty] Use a simpler ordering for BDD variables

---

_@dcreager_

This PR updates the ordering that we use for the BDD variables in our constraint set representation. If we only care about _correctness_, we can choose any ordering that we want, as long as it's consistent. However, different orderings can have very different _performance_ characteristics. Many BDD libraries attempt to reorder variables on the fly while building and working with BDDs. We don't do that, but we have tried to make some simple choices that have clear wins.

In particular, we now use the IDs that salsa assigns to each constraint as it is created. This tends to ensure that constraints that are close to each other in the source are also close to each other in the BDD structure.

As an optimization, we also _reverse_ this ordering, so that constraints that appear earlier in the source appear "lower" (closer to the terminal nodes) in the BDD. Since we build up BDDs by combining smaller BDDs (which will have been constructed from expressions earlier in the source), this tends to minimize the amount of "node shuffling" that we have to do when combining BDDs.

Previously, we tried to be more clever — for instance, by comparing the typevars of each constraint first, in an attempt to keep all of the constraints for a single typevar adjacent in the BDD structure. However, this proved to be counterproductive; we've found empirically that we get smaller BDDs with an ordering that is more aligned with source order.


---

_Review requested from @carljm by @dcreager on 2026-01-20 21:28_

---

_Review requested from @AlexWaygood by @dcreager on 2026-01-20 21:28_

---

_Review requested from @sharkdp by @dcreager on 2026-01-20 21:28_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 21:29_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-20 21:30_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_validators.py:1257:20: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["spam"]`
+ tests/test_validators.py:1257:20: error[invalid-argument-type] Argument is incorrect: Expected `float | int`, found `Literal["spam"]`

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/kernel/__init__.py:218:20: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `Literal[1]`
+ pwndbg/aglib/kernel/__init__.py:218:20: error[unsupported-operator] Operator `+` is not supported between objects of type `None | Unknown` and `Literal[1]`
- pwndbg/aglib/kernel/__init__.py:224:22: error[unsupported-operator] Operator `+` is not supported between objects of type `Unknown | None` and `int`
+ pwndbg/aglib/kernel/__init__.py:224:22: error[unsupported-operator] Operator `+` is not supported between objects of type `None | Unknown` and `int`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
+ tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4454 diagnostics
+ Found 4456 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2058 diagnostics
+ Found 2059 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~12MB
+     struct fields = ~11MB


```

</details>




---

_Label `internal` added by @dcreager on 2026-01-20 21:37_

---

_Label `ty` added by @dcreager on 2026-01-20 21:37_

---

_Label `ecosystem-analyzer` added by @dcreager on 2026-01-20 21:37_

---

_@MichaReiser approved on 2026-01-20 21:42_

---

_Comment by @astral-sh-bot[bot] on 2026-01-20 21:43_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 2 | 2 | 4 |
| `invalid-await` | 0 | 2 | 6 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-return-type` | 1 | 0 | 3 |
| `possibly-missing-attribute` | 0 | 3 | 1 |
| `unused-ignore-comment` | 1 | 2 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unsupported-operator` | 0 | 0 | 2 |
| **Total** | **4** | **9** | **23** |


**[Full report with detailed diff](https://3b5e2689.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://3b5e2689.ty-ecosystem-ext.pages.dev/timing))



---

_@ibraheemdev approved on 2026-01-21 00:02_

---
