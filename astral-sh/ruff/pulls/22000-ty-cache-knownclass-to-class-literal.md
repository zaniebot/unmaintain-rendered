```yaml
number: 22000
title: "[ty] Cache `KnownClass::to_class_literal`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: micha/cache-known-class-to-class-literal
created_at: 2025-12-16T10:08:48Z
updated_at: 2025-12-16T12:04:15Z
url: https://github.com/astral-sh/ruff/pull/22000
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Cache `KnownClass::to_class_literal`

---

_@MichaReiser_

This PR adds salsa caching to `KnownClass::to_class_literal`. 

I noticed that we spend about 4.5% of discord.py's entire check time in `KnownClass::to_class_literal` which seemed high. That's why I added salsa caching to the lookup and the results are much better than I thought because:

* `KnownClass::Module::to_class_literal` ends up in a cycle. Adding cycle handling to `to_class_literal` reduces the size of the cycle. `imported_symbol` (which `KnownClass::to_class_literal` uses resolves `KnownClass::Module.to_instance`). It might be worth trying if we can resolve this cycle somehow
* We tracked the dependencies of `KnownClass::to_class_literal` on the calling query, adding many dependencies to every query that uses a `KnownClass`. Making `KnownClass::to_class_literal` a query ensures that we store the dependencies only once, and dependent query only have to depend on `KnownClass::to_class_literal`. This helps reduce the memo metadata.
* We avoid recomputing the same value which is obviously great

---

_Label `internal` added by @MichaReiser on 2025-12-16 10:08_

---

_Label `ty` added by @MichaReiser on 2025-12-16 10:08_

---

_Comment by @astral-sh-bot[bot] on 2025-12-16 10:10_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-16 10:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5124 diagnostics
+ Found 5123 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo metadata = ~8MB
+     memo metadata = ~7MB

trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~167MB
+ TOTAL MEMORY USAGE: ~159MB
-     memo metadata = ~35MB
+     memo metadata = ~33MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~287MB
-     memo metadata = ~76MB
+     memo metadata = ~73MB

prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~176MB
+     memo metadata = ~167MB


```

</details>




---

_Comment by @codspeed-hq[bot] on 2025-12-16 10:28_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #22000 will **improve performances by 17.44%**

<sub>Comparing <code>micha/cache-known-class-to-class-literal</code> (e73332b) with <code>main</code> (01c0a3e)</sub>



### Summary

