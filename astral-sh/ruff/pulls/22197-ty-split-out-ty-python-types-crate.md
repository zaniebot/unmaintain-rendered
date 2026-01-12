```yaml
number: 22197
title: "[ty] Split out `ty_python_types` crate"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: micha/ty-python-types
created_at: 2025-12-25T16:37:29Z
updated_at: 2025-12-28T08:57:04Z
url: https://github.com/astral-sh/ruff/pull/22197
synced_at: 2026-01-12T15:57:43Z
```

# [ty] Split out `ty_python_types` crate

---

_@MichaReiser_

## Summary

Surprisingly, this doesn't really help with cold compile times. 
I think mainly because `ty_python_types` depends on `ty_python_semantic`, so the compilation is mostly sequential.

However, this should help with incremental compilation. 

The obvious downside is that it's now necessary to make more types `pub`. 


To give a sense of the size of the two crates and why splitting doesn't necessarily help that much:

**`ty_python_types`**

```
───────────────────────────────────────────────────────────────────────────────
Language            Files       Lines    Blanks  Comments       Code Complexity
───────────────────────────────────────────────────────────────────────────────
Python                319       1,573       190        66      1,317        297
Markdown              306      92,480    24,400         0     68,080          0
Rust                   53      83,605     7,066    12,430     64,109      6,422
TOML                    2          86         8         0         78          0
───────────────────────────────────────────────────────────────────────────────
Total                 680     177,744    31,664    12,496    133,584      6,719
───────────────────────────────────────────────────────────────────────────────
Estimated Cost to Develop (organic) $4,609,326
Estimated Schedule Effort (organic) 24.58 months
Estimated People Required (organic) 16.66
───────────────────────────────────────────────────────────────────────────────
Processed 5978645 bytes, 5.979 megabytes (SI)
───────────────────────────────────────────────────────────────────────────────
```

**`ty_python_semantic`**

```
───────────────────────────────────────────────────────────────────────────────
Language            Files       Lines    Blanks  Comments       Code Complexity
───────────────────────────────────────────────────────────────────────────────
Rust                   36      19,827     2,346     3,120     14,361      1,208
Plain Text              3         158         0         0        158          0
Python                  1         292        38        23        231         47
TOML                    1          77         6         0         71          0
───────────────────────────────────────────────────────────────────────────────
Total                  41      20,354     2,390     3,143     14,821      1,255
───────────────────────────────────────────────────────────────────────────────
Estimated Cost to Develop (organic) $458,159
Estimated Schedule Effort (organic) 10.22 months
Estimated People Required (organic) 3.98
───────────────────────────────────────────────────────────────────────────────
Processed 753094 bytes, 0.753 megabytes (SI)
───────────────────────────────────────────────────────────────────────────────

```


DISCLAIMER: I haven't yet reviewed what Claude generated :) I know there's at least one failing test

## Test Plan

<!-- How was it tested? -->


---

_Label `internal` added by @MichaReiser on 2025-12-25 16:37_

---

_Label `ty` added by @MichaReiser on 2025-12-25 16:37_

---

_Comment by @astral-sh-bot[bot] on 2025-12-25 16:39_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-25 16:40_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
jax (https://github.com/google/jax)
+ jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
+ jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2800 diagnostics
+ Found 2803 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- Found 1843 diagnostics
+ Found 1844 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5135 diagnostics
+ Found 5134 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-25 16:45_


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

_Comment by @codspeed-hq[bot] on 2025-12-25 17:04_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fty-python-types?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22197 will **degrade performance by 4.66%**

<sub>Comparing <code>micha/ty-python-types</code> (0ad2e09) with <code>main</code> (014abe1)</sub>



### Summary

`❌ 2` regressions  
`✅ 50` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fty-python-types?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` altair ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fty-python-types?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Aaltair&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 5 s | 5.2 s | -4.18% |
| ❌ | WallTime | [`` sympy ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fty-python-types?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asympy&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 51 s | 53.5 s | -4.66% |


---

_Comment by @MichaReiser on 2025-12-25 17:08_

Surprise, less inlining is bad for performance :laughing: 

---

_Closed by @MichaReiser on 2025-12-28 08:57_

---
