```yaml
number: 21975
title: "[ty] Use jemalloc on linux"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: claude/custom-allocators-mimalloc-01VFGzka5wDLFvvNY9bHrHgG
created_at: 2025-12-14T09:30:43Z
updated_at: 2025-12-15T16:24:50Z
url: https://github.com/astral-sh/ruff/pull/21975
synced_at: 2026-01-12T15:57:38Z
```

# [ty] Use jemalloc on linux

---

_@MichaReiser_

## Summary

After reviewing the results in the thread below, I propose using jemalloc on Linux only and considering a custom allocator on Windows separately. 

**Linux**

* The performance improvements on Linux are very substantial
* Linux is very popular in CI
* Jemalloc uses less memory in cold runs 
* I also didn't see a significant memory regression (seems even to be better?) in the LSP use case

I don't expect this to move the needle on our benchmarks as the improvements mainly show when running ty on many cores.

**Windows**:

* Using mimalloc would improve performance significantly
* but mimalloc uses a lot more memory in the LSP use case (or I measure it all wrong)
* The performance in the LSP is mainly noticeable in workspace symbols and completions, but that's something that @BurntSushi plans to fix separately anyway and is also something I noticed on Linux

It might be worth using mimalloc in the future but this certainly requires a more in-depth analysis.


**macOS**:

* The performance improvements of using jemalloc over the system allocator are way less convincing
* But the increased memory usage in the LSP use case is concerning. 

Part of https://github.com/astral-sh/ty/issues/644

## Test Plan

See the following comments on this PR


---

_Comment by @astral-sh-bot[bot] on 2025-12-14 09:32_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-14 09:38_


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

_Comment by @astral-sh-bot[bot] on 2025-12-14 09:41_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Comment by @MichaReiser on 2025-12-14 10:16_

## MacOS


mimalloc wins consistently</strong> across almost all benchmarks, with speedups typically ranging from 1.03× to 1.13× over the system allocator.

Project | Winner | vs System | vs jemalloc
-- | -- | -- | --
black | mimalloc | 1.08× faster | 1.13× faster
discord.py | mimalloc | 1.11× faster | 1.05× faster
homeassistant | mimalloc | 1.04× faster | 1.03× faster
isort | mimalloc | 1.08× faster | 1.10× faster
jinja | mimalloc | 1.10× faster | 1.07× faster
pandas | jemalloc | 1.09× faster | 1.05× vs mimalloc
pandas-stubs | mimalloc | 1.07× faster | 1.07× faster
prefect | mimalloc | 1.10× faster | 1.09× faster
pytorch | mimalloc | 1.05× faster | 1.04× faster


<p><strong>Key takeaways:</strong></p>
<ul>
<li><strong>mimalloc</strong> is the clear winner for ty's workload, winning 8 of 9 benchmarks</li>
<li><strong>pandas</strong> is the sole exception where jemalloc edges out mimalloc by ~5%</li>
<li>The system allocator consistently comes in last</li>
<li>Gains are most pronounced on smaller/faster projects (8-13% improvement) and diminish somewhat on larger codebases like homeassistant and pytorch (3-5% improvement)</li>
</ul></body></html>

<details>
	<summary>Benchmark results</summary>

