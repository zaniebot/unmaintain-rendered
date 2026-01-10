```yaml
number: 21070
title: "[ty] Prefer declared type of generic classes"
type: pull_request
state: closed
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: ibraheem/declared-generic-type
created_at: 2025-10-25T02:50:37Z
updated_at: 2025-11-03T00:32:22Z
url: https://github.com/astral-sh/ruff/pull/21070
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Prefer declared type of generic classes

---

_Pull request opened by @ibraheemdev on 2025-10-25 02:50_

## Summary

Extends https://github.com/astral-sh/ruff/pull/20927 to generic call inference, preferring the declared type of generic classes:

```py
def f[T](x: T) -> list[T]:
    return [x]

x: list[Any] = f(1)
reveal_type(x)  # before: list[Literal[1]], after: list[Any]
```

Part of https://github.com/astral-sh/ty/issues/136.

---

_Review requested from @carljm by @ibraheemdev on 2025-10-25 02:50_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-10-25 02:50_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-10-25 02:50_

---

_Review requested from @dcreager by @ibraheemdev on 2025-10-25 02:50_

---

_Label `ty` added by @ibraheemdev on 2025-10-25 02:50_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-25 02:55_

---

_Comment by @github-actions[bot] on 2025-10-25 02:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-25 02:58_

---

_Comment by @github-actions[bot] on 2025-10-25 03:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dulwich (https://github.com/dulwich/dulwich)
- dulwich/pack.py:1703:83: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 172 diagnostics
+ Found 171 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_validators.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[bytes]`, found `Pattern[str | bytes]`
+ pydantic/_internal/_validators.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[bytes]`, found `Pattern[bytes | str]`

Expression (https://github.com/cognitedata/Expression)
- tests/test_result.py:491:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Literal[42], Any]`, found `Result[Literal[0], Any]`
- tests/test_result.py:509:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Any, Literal["original error"]]`, found `Result[Any, Literal["new error"]]`
- Found 201 diagnostics
+ Found 199 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/domains/python/__init__.py:672:42: error[index-out-of-bounds] Index 0 is out of bounds for string `Literal[""]` with length 0
- Found 530 diagnostics
+ Found 529 diagnostics

jax (https://github.com/google/jax)
- jax/_src/interpreters/mlir.py:1484:5: error[invalid-assignment] Object of type `OrderedDict[str, Unknown]` is not assignable to attribute `_tokens` of type `OrderedDict[Effect, Unknown]`
- Found 2544 diagnostics
+ Found 2543 diagnostics

rotki (https://github.com/rotki/rotki)
+ rotkehlchen/exchanges/coinbase.py:524:20: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `defaultdict[str, list[dict[Unknown, Unknown]]]`
- Found 1684 diagnostics
+ Found 1685 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/helpers/service.py:913:9: error[invalid-assignment] Object of type `object` is not assignable to `ServiceResponse`
- Found 14272 diagnostics
+ Found 14273 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~33MB
+     struct metadata = ~35MB

```
</details>


---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-25 03:05_

---

_Comment by @github-actions[bot] on 2025-10-25 03:11_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 2 | 1 | 0 |
| `invalid-argument-type` | 0 | 2 | 0 |
| `index-out-of-bounds` | 0 | 1 | 0 |
| `invalid-return-type` | 0 | 0 | 1 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **2** | **5** | **1** |

**[Full report with detailed diff](https://ibraheem-declared-generic-ty.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-declared-generic-ty.ecosystem-663.pages.dev/timing))


---

_Comment by @ibraheemdev on 2025-10-25 03:48_

I'm not sure if we should restrict this change to invariant generics. It seems reasonable to use `frozendict[str, Any]` as an implicit typed dict, so preferring the `Any` there might still be meaningful (even thought it feels strictly worse).

---

_Converted to draft by @ibraheemdev on 2025-10-25 03:49_

---

_Comment by @carljm on 2025-10-30 16:06_

@ibraheemdev Is this blocked? What's keeping it in draft?

---

_Comment by @codspeed-hq[bot] on 2025-10-30 21:32_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fdeclared-generic-type?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21070 will **not alter performance**

<sub>Comparing <code>ibraheem/declared-generic-type</code> (2f4dcbf) with <code>main</code> (1d111c8)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fdeclared-generic-type?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @ibraheemdev on 2025-10-30 21:44_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-10-31 15:21_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-10-31 15:21_

---

_Comment by @ibraheemdev on 2025-10-31 15:38_

The ecosystem changes all seem correct.

---

_Comment by @ibraheemdev on 2025-11-03 00:32_

Superceded by https://github.com/astral-sh/ruff/pull/21210.

---

_Closed by @ibraheemdev on 2025-11-03 00:32_

---
