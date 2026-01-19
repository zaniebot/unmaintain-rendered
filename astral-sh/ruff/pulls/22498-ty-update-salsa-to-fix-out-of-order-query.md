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
updated_at: 2026-01-19T14:22:52Z
url: https://github.com/astral-sh/ruff/pull/22498
synced_at: 2026-01-19T14:23:24Z
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


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

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

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
+ rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2055 diagnostics
+ Found 2056 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14492 diagnostics
+ Found 14491 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
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

### Merging this PR will **degrade performance by 28.59%**

<sub>Comparing <code>micha/cycle-validation-order</code> (044784d) with <code>main</code> (5949fa0)</sub>



### Summary

`❌ 22` regressed benchmarks  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` altair ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Aaltair&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 4.4 s | 5.6 s | -20.89% |
| ❌ | WallTime | [`` pandas ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apandas&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 62.8 s | 75 s | -16.26% |
| ❌ | WallTime | [`` tanjun ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Atanjun&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.5 s | 3.5 s | -28.59% |
| ❌ | WallTime | [`` sympy ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asympy&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 50.7 s | 63.5 s | -20.11% |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 21.1 s | 24.5 s | -13.88% |
| ❌ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 88.2 s | 94.8 s | -6.92% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.7 s | 9.4 s | -17.98% |
| ❌ | WallTime | [`` multithreaded ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amultithreaded&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.1 s | 1.5 s | -22.84% |
| ❌ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 8 s | 10.5 s | -22.99% |
| ❌ | Simulation | [`` ty_check_file[cold] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_cold%3A%3Aty_check_file%5Bcold%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 130.4 ms | 145.7 ms | -10.5% |
| ❌ | Simulation | [`` ty_micro[many_enum_members_2] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_enum_members_2%3A%3Aty_micro%5Bmany_enum_members_2%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 153 ms | 169.8 ms | -9.9% |
| ❌ | Simulation | [`` ty_micro[complex_constrained_attributes_1] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_complex_constrained_attributes_1%3A%3Aty_micro%5Bcomplex_constrained_attributes_1%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 67.8 ms | 70.8 ms | -4.27% |
| ❌ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 6.1 ms | 6.4 ms | -4.66% |
| ❌ | Simulation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.2 s | 1.4 s | -15.73% |
| ❌ | Simulation | [`` ty_micro[many_tuple_assignments] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_tuple_assignments%3A%3Aty_micro%5Bmany_tuple_assignments%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 65.8 ms | 69.5 ms | -5.33% |
| ❌ | Simulation | [`` ty_micro[many_enum_members] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_enum_members%3A%3Aty_micro%5Bmany_enum_members%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 122.6 ms | 143.2 ms | -14.42% |
| ❌ | Simulation | [`` ty_micro[complex_constrained_attributes_3] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_complex_constrained_attributes_3%3A%3Aty_micro%5Bcomplex_constrained_attributes_3%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 71.1 ms | 74.8 ms | -4.95% |
| ❌ | Simulation | [`` ty_micro[many_tuple_assignments] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_tuple_implicit_instance_attributes%3A%3Aty_micro%5Bmany_tuple_assignments%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 67.7 ms | 71 ms | -4.54% |
| ❌ | Simulation | [`` ty_micro[many_string_assignments] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_string_assignments%3A%3Aty_micro%5Bmany_string_assignments%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 77 ms | 84.3 ms | -8.6% |
| ❌ | Simulation | [`` DateType ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Adatetype%3A%3Aproject%3A%3ADateType&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 236.8 ms | 265.9 ms | -10.95% |
| ... | ... | ... | ... | ... | ... |

<br/>

> :information_source: _Only the first 20 benchmarks are displayed. [Go to the app to view all benchmarks](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcycle-validation-order?utm_source=github&utm_medium=comment&utm_content=all_benchmarks)._


---