```
uv run benchmark --tool ty --ty-path ../../target/release/ty-system --ty-path ../../target/release/ty-jemalloc --ty-path ../../target/release/ty-mimalloc
black
-----

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):      55.1 ms ±   1.1 ms    [User: 350.4 ms, System: 42.1 ms]
  Range (min … max):    52.8 ms …  57.7 ms    49 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):      57.5 ms ±   1.3 ms    [User: 302.4 ms, System: 69.7 ms]
  Range (min … max):    55.3 ms …  60.5 ms    47 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      50.9 ms ±   0.8 ms    [User: 295.0 ms, System: 43.2 ms]
  Range (min … max):    49.1 ms …  52.8 ms    52 runs

  Warning: Ignoring non-zero exit code.

Summary
  ../../target/release/ty-mimalloc ran
    1.08 ± 0.03 times faster than ../../target/release/ty-system
    1.13 ± 0.03 times faster than ../../target/release/ty-jemalloc

-------------------------------------------------------------------------------

discord.py
----------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):     257.5 ms ±   2.0 ms    [User: 1465.6 ms, System: 122.1 ms]
  Range (min … max):   255.2 ms … 262.1 ms    11 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):     244.5 ms ±   1.9 ms    [User: 1309.4 ms, System: 157.2 ms]
  Range (min … max):   242.4 ms … 248.5 ms    12 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):     232.8 ms ±   2.1 ms    [User: 1298.4 ms, System: 125.6 ms]
  Range (min … max):   229.6 ms … 235.5 ms    12 runs

  Warning: Ignoring non-zero exit code.

Summary
  ../../target/release/ty-mimalloc ran
    1.05 ± 0.01 times faster than ../../target/release/ty-jemalloc
    1.11 ± 0.01 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

homeassistant
-------------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):      2.173 s ±  0.080 s    [User: 22.585 s, System: 3.161 s]
  Range (min … max):    2.082 s …  2.330 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):      2.152 s ±  0.092 s    [User: 21.244 s, System: 3.537 s]
  Range (min … max):    2.041 s …  2.280 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      2.087 s ±  0.052 s    [User: 21.045 s, System: 3.235 s]
  Range (min … max):    2.002 s …  2.177 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ../../target/release/ty-mimalloc ran
    1.03 ± 0.05 times faster than ../../target/release/ty-jemalloc
    1.04 ± 0.05 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

isort
-----

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):      39.1 ms ±   1.2 ms    [User: 165.7 ms, System: 23.1 ms]
  Range (min … max):    36.9 ms …  42.0 ms    67 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):      40.0 ms ±   0.9 ms    [User: 148.2 ms, System: 41.9 ms]
  Range (min … max):    38.0 ms …  42.0 ms    64 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      36.2 ms ±   0.7 ms    [User: 145.2 ms, System: 24.7 ms]
  Range (min … max):    34.6 ms …  38.1 ms    70 runs

  Warning: Ignoring non-zero exit code.

Summary
  ../../target/release/ty-mimalloc ran
    1.08 ± 0.04 times faster than ../../target/release/ty-system
    1.10 ± 0.03 times faster than ../../target/release/ty-jemalloc

-------------------------------------------------------------------------------

jinja
-----

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):     108.0 ms ±   2.1 ms    [User: 329.0 ms, System: 31.6 ms]
  Range (min … max):   104.9 ms … 115.6 ms    25 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):     104.6 ms ±   1.7 ms    [User: 292.2 ms, System: 53.0 ms]
  Range (min … max):   102.3 ms … 108.1 ms    27 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      98.0 ms ±   0.8 ms    [User: 288.7 ms, System: 32.1 ms]
  Range (min … max):    96.7 ms … 100.4 ms    28 runs

  Warning: Ignoring non-zero exit code.

Summary
  ../../target/release/ty-mimalloc ran
    1.07 ± 0.02 times faster than ../../target/release/ty-jemalloc
    1.10 ± 0.02 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

pandas
------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):     605.6 ms ±  45.9 ms    [User: 5148.3 ms, System: 342.9 ms]
  Range (min … max):   535.3 ms … 679.4 ms    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):     554.7 ms ±  40.6 ms    [User: 4677.6 ms, System: 394.0 ms]
  Range (min … max):   515.0 ms … 625.9 ms    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):     585.1 ms ±  32.8 ms    [User: 4642.6 ms, System: 342.5 ms]
  Range (min … max):   539.8 ms … 638.6 ms    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ../../target/release/ty-jemalloc ran
    1.05 ± 0.10 times faster than ../../target/release/ty-mimalloc
    1.09 ± 0.12 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

pandas-stubs
------------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):      88.4 ms ±   3.6 ms    [User: 392.0 ms, System: 55.9 ms]
  Range (min … max):    79.7 ms …  99.7 ms    32 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):      88.2 ms ±   4.3 ms    [User: 355.0 ms, System: 80.6 ms]
  Range (min … max):    82.0 ms … 105.0 ms    32 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      82.2 ms ±   2.3 ms    [User: 348.8 ms, System: 59.3 ms]
  Range (min … max):    78.5 ms …  86.7 ms    33 runs

  Warning: Ignoring non-zero exit code.

Summary
  ../../target/release/ty-mimalloc ran
    1.07 ± 0.06 times faster than ../../target/release/ty-jemalloc
    1.07 ± 0.05 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

prefect
-------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):      94.5 ms ±   0.8 ms    [User: 530.8 ms, System: 76.2 ms]
  Range (min … max):    92.9 ms …  96.7 ms    30 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):      94.0 ms ±   1.5 ms    [User: 473.3 ms, System: 100.3 ms]
  Range (min … max):    92.2 ms …  98.7 ms    30 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      85.9 ms ±   0.8 ms    [User: 462.2 ms, System: 76.3 ms]
  Range (min … max):    84.2 ms …  87.8 ms    33 runs

  Warning: Ignoring non-zero exit code.

Summary
  ../../target/release/ty-mimalloc ran
    1.09 ± 0.02 times faster than ../../target/release/ty-jemalloc
    1.10 ± 0.01 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

pytorch
-------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):      1.074 s ±  0.013 s    [User: 11.293 s, System: 1.497 s]
  Range (min … max):    1.058 s …  1.099 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):      1.065 s ±  0.018 s    [User: 10.577 s, System: 1.637 s]
  Range (min … max):    1.031 s …  1.087 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      1.023 s ±  0.015 s    [User: 10.509 s, System: 1.508 s]
  Range (min … max):    1.000 s …  1.054 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ../../target/release/ty-mimalloc ran
    1.04 ± 0.02 times faster than ../../target/release/ty-jemalloc
    1.05 ± 0.02 times faster than ../../target/release/ty-system
```