`⚡ 13` improvements  
`✅ 9` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` large[sympy] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bsympy%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 55.7 s | 51.9 s | +7.15% |
| ⚡ | WallTime | [`` small[altair] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Baltair%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 5.6 s | 5 s | +10.61% |
| ⚡ | WallTime | [`` small[freqtrade] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bfreqtrade%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 8.5 s | 8.1 s | +4.86% |
| ⚡ | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.7 s | 2.5 s | +4.84% |
| ⚡ | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 20.9 s | 19.7 s | +6.24% |
| ⚡ | WallTime | [`` multithreaded[altair] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amultithreaded%5Baltair%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.6 s | 1.4 s | +17.44% |
| ⚡ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 109.6 s | 105.1 s | +4.26% |
| ⚡ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 69.5 s | 64.6 s | +7.59% |
| ⚡ | Simulation | [`` ty_micro[many_string_assignments] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_many_string_assignments%3A%3Aty_micro%5Bmany_string_assignments%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 85.9 ms | 82.4 ms | +4.3% |
| ⚡ | Simulation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.4 s | 1.4 s | +4.61% |
| ⚡ | Simulation | [`` attrs ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aattrs%3A%3Aproject%3A%3Aattrs&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 445 ms | 426.1 ms | +4.44% |
| ⚡ | Simulation | [`` DateType ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Adatetype%3A%3Aproject%3A%3ADateType&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 237.8 ms | 227.4 ms | +4.55% |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.3 s | 1.3 s | +4.43% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/micha%2Fcache-known-class-to-class-literal?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @MichaReiser on 2025-12-16 10:32_

And I have to run all benchmarks one more time

---

_Label `internal` removed by @AlexWaygood on 2025-12-16 10:39_

---

_Label `performance` added by @AlexWaygood on 2025-12-16 10:39_

---

_Comment by @AlexWaygood on 2025-12-16 10:40_

> ```diff
> flake8 (https://github.com/pycqa/flake8)
> + WARN expected `heap_size` to be provided by Salsa struct `KnownClassArgument`
> + WARN expected `heap_size` to be provided by Salsa struct `KnownClassArgument`
> + WARN expected `heap_size` to be provided by Salsa struct `KnownClassArgument`
> + WARN expected `heap_size` to be provided by Salsa struct `KnownClassArgument`
> + WARN expected `heap_size` to be provided by Salsa struct `KnownClassArgument`
> + WARN expected `heap_size` to be provided by Salsa struct `KnownClassArgument`
> + WARN expected `heap_size` to be provided by Salsa struct `KnownClassArgument`
> + WARN expected `heap_size` to be provided by Salsa struct `KnownClassArgument`
> -     memo metadata = ~8MB
> +     memo metadata = ~7MB
> ```

Consider adding `heap_size` to the Salsa struct `KnownClassArgument` ;)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:4264 on 2025-12-16 11:31_

```suggestion
```

---

_@MichaReiser reviewed on 2025-12-16 11:31_

---

_@MichaReiser reviewed on 2025-12-16 11:32_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:5100 on 2025-12-16 11:32_

I don't think this will be a new issue but we might end up seeing this log message when we hit a salsa cycle because the cycle initial function returns `None`. I can't think of an easy way to avoid this AND keeping the log message.

---

_Comment by @MichaReiser on 2025-12-16 11:37_

The perf improvement is a wide range from 0-15% on ty benchmark

```
black
-----

Benchmark 1: ../../target/release/ty-known-class
  Time (mean ± σ):      82.5 ms ±   2.3 ms    [User: 559.1 ms, System: 77.0 ms]
  Range (min … max):    77.8 ms …  87.2 ms    35 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty
  Time (mean ± σ):      83.2 ms ±   2.3 ms    [User: 582.9 ms, System: 77.7 ms]
  Range (min … max):    78.0 ms …  86.9 ms    35 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-known-class ran
    1.01 ± 0.04 times faster than ../../target/release/ty

-------------------------------------------------------------------------------

discord.py
----------

Benchmark 1: ../../target/release/ty-known-class
  Time (mean ± σ):     341.2 ms ±   4.7 ms    [User: 2388.9 ms, System: 138.6 ms]
  Range (min … max):   334.1 ms … 348.6 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty
  Time (mean ± σ):     353.8 ms ±   6.7 ms    [User: 2490.6 ms, System: 141.8 ms]
  Range (min … max):   346.9 ms … 367.3 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-known-class ran
    1.04 ± 0.02 times faster than ../../target/release/ty

-------------------------------------------------------------------------------

homeassistant
-------------

Benchmark 1: ../../target/release/ty-known-class
  Time (mean ± σ):      2.229 s ±  0.112 s    [User: 41.375 s, System: 1.426 s]
  Range (min … max):    2.025 s …  2.389 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty
  Time (mean ± σ):      2.284 s ±  0.116 s    [User: 43.168 s, System: 1.458 s]
  Range (min … max):    2.105 s …  2.444 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-known-class ran
    1.02 ± 0.07 times faster than ../../target/release/ty

-------------------------------------------------------------------------------

isort
-----

Benchmark 1: ../../target/release/ty-known-class
  Time (mean ± σ):      63.1 ms ±   2.2 ms    [User: 261.6 ms, System: 52.1 ms]
  Range (min … max):    59.0 ms …  68.2 ms    43 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty
  Time (mean ± σ):      65.2 ms ±   1.8 ms    [User: 273.3 ms, System: 53.1 ms]
  Range (min … max):    60.6 ms …  68.3 ms    45 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-known-class ran
    1.03 ± 0.05 times faster than ../../target/release/ty

-------------------------------------------------------------------------------

jinja
-----

Benchmark 1: ../../target/release/ty-known-class
  Time (mean ± σ):     167.7 ms ±   3.7 ms    [User: 512.6 ms, System: 64.8 ms]
  Range (min … max):   159.8 ms … 176.1 ms    17 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty
  Time (mean ± σ):     166.7 ms ±   3.1 ms    [User: 530.8 ms, System: 61.9 ms]
  Range (min … max):   161.8 ms … 174.7 ms    18 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty ran
    1.01 ± 0.03 times faster than ../../target/release/ty-known-class

-------------------------------------------------------------------------------

pandas
------

Benchmark 1: ../../target/release/ty-known-class
  Time (mean ± σ):     678.8 ms ±  32.9 ms    [User: 8864.6 ms, System: 234.4 ms]
  Range (min … max):   643.1 ms … 729.8 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty
  Time (mean ± σ):     782.5 ms ±  35.2 ms    [User: 9221.0 ms, System: 249.8 ms]
  Range (min … max):   739.5 ms … 833.7 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-known-class ran
    1.15 ± 0.08 times faster than ../../target/release/ty

-------------------------------------------------------------------------------

pandas-stubs
------------

Benchmark 1: ../../target/release/ty-known-class
  Time (mean ± σ):     124.9 ms ±   4.0 ms    [User: 627.9 ms, System: 80.8 ms]
  Range (min … max):   117.8 ms … 132.8 ms    23 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty
  Time (mean ± σ):     128.5 ms ±   3.3 ms    [User: 660.5 ms, System: 79.5 ms]
  Range (min … max):   122.3 ms … 137.3 ms    21 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-known-class ran
    1.03 ± 0.04 times faster than ../../target/release/ty

-------------------------------------------------------------------------------

prefect
-------

Benchmark 1: ../../target/release/ty-known-class
  Time (mean ± σ):     133.4 ms ±   3.2 ms    [User: 848.3 ms, System: 104.5 ms]
  Range (min … max):   127.7 ms … 138.9 ms    22 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty
  Time (mean ± σ):     133.6 ms ±   3.2 ms    [User: 873.0 ms, System: 109.8 ms]
  Range (min … max):   127.8 ms … 139.9 ms    22 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-known-class ran
    1.00 ± 0.03 times faster than ../../target/release/ty

-------------------------------------------------------------------------------

pytorch
-------

Benchmark 1: ../../target/release/ty-known-class
  Time (mean ± σ):      1.065 s ±  0.016 s    [User: 20.199 s, System: 0.731 s]
  Range (min … max):    1.046 s …  1.089 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty
  Time (mean ± σ):      1.128 s ±  0.021 s    [User: 21.335 s, System: 0.756 s]
  Range (min … max):    1.089 s …  1.169 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-known-class ran
    1.06 ± 0.03 times faster than ../../target/release/ty

```

---

_Marked ready for review by @MichaReiser on 2025-12-16 11:39_

---

_Review requested from @carljm by @MichaReiser on 2025-12-16 11:39_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-16 11:39_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-16 11:39_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-16 11:39_

---

_@AlexWaygood approved on 2025-12-16 11:54_

let's go 🚀

---

_Merged by @MichaReiser on 2025-12-16 12:04_

---

_Closed by @MichaReiser on 2025-12-16 12:04_

---

_Branch deleted on 2025-12-16 12:04_

---
