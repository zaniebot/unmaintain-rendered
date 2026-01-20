```yaml
number: 22498
title: "[ty] Update salsa to fix out-of-order query validation"
type: pull_request
state: open
author: MichaReiser
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: micha/cycle-validation-order
created_at: 2026-01-10T18:02:49Z
updated_at: 2026-01-20T08:31:14Z
url: https://github.com/astral-sh/ruff/pull/22498
synced_at: 2026-01-20T08:39:33Z
```

# [ty] Update salsa to fix out-of-order query validation

---

_@MichaReiser_

## Summary

Pulls in the very much in-progress https://github.com/salsa-rs/salsa/pull/1061 

I know, this panics in a ton of places. It's  unfinished but I wnat to get some initial perf numbers to see if the approach is feasible perf wise

## Test Plan

<!-- How was it tested? -->


---

_Label `bug` added by @MichaReiser on 2026-01-10 18:02_

---

_Label `ty` added by @MichaReiser on 2026-01-10 18:02_

---

_Comment by @astral-sh-bot[bot] on 2026-01-10 18:04_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-10 18:06_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 46 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2057 diagnostics
+ Found 2056 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~33MB
+     memo metadata = ~31MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~287MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~690MB
+ TOTAL MEMORY USAGE: ~725MB
-     memo metadata = ~167MB
+     memo metadata = ~194MB


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-10 18:12_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @MichaReiser on 2026-01-10 18:13_

Okay, perf looks pretty neutral. Some of the panics look absolutely terrifying (yay, panics in unsafe code)

---

_Comment by @codspeed-hq[bot] on 2026-01-19 14:22_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **improve performance by 4.05%**

<sub>Comparing <code>micha/cycle-validation-order</code> (2bb6aa3) with <code>main</code> (0cbe2af)</sub>



### Summary

`⚡ 1` improved benchmark  
`✅ 52` untouched benchmarks  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 6.1 ms | 5.9 ms | +4.05% |


---

_Comment by @MichaReiser on 2026-01-19 14:26_

ouch (perf and memory usage)

---

_Comment by @MichaReiser on 2026-01-19 14:26_

But hey, all tests pass

---

_Comment by @MichaReiser on 2026-01-19 15:00_

Okay, this looks less terrible. The memory regression is a bit brutal

---

_Comment by @AlexWaygood on 2026-01-19 16:48_

> Okay, this looks less terrible. The memory regression is a bit brutal

Notably, only on prefect, where we know (from our flaky mypy_primer comments of late) that we encounter some very bad cycles.

(Not saying we shouldn't be concerned about the memory regression, just speculating that that's one reason why it might show up especially on prefect, possibly?)

---

_Comment by @MichaReiser on 2026-01-19 16:53_

Yeah, I suspect that prefect has some very large cycles. This becomes an issue with the new approach, where we flatten all inputs (reads to tracked struct, inputs, and created interned values) for every cycle head. This leads to a lot of redundant metadata (we no longer get the nice binary-tree memory saving where we only have one dependency when we call a query, instead we flatten that query's dependency too)

The memory regression is also already much less terryfing :) 

---

_Comment by @MichaReiser on 2026-01-20 08:28_

The memo metadata increase for prefect is pretty substantial and mainly due to that we store metadata for `infer_expression_types_impl` and `infer_definition_types`. I suspect due to some larger cycles. [Here's the full memory report diff](https://www.diffchecker.com/SyXaXRSO/).

However, in total, it's only a 1.5% increase. There are other impactful changes that we can make in Salsa to compensate for this increase (within the same revision LRU, immortal durability)

Perf looks pretty good now. The simplified `deep_verify_memo` pays for most of the extra overhead incurred by cyclic queries. 

---