</details>

## Linux glibc Allocator Benchmark Summary

The system allocator (glibc) performs significantly worse on Linux</strong> compared to macOS, making the alternative allocators much more impactful.


Project | Winner | vs System | vs jemalloc
-- | -- | -- | --
black | mimalloc | 1.11× faster | 1.04× faster
discord.py | mimalloc | 1.17× faster | 1.02× faster
homeassistant | jemalloc | 1.36× faster | 1.02× vs mimalloc
isort | mimalloc | 1.07× faster | 1.05× faster
jinja | mimalloc | 1.18× faster | 1.04× faster
pandas | jemalloc | 1.13× faster | 1.01× vs mimalloc
pandas-stubs | mimalloc | 1.22× faster | 1.03× faster
prefect | mimalloc | 1.36× faster | 1.06× faster
pytorch | ~tie | 1.26× faster | essentially tied


<p><strong>Key differences from macOS:</strong></p>
<ul>
<li><strong>Gains over system allocator are much larger</strong> on Linux (up to 1.36×) vs macOS (up to 1.11×) — glibc's allocator really struggles with ty's allocation patterns</li>
<li><strong>jemalloc and mimalloc are much closer</strong> on Linux, often within measurement noise</li>
<li><strong>jemalloc wins</strong> on homeassistant and pandas (larger/more complex codebases)</li>
<li><strong>mimalloc wins</strong> on most smaller-to-medium projects, but the margins are tighter than on macOS</li>
</ul>
<p><strong>Bottom line:</strong> On Linux, either jemalloc or mimalloc is a massive win over glibc. The choice between them matters less than on macOS, though mimalloc still has a slight edge overall.</p></body></html>

<details>
<summary>Benchmark results</summary>

```
black
-----

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):      87.4 ms ±   3.0 ms    [User: 598.6 ms, System: 133.8 ms]
  Range (min … max):    81.6 ms …  95.0 ms    34 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):      81.4 ms ±   2.4 ms    [User: 576.4 ms, System: 73.7 ms]
  Range (min … max):    77.8 ms …  90.3 ms    37 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      78.4 ms ±   2.0 ms    [User: 623.8 ms, System: 174.0 ms]
  Range (min … max):    75.0 ms …  82.5 ms    36 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-mimalloc ran
    1.04 ± 0.04 times faster than ../../target/release/ty-jemalloc
    1.11 ± 0.05 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

discord.py
----------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):     395.3 ms ±   3.9 ms    [User: 2661.9 ms, System: 334.8 ms]
  Range (min … max):   387.2 ms … 401.2 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):     344.4 ms ±   3.8 ms    [User: 2494.0 ms, System: 133.5 ms]
  Range (min … max):   340.0 ms … 349.3 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):     338.9 ms ±   9.0 ms    [User: 2512.9 ms, System: 209.2 ms]
  Range (min … max):   323.4 ms … 356.1 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-mimalloc ran
    1.02 ± 0.03 times faster than ../../target/release/ty-jemalloc
    1.17 ± 0.03 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

homeassistant
-------------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):      3.147 s ±  0.122 s    [User: 46.071 s, System: 8.655 s]
  Range (min … max):    2.899 s …  3.320 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):      2.313 s ±  0.104 s    [User: 42.775 s, System: 1.451 s]
  Range (min … max):    2.159 s …  2.477 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      2.355 s ±  0.112 s    [User: 43.010 s, System: 1.628 s]
  Range (min … max):    2.174 s …  2.527 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-jemalloc ran
    1.02 ± 0.07 times faster than ../../target/release/ty-mimalloc
    1.36 ± 0.08 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

isort
-----

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):      65.7 ms ±   1.9 ms    [User: 279.6 ms, System: 67.0 ms]
  Range (min … max):    61.9 ms …  70.3 ms    45 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):      64.6 ms ±   1.9 ms    [User: 273.0 ms, System: 51.9 ms]
  Range (min … max):    60.2 ms …  69.4 ms    43 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      61.4 ms ±   1.7 ms    [User: 291.6 ms, System: 131.6 ms]
  Range (min … max):    57.9 ms …  65.6 ms    49 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-mimalloc ran
    1.05 ± 0.04 times faster than ../../target/release/ty-jemalloc
    1.07 ± 0.04 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

jinja
-----

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):     185.7 ms ±   4.1 ms    [User: 562.1 ms, System: 115.1 ms]
  Range (min … max):   178.8 ms … 192.1 ms    16 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):     164.2 ms ±   3.1 ms    [User: 520.9 ms, System: 67.7 ms]
  Range (min … max):   159.5 ms … 171.4 ms    18 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):     157.8 ms ±   3.3 ms    [User: 557.3 ms, System: 161.0 ms]
  Range (min … max):   151.0 ms … 163.2 ms    19 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-mimalloc ran
    1.04 ± 0.03 times faster than ../../target/release/ty-jemalloc
    1.18 ± 0.04 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

pandas
------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):     849.3 ms ±  49.7 ms    [User: 9947.3 ms, System: 857.1 ms]
  Range (min … max):   794.5 ms … 936.7 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):     753.8 ms ±  40.1 ms    [User: 9202.6 ms, System: 253.8 ms]
  Range (min … max):   701.4 ms … 832.1 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):     759.5 ms ±  44.4 ms    [User: 9084.8 ms, System: 334.8 ms]
  Range (min … max):   697.4 ms … 810.1 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-jemalloc ran
    1.01 ± 0.08 times faster than ../../target/release/ty-mimalloc
    1.13 ± 0.09 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

pandas-stubs
------------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):     149.9 ms ±   5.4 ms    [User: 717.7 ms, System: 151.1 ms]
  Range (min … max):   136.8 ms … 162.4 ms    19 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):     126.2 ms ±   3.1 ms    [User: 649.1 ms, System: 85.1 ms]
  Range (min … max):   121.4 ms … 133.1 ms    23 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):     122.7 ms ±   3.2 ms    [User: 666.4 ms, System: 151.3 ms]
  Range (min … max):   115.9 ms … 128.5 ms    24 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-mimalloc ran
    1.03 ± 0.04 times faster than ../../target/release/ty-jemalloc
    1.22 ± 0.05 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

prefect
-------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):     169.5 ms ±   4.1 ms    [User: 919.1 ms, System: 257.5 ms]
  Range (min … max):   160.5 ms … 175.5 ms    17 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):     132.1 ms ±   3.6 ms    [User: 864.1 ms, System: 107.3 ms]
  Range (min … max):   125.0 ms … 139.2 ms    22 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):     125.0 ms ±   3.1 ms    [User: 883.0 ms, System: 178.9 ms]
  Range (min … max):   117.7 ms … 130.4 ms    23 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-mimalloc ran
    1.06 ± 0.04 times faster than ../../target/release/ty-jemalloc
    1.36 ± 0.05 times faster than ../../target/release/ty-system

-------------------------------------------------------------------------------

pytorch
-------

Benchmark 1: ../../target/release/ty-system
  Time (mean ± σ):      1.443 s ±  0.020 s    [User: 23.114 s, System: 3.029 s]
  Range (min … max):    1.412 s …  1.485 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 2: ../../target/release/ty-jemalloc
  Time (mean ± σ):      1.148 s ±  0.038 s    [User: 21.205 s, System: 0.754 s]
  Range (min … max):    1.105 s …  1.227 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: ../../target/release/ty-mimalloc
  Time (mean ± σ):      1.145 s ±  0.032 s    [User: 21.460 s, System: 0.900 s]
  Range (min … max):    1.091 s …  1.185 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  ../../target/release/ty-mimalloc ran
    1.00 ± 0.04 times faster than ../../target/release/ty-jemalloc
    1.26 ± 0.04 times faster than ../../target/release/ty-system
```

</details>

## Windows

mimalloc delivers substantial wins over the Windows system allocator. jemalloc isn't included because it doesn't support Windows.

| Project | mimalloc vs System |
|---------|-------------------|
| black | 1.34× faster |
| discord.py | 1.26× faster |
| homeassistant | *(skipped)* |
| isort | 1.19× faster |
| jinja | 1.24× faster |
| pandas | 1.14× faster |
| pandas-stubs | 1.19× faster |
| prefect | 1.30× faster |
| pytorch | 1.10× faster |

- **Windows system allocator is the worst performer** across all platforms — mimalloc provides 1.10×–1.34× speedups
- **Absolute times are also slower** than Linux/macOS (e.g., pytorch: 2.4s on Windows vs 1.1s on Linux with mimalloc)
- **Gains are largest on smaller projects** (black at 1.34×) and smallest on the biggest codebase (pytorch at 1.10×)


<details>
<summary>Benchmark results</summary>

```
black
-----

Benchmark 1: ..\..\target\release\ty-system.exe
  Time (mean ± σ):     153.8 ms ±   3.1 ms    [User: 584.4 ms, System: 519.3 ms]
  Range (min … max):   148.7 ms … 159.5 ms    18 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ..\..\target\release\ty-mimalloc.exe
  Time (mean ± σ):     114.9 ms ±   3.2 ms    [User: 496.5 ms, System: 372.8 ms]
  Range (min … max):   109.1 ms … 121.0 ms    24 runs

  Warning: Ignoring non-zero exit code.

Summary
  ..\..\target\release\ty-mimalloc.exe ran
    1.34 ± 0.05 times faster than ..\..\target\release\ty-system.exe

-------------------------------------------------------------------------------

discord.py
----------

Benchmark 1: ..\..\target\release\ty-system.exe
  Time (mean ± σ):     540.4 ms ±  13.0 ms    [User: 2702.2 ms, System: 1301.6 ms]
  Range (min … max):   524.5 ms … 569.5 ms    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ..\..\target\release\ty-mimalloc.exe
  Time (mean ± σ):     428.9 ms ±  12.0 ms    [User: 2449.1 ms, System: 787.5 ms]
  Range (min … max):   410.9 ms … 446.6 ms    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ..\..\target\release\ty-mimalloc.exe ran
    1.26 ± 0.05 times faster than ..\..\target\release\ty-system.exe
Skipping homeassistant: Missing dependencies on Windows

-------------------------------------------------------------------------------

isort
-----

Benchmark 1: ..\..\target\release\ty-system.exe
  Time (mean ± σ):     103.1 ms ±   2.7 ms    [User: 295.0 ms, System: 263.2 ms]
  Range (min … max):    97.7 ms … 108.8 ms    26 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ..\..\target\release\ty-mimalloc.exe
  Time (mean ± σ):      86.8 ms ±   2.4 ms    [User: 226.5 ms, System: 204.4 ms]
  Range (min … max):    81.8 ms …  93.0 ms    31 runs

  Warning: Ignoring non-zero exit code.

Summary
  ..\..\target\release\ty-mimalloc.exe ran
    1.19 ± 0.05 times faster than ..\..\target\release\ty-system.exe

-------------------------------------------------------------------------------

jinja
-----

Benchmark 1: ..\..\target\release\ty-system.exe
  Time (mean ± σ):     251.7 ms ±   8.8 ms    [User: 560.1 ms, System: 412.9 ms]
  Range (min … max):   241.4 ms … 270.8 ms    11 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ..\..\target\release\ty-mimalloc.exe
  Time (mean ± σ):     202.6 ms ±   4.9 ms    [User: 415.4 ms, System: 238.1 ms]
  Range (min … max):   193.5 ms … 209.3 ms    14 runs

  Warning: Ignoring non-zero exit code.

Summary
  ..\..\target\release\ty-mimalloc.exe ran
    1.24 ± 0.05 times faster than ..\..\target\release\ty-system.exe

-------------------------------------------------------------------------------

pandas
------

Benchmark 1: ..\..\target\release\ty-system.exe
  Time (mean ± σ):      1.087 s ±  0.047 s    [User: 11.692 s, System: 1.997 s]
  Range (min … max):    1.020 s …  1.185 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ..\..\target\release\ty-mimalloc.exe
  Time (mean ± σ):     951.1 ms ±  45.9 ms    [User: 9839.1 ms, System: 1463.7 ms]
  Range (min … max):   887.3 ms … 1009.7 ms    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ..\..\target\release\ty-mimalloc.exe ran
    1.14 ± 0.07 times faster than ..\..\target\release\ty-system.exe

-------------------------------------------------------------------------------

pandas-stubs
------------

Benchmark 1: ..\..\target\release\ty-system.exe
  Time (mean ± σ):     206.6 ms ±   7.1 ms    [User: 662.8 ms, System: 544.1 ms]
  Range (min … max):   197.7 ms … 222.5 ms    13 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ..\..\target\release\ty-mimalloc.exe
  Time (mean ± σ):     174.2 ms ±   4.1 ms    [User: 551.8 ms, System: 431.1 ms]
  Range (min … max):   168.6 ms … 183.6 ms    16 runs

  Warning: Ignoring non-zero exit code.

Summary
  ..\..\target\release\ty-mimalloc.exe ran
    1.19 ± 0.05 times faster than ..\..\target\release\ty-system.exe

-------------------------------------------------------------------------------

prefect
-------

Benchmark 1: ..\..\target\release\ty-system.exe
  Time (mean ± σ):     254.0 ms ±   6.2 ms    [User: 936.7 ms, System: 855.6 ms]
  Range (min … max):   247.9 ms … 266.6 ms    11 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ..\..\target\release\ty-mimalloc.exe
  Time (mean ± σ):     195.0 ms ±   3.5 ms    [User: 752.3 ms, System: 606.2 ms]
  Range (min … max):   189.7 ms … 201.6 ms    14 runs

  Warning: Ignoring non-zero exit code.

Summary
  ..\..\target\release\ty-mimalloc.exe ran
    1.30 ± 0.04 times faster than ..\..\target\release\ty-system.exe

-------------------------------------------------------------------------------

pytorch
-------

Benchmark 1: ..\..\target\release\ty-system.exe
  Time (mean ± σ):      2.688 s ±  0.039 s    [User: 32.759 s, System: 6.658 s]
  Range (min … max):    2.624 s …  2.756 s    10 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ..\..\target\release\ty-mimalloc.exe
  Time (mean ± σ):      2.445 s ±  0.032 s    [User: 26.743 s, System: 6.101 s]
  Range (min … max):    2.398 s …  2.500 s    10 runs

  Warning: Ignoring non-zero exit code.

Summary
  ..\..\target\release\ty-mimalloc.exe ran
    1.10 ± 0.02 times faster than ..\..\target\release\ty-system.exe
```

</details>

## Summary

| Platform | System Allocator | Best Choice | Typical Gain |
|----------|------------------|-------------|--------------|
| macOS | Decent | mimalloc | 3–13% |
| Linux glibc | Poor | mimalloc or jemalloc | 7–36% |
| Windows | Worst | mimalloc | 10–34% |

Note, this only compares performance, not memory usage

---

_Label `performance` added by @AlexWaygood on 2025-12-14 10:38_

---

_Label `ty` added by @AlexWaygood on 2025-12-14 10:38_

---

_Comment by @MichaReiser on 2025-12-14 12:48_

## Cold peak memory usage

I used homeassistant on mac and linux

### Mac OS

Allocator | Avg Peak Memory | vs System
-- | -- | --
System | 5.37 GB | baseline
jemalloc | 5.23 GB | 2.7% less
mimalloc | 5.32 GB | 1.0% less

<details>
<summary>Results</summary>

peak memory footprint as reported by `/usr/bin/time -l` 

**System**:

* 5390769496  peak memory footprint
* 5347024192  peak memory footprint
* 5365554544  peak memory footprint
* 5374287168  peak memory footprint
* 5370830168  peak memory footprint

**Jemalloc**

* 5198926456  peak memory footprint
* 5215015568  peak memory footprint
* 5253501608  peak memory footprint
* 5244605096  peak memory footprint
* 5228794536  peak memory footprint

**mimalloc**

* 5314991264  peak memory footprint
* 5304636600  peak memory footprint
* 5281731696  peak memory footprint
* 5327590512  peak memory footprint
* 5353788600  peak memory footprint

</details>

## Linux

Allocator | Avg Peak Memory | vs System
-- | -- | --
System (glibc) | 5.03 GB | baseline
jemalloc | 5.22 GB | 3.8% more
mimalloc | 5.69 GB | 13.1% more


<details>
<summary>Results</summary>

`/usr/bin/time -v`, 	

System:

* Maximum resident set size (kbytes): 5153244
* Maximum resident set size (kbytes): 5149456
* Maximum resident set size (kbytes): 5151964
* Maximum resident set size (kbytes): 5136340
* Maximum resident set size (kbytes): 5140888

Jemalloc:

* Maximum resident set size (kbytes): 5344916
* Maximum resident set size (kbytes): 5385424
* Maximum resident set size (kbytes): 5308988
* Maximum resident set size (kbytes): 5364632
* Maximum resident set size (kbytes): 5314000

Mimalloc:

* Maximum resident set size (kbytes): 5811044
* Maximum resident set size (kbytes): 5842680
* Maximum resident set size (kbytes): 5830728
* Maximum resident set size (kbytes): 5831760
* Maximum resident set size (kbytes): 5817932
</details>

## Windows

### Pandas

| Allocator | Avg Peak Memory | vs System |
|-----------|-----------------|-----------|
| System | 1.83 GB | baseline |
| mimalloc | 1.89 GB | **3.4% more** |

### Prefect

| Allocator | Avg Peak Memory | vs System |
|-----------|-----------------|-----------|
| System | 345 MB | baseline |
| mimalloc | 371 MB | **7.5% more** |


<details>
<summary>Results</summary>

Pandas, System

* Peak: 1828.02 MB
* Peak: 1831.66 MB
* Peak: 1826.82 MB
* Peak: 1830.15 MB
* Peak: 1826.82 MB


Pandas, mimalloc 

* Peak: 1889.79 MB
* Peak: 1881.47 MB
* Peak: 1891.35 MB
* Peak: 1883.98 MB
* Peak: 1885.88 MB
* Peak: 1900.97 MB


Prefect, System

* Peak: 344.98 MB
* Peak: 345.59 MB
* Peak: 344.34 MB
* Peak: 345.39 MB
* Peak: 345.02 MB
* Peak: 345.8 MB

Prefect, mimalloc
* Peak: 370.75 MB
* Peak: 367.51 MB
* Peak: 370.64 MB
* Peak: 375.81 MB
* Peak: 371.82 MB
* Peak: 369.55 MB
* Peak: 370.01 MB

Script

```ps1
$proc = Start-Process "..\ruff\target\release\ty-system.exe" -ArgumentList "check -q pandas typings" -PassThru -NoNewWindow
while (!$proc.HasExited) { $proc.Refresh(); Start-Sleep -Milliseconds 10 }
$proc.Refresh()
Write-Host "Peak: $([math]::Round($proc.PeakWorkingSet64 / 1MB, 2)) MB"
```

</details>

---

_Comment by @MichaReiser on 2025-12-14 14:10_

Incremental memory usage is much more difficult to capture and I found results to change a lot between (manual) runs. 

It's also difficult to know at which number to use in the first place. Virtual memory, resident memory? How long to wait for decay to kick in, etc.

## Macos

System:

* after start: 96MB
* after goto: 1.57gb
* after completions: 2.09gb
* stays at 2.10gb

Jemalloc:

* after start: 114mb
* after goto: 1.62gb
* after completions: 2.13gb
* jumps up to 2.5gb, goes back to 2.34gb


mimalloc:

* after start: 110mb
* after goto: 1.56gb
* after completions: 1.89gb
* stays at roughly 1.93

## Linux

Jemalloc does pretty well overall, even using less than the system allocator?

System
* start: 70MB,
* after completion, typing, 3.4 GB
* Didn't go back again?

jemalloc:
* 260mb after start
* 1.8 GB after symbol search
* After a while, back to 1.4gb

mimalloc:
* start: 172MB
* 1.9gb after symbol search
* 2.9gb after completion

## Windows

System, prefect:

* 54MB after start
* 358mb after go to 
* 465 mb after comletions
* Down to 400mb after a while

mimalloc:

* 58MB after start
* 340mb after goto 
* 600mb after completions
* up to 690mb
* doesn't go down again.


---

_Renamed from "[ty] Use custom allocator" to "[ty] Use jemalloc on linux" by @MichaReiser on 2025-12-14 14:44_

---

_Marked ready for review by @MichaReiser on 2025-12-14 15:14_

---

_Review requested from @carljm by @MichaReiser on 2025-12-14 15:14_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-12-14 15:14_

---

_Review requested from @sharkdp by @MichaReiser on 2025-12-14 15:14_

---

_Review requested from @dcreager by @MichaReiser on 2025-12-14 15:14_

---

_Comment by @AlexWaygood on 2025-12-14 16:38_

This looks awesome. Thanks for spending so much time on the analysis!! I'll leave it to others to review the code, however; I feel very underqualified there 😆

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-14 16:39_

---

_@charliermarsh approved on 2025-12-14 18:27_

---

_Review comment by @sharkdp on `crates/ty/src/main.rs`:19 on 2025-12-14 19:25_

We do [something very similar in fd](https://github.com/sharkdp/fd/blob/f5b4171631c7615435b3e2275fff768c99e3c4f3/src/main.rs#L37-L53), and had to add more and more exclusions over the years. Might be worth comparing the lists(?).

Unrelated, it might be good to add a comment here and in `Cargo.toml` that the filtering rules need to be kept in sync? We don't test on all of these platforms..

---

_@sharkdp reviewed on 2025-12-14 19:25_

---

_Review comment by @charliermarsh on `crates/ty/src/main.rs`:19 on 2025-12-14 20:08_

For reference, I believe this is the same as in Ruff (except you're also omitting macOS (intentionally)). In uv, it looks like we use:
```rust
#[cfg(all(
    not(target_os = "windows"),
    not(target_os = "openbsd"),
    not(target_os = "freebsd"),
    any(
        target_arch = "x86_64",
        target_arch = "aarch64",
        target_arch = "powerpc64"
    )
))]
#[global_allocator]
static GLOBAL: tikv_jemallocator::Jemalloc = tikv_jemallocator::Jemalloc;
```

---

_@charliermarsh reviewed on 2025-12-14 20:08_

---

_@MichaReiser reviewed on 2025-12-14 21:54_

---

_Review comment by @MichaReiser on `crates/ty/src/main.rs`:19 on 2025-12-14 21:54_

Yes, it's the same as Ruff, but excluding macos for the reasons described in the PR summary

---

_Comment by @zanieb on 2025-12-15 14:41_

Just as a note, we split this into a separate crate in uv a while back to improve developer compile times (https://github.com/astral-sh/uv/tree/main/crates/uv-performance-memory-allocator / https://github.com/astral-sh/uv/pull/7686)

---

_@BurntSushi approved on 2025-12-15 14:53_

---

_Merged by @MichaReiser on 2025-12-15 15:04_

---

_Closed by @MichaReiser on 2025-12-15 15:04_

---

_Branch deleted on 2025-12-15 15:04_

---

_Comment by @dsully on 2025-12-15 16:24_

FWIW, I've seen substantial RSS usage on macOS (5 GB+) for one of my repositories. Use `jemalloc` as the allocator would allow me to set the decay time and areanas via an environment variable similar to the issues mentioned here: 

https://github.com/pola-rs/polars/issues/23128

---
