```yaml
number: 3270
title: Use mimalloc on MacOS and Linux
type: pull_request
state: closed
author: ibraheemdev
labels:
  - performance
assignees: []
base: main
head: mimalloc
created_at: 2024-04-25T20:30:02Z
updated_at: 2024-05-01T14:37:44Z
url: https://github.com/astral-sh/uv/pull/3270
synced_at: 2026-01-12T16:05:32Z
```

# Use mimalloc on MacOS and Linux

---

_@ibraheemdev_

## Summary

`mimalloc` tends to provide performance benefits, especially in multi-threaded scenarios, and even more when using work-stealing schedulers where data is migrated across threads between allocation and de-allocation. Currently, we only use `mimalloc` on Windows, but we can also use it on MacOS and Linux (`mimalloc` supports more platforms than this, but those seem to be the only officially supported ones https://github.com/microsoft/mimalloc/issues/482#issuecomment-953190795).

## Test Plan

On Linux, I'm seeing between 15-30% performance improvements on the `install-warm` and `resolve-warm` benchmarks. On M1 I'm seeing a slight regression, so it might only be beneficial to switch on Linux, but I want to see the results on other machines first.

We should also test the memory usage affects of this change.

---

_Label `performance` added by @zanieb on 2024-04-25 23:37_

---

_Comment by @charliermarsh on 2024-04-26 04:03_

I did some benchmarking on my M3. Hard to draw firm conclusions but I think I see something like a 5-10% regression in `install-warm` for `./scripts/requirements/compiled/jupyter.txt` (replicated in three runs with `--min-runs 50 --warmup 20`), although in one run, this branch was 5% faster. I'd probably err on the side of _not_ changing the allocator on macOS based on that, though.

---

_Comment by @konstin on 2024-04-26 09:15_

Resolutions on ubuntu 22.04 on a 16 core / 32 thread i9-13900K:

jupyter:
```
Benchmark 1: target/profiling/uv-main pip compile scripts/requirements/jupyter.in
  Time (mean ± σ):      14.4 ms ±   0.8 ms    [User: 15.0 ms, System: 15.9 ms]
  Range (min … max):    12.8 ms …  17.4 ms    200 runs
 
Benchmark 2: target/profiling/uv-branch pip compile scripts/requirements/jupyter.in
  Time (mean ± σ):      15.2 ms ±   0.9 ms    [User: 14.1 ms, System: 20.0 ms]
  Range (min … max):    13.1 ms …  18.6 ms    211 runs
 
Summary
  target/profiling/uv-main pip compile scripts/requirements/jupyter.in ran
    1.05 ± 0.09 times faster than target/profiling/uv-branch pip compile scripts/requirements/jupyter.in
```

The jupyter install benchmark is only noise for me unfortunately.

airflow:
```
Benchmark 1: target/profiling/uv-main pip compile airflow.in
  Time (mean ± σ):     154.9 ms ±   2.7 ms    [User: 148.1 ms, System: 128.2 ms]
  Range (min … max):   149.7 ms … 160.0 ms    19 runs
 
Benchmark 2: target/profiling/uv-branch pip compile airflow.in
  Time (mean ± σ):     150.6 ms ±   3.5 ms    [User: 146.3 ms, System: 133.6 ms]
  Range (min … max):   146.4 ms … 158.3 ms    20 runs
 
Summary
  target/profiling/uv-branch pip compile airflow.in ran
    1.03 ± 0.03 times faster than target/profiling/uv-main pip compile airflow.in
```

boto3:
```
Benchmark 1: target/profiling/uv-main pip compile scripts/requirements/boto3.in
  Time (mean ± σ):     356.3 ms ±   0.9 ms    [User: 322.2 ms, System: 45.2 ms]
  Range (min … max):   355.2 ms … 357.6 ms    10 runs
 
Benchmark 2: target/profiling/uv-branch pip compile scripts/requirements/boto3.in
  Time (mean ± σ):     355.1 ms ±   1.7 ms    [User: 323.1 ms, System: 44.0 ms]
  Range (min … max):   352.0 ms … 358.0 ms    10 runs
 
Summary
  target/profiling/uv-branch pip compile scripts/requirements/boto3.in ran
    1.00 ± 0.01 times faster than target/profiling/uv-main pip compile scripts/requirements/boto3.in
```

I don't mind the 0.8ms (that's ms, not s) more for jupyter since it doesn't affect more complex resolutions.

I'm all for merging this just for removing the tikv-jemallocator dependency and the simpler cfg's.

---

_@konstin approved on 2024-04-26 09:15_

---

_Comment by @zanieb on 2024-04-26 14:32_

## macOS (M1 Pro)
 
```
❯ python -m scripts.bench \
    --uv-path ./baseline \
    --uv-path ./target/release/uv \
    ../prefect/requirements.txt
Benchmark 1: ./baseline (install-cold)
  Time (mean ± σ):      2.074 s ±  0.545 s    [User: 0.484 s, System: 0.856 s]
  Range (min … max):    1.381 s …  2.849 s    10 runs
 
Benchmark 2: ./target/release/uv (install-cold)
  Time (mean ± σ):      1.960 s ±  0.454 s    [User: 0.457 s, System: 0.842 s]
  Range (min … max):    1.664 s …  3.203 s    10 runs
 
Summary
  ./target/release/uv (install-cold) ran
    1.06 ± 0.37 times faster than ./baseline (install-cold)

Benchmark 1: ./baseline (install-warm)
  Time (mean ± σ):      66.5 ms ±  12.3 ms    [User: 11.1 ms, System: 99.8 ms]
  Range (min … max):    53.2 ms …  88.1 ms    10 runs
 
Benchmark 2: ./target/release/uv (install-warm)
  Time (mean ± σ):      72.0 ms ±  14.1 ms    [User: 10.3 ms, System: 99.9 ms]
  Range (min … max):    53.3 ms …  92.1 ms    10 runs
 
Summary
  ./baseline (install-warm) ran
    1.08 ± 0.29 times faster than ./target/release/uv (install-warm)
```

```
❯ python -m scripts.bench \
    --uv-path ./baseline \
    --uv-path ./target/release/uv \
  ./scripts/requirements/compiled/jupyter.txt
Benchmark 1: ./baseline (install-cold)
  Time (mean ± σ):      3.514 s ±  0.931 s    [User: 0.864 s, System: 1.726 s]
  Range (min … max):    2.228 s …  5.212 s    10 runs
 
Benchmark 2: ./target/release/uv (install-cold)
  Time (mean ± σ):      3.544 s ±  1.190 s    [User: 0.882 s, System: 1.725 s]
  Range (min … max):    2.321 s …  5.626 s    10 runs
 
Summary
  ./baseline (install-cold) ran
    1.01 ± 0.43 times faster than ./target/release/uv (install-cold)

Benchmark 1: ./baseline (install-warm)
  Time (mean ± σ):     213.7 ms ±  19.1 ms    [User: 29.0 ms, System: 226.1 ms]
  Range (min … max):   177.2 ms … 238.9 ms    10 runs
 
Benchmark 2: ./target/release/uv (install-warm)
  Time (mean ± σ):     206.3 ms ±  23.8 ms    [User: 28.4 ms, System: 224.4 ms]
  Range (min … max):   177.7 ms … 257.9 ms    10 runs
 
Summary
  ./target/release/uv (install-warm) ran
    1.04 ± 0.15 times faster than ./baseline (install-warm)
```

```
❯ python -m scripts.bench \
    --uv-path ./baseline \
    --uv-path ./target/release/uv \
  ./scripts/requirements/boto3.in
Benchmark 1: ./baseline (resolve-cold)
  Time (mean ± σ):      2.883 s ±  0.176 s    [User: 0.921 s, System: 0.819 s]
  Range (min … max):    2.645 s …  3.159 s    10 runs
 
Benchmark 2: ./target/release/uv (resolve-cold)
  Time (mean ± σ):      2.906 s ±  0.571 s    [User: 0.875 s, System: 0.770 s]
  Range (min … max):    2.116 s …  3.903 s    10 runs
 
Summary
  ./baseline (resolve-cold) ran
    1.01 ± 0.21 times faster than ./target/release/uv (resolve-cold)

Benchmark 1: ./baseline (resolve-warm)
  Time (mean ± σ):     482.8 ms ±  44.0 ms    [User: 429.6 ms, System: 123.7 ms]
  Range (min … max):   437.9 ms … 560.1 ms    10 runs
 
Benchmark 2: ./target/release/uv (resolve-warm)
  Time (mean ± σ):     516.9 ms ± 104.1 ms    [User: 422.6 ms, System: 127.4 ms]
  Range (min … max):   420.3 ms … 721.9 ms    10 runs
 
Summary
  ./baseline (resolve-warm) ran
    1.07 ± 0.24 times faster than ./target/release/uv (resolve-warm)
```

## Linux (AMD Ryzen 7 3800X)

```
❯ python3 -m scripts.bench \
    --uv-path ./baseline \
    --uv-path ./target/release/uv \
  ./scripts/requirements/compiled/jupyter.txt
Benchmark 1: ./baseline (install-cold)
  Time (mean ± σ):      3.976 s ±  0.960 s    [User: 0.951 s, System: 0.669 s]
  Range (min … max):    2.351 s …  5.159 s    10 runs
 
Benchmark 2: ./target/release/uv (install-cold)
  Time (mean ± σ):      3.926 s ±  0.955 s    [User: 0.925 s, System: 0.695 s]
  Range (min … max):    2.997 s …  6.253 s    10 runs
 
Summary
  ./target/release/uv (install-cold) ran
    1.01 ± 0.35 times faster than ./baseline (install-cold)

Benchmark 1: ./baseline (install-warm)
  Time (mean ± σ):      82.4 ms ±  19.2 ms    [User: 46.1 ms, System: 101.0 ms]
  Range (min … max):    53.6 ms … 116.9 ms    10 runs
 
Benchmark 2: ./target/release/uv (install-warm)
  Time (mean ± σ):      81.4 ms ±  13.9 ms    [User: 45.3 ms, System: 107.6 ms]
  Range (min … max):    62.5 ms … 101.3 ms    10 runs
 
Summary
  ./target/release/uv (install-warm) ran
    1.01 ± 0.29 times faster than ./baseline (install-warm)
```

```
❯ python3 -m scripts.bench \
    --uv-path ./baseline \
    --uv-path ./target/release/uv \
  ./scripts/requirements/boto3.in
Benchmark 1: ./baseline (resolve-cold)
  Time (mean ± σ):      2.542 s ±  0.333 s    [User: 0.943 s, System: 0.286 s]
  Range (min … max):    2.094 s …  3.206 s    10 runs
 
Benchmark 2: ./target/release/uv (resolve-cold)
  Time (mean ± σ):      2.666 s ±  0.276 s    [User: 0.933 s, System: 0.297 s]
  Range (min … max):    2.332 s …  3.226 s    10 runs
 
Summary
  ./baseline (resolve-cold) ran
    1.05 ± 0.18 times faster than ./target/release/uv (resolve-cold)

Benchmark 1: ./baseline (resolve-warm)
  Time (mean ± σ):     493.1 ms ±   4.9 ms    [User: 471.1 ms, System: 60.3 ms]
  Range (min … max):   483.7 ms … 500.1 ms    10 runs
 
Benchmark 2: ./target/release/uv (resolve-warm)
  Time (mean ± σ):     484.7 ms ±   4.4 ms    [User: 467.2 ms, System: 56.6 ms]
  Range (min … max):   478.1 ms … 492.2 ms    10 runs
 
Summary
  ./target/release/uv (resolve-warm) ran
    1.02 ± 0.01 times faster than ./baseline (resolve-warm)
```

```
❯ python3 -m scripts.bench \
    --uv-path ./baseline \
    --uv-path ./target/release/uv \
  ./scripts/requirements/trio.in --benchmark resolve-warm --benchmark install-warm --min-runs 100
Benchmark 1: ./baseline (resolve-warm)
  Time (mean ± σ):      17.9 ms ±   0.6 ms    [User: 8.8 ms, System: 19.4 ms]
  Range (min … max):    15.7 ms …  19.5 ms    132 runs
 
Benchmark 2: ./target/release/uv (resolve-warm)
  Time (mean ± σ):      17.1 ms ±   0.4 ms    [User: 11.2 ms, System: 25.9 ms]
  Range (min … max):    16.1 ms …  18.1 ms    139 runs
 
Summary
  ./target/release/uv (resolve-warm) ran
    1.05 ± 0.04 times faster than ./baseline (resolve-warm)

Benchmark 1: ./baseline (install-warm)
  Time (mean ± σ):      19.5 ms ±   2.5 ms    [User: 6.6 ms, System: 27.3 ms]
  Range (min … max):    17.2 ms …  41.0 ms    100 runs
 
  Warning: Statistical outliers were detected.

Benchmark 2: ./target/release/uv (install-warm)
  Time (mean ± σ):      16.4 ms ±   0.6 ms    [User: 7.5 ms, System: 29.9 ms]
  Range (min … max):    15.8 ms …  18.7 ms    100 runs
 
Summary
  ./target/release/uv (install-warm) ran
    1.19 ± 0.16 times faster than ./baseline (install-warm)
```

---

_Comment by @zanieb on 2024-04-26 14:35_

Not really seeing anything definitive, I think I'd need to run more samples if we want more data for a decision.

---

_Comment by @charliermarsh on 2024-04-26 17:22_

I think the warm-resolve benchmarks are so fast that there won't be much signal in them. And the cold benchmarks are so bounded by other things that it's hard to get much signal from them either. So I'd suggest we focus on the warm-install benchmarks, I guess?

---

_Comment by @codspeed-hq[bot] on 2024-04-30 19:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:mimalloc)

### Merging #3270 will **degrade performances by 11.59%**

<sub>Comparing <code>ibraheemdev:mimalloc</code> (2b5fefc) with <code>main</code> (630d3fd)</sub>



### Summary

`❌ 1` regressions
`✅ 11` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:mimalloc)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ibraheemdev:mimalloc` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_jupyter` | 367.5 ms | 415.7 ms | -11.59% |


---

_Comment by @ibraheemdev on 2024-04-30 19:55_

I'm going to close this for now, I'm also seeing the regression locally with the new resolver benchmarks, so it's not just CodSpeed.

---

_Closed by @ibraheemdev on 2024-04-30 19:55_

---

_Comment by @zanieb on 2024-04-30 20:22_

@ibraheemdev just be warned, I think allocations are "free" and not part of the measurements in CodSpeed.

cc @MichaReiser

---

_Comment by @charliermarsh on 2024-04-30 20:24_

I’m generally fine to merge this for Linux at least if we see some improvements in the warm install.

---

_Comment by @zanieb on 2024-04-30 20:26_

Yeah the 20% speed-up on Linux seemed nice.

---

_Comment by @ibraheemdev on 2024-04-30 20:49_

@zanieb Can you run `cargo bench --bench uv` locally? I'm seeing a large regression on that benchmark on my Linux machine.

---

_Comment by @zanieb on 2024-05-01 13:27_

I don't see a large regression on my Linux machine.

```
❯ cargo bench --bench uv
resolve_warm_jupyter    time:   [12.663 ms 12.739 ms 12.816 ms]
                        change: [+1.2044% +1.9636% +2.6014%] (p = 0.00 < 0.05)
                        Performance has regressed.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild

❯ cargo bench --bench uv
resolve_warm_jupyter    time:   [12.460 ms 12.516 ms 12.571 ms]
                        change: [-2.4970% -1.7496% -1.0174%] (p = 0.00 < 0.05)
                        Performance has improved.
```

---

_Comment by @MichaReiser on 2024-05-01 13:53_

> @ibraheemdev just be warned, I think allocations are "free" and not part of the measurements in CodSpeed.

Yes and no. The `alloc` syscall is free, but not the `alloc` of a custom memory allocator. CodSpeed will measure the instructions in the custom allocator the same as our user code. In general, all syscalls are free (which I think should be fixed because they're the most expensive calls).

That means, codspeed will not measure if this PR e.g. reduces the calls to the system `alloc` (or `free` to that matter) function. But it still measures how many other non-syscall instructions our code runs.


**Linux (AMD Ryzen 9 5900X 12-Core Processor)** 

```
python3 -m scripts.bench \
          --uv-path ./target/release/puffin-main \
          --uv-path ./target/release/puffin \
        ./scripts/requirements/trio.in --benchmark resolve-warm --benchmark install-warm --min-runs 100
Benchmark 1: ./target/release/puffin-main (resolve-warm)
  Time (mean ± σ):      21.0 ms ±   1.1 ms    [User: 13.1 ms, System: 20.7 ms]
  Range (min … max):    19.1 ms …  26.0 ms    107 runs
 
Benchmark 2: ./target/release/puffin (resolve-warm)
  Time (mean ± σ):      20.7 ms ±   0.9 ms    [User: 13.5 ms, System: 19.8 ms]
  Range (min … max):    18.9 ms …  22.9 ms    118 runs
 
Summary
  ./target/release/puffin (resolve-warm) ran
    1.02 ± 0.07 times faster than ./target/release/puffin-main (resolve-warm)
Benchmark 1: ./target/release/puffin-main (install-warm)
  Time (mean ± σ):      20.7 ms ±   1.0 ms    [User: 9.8 ms, System: 26.7 ms]
  Range (min … max):    18.9 ms …  24.4 ms    100 runs
 
Benchmark 2: ./target/release/puffin (install-warm)
  Time (mean ± σ):      20.6 ms ±   1.0 ms    [User: 9.8 ms, System: 26.6 ms]
  Range (min … max):    18.7 ms …  24.5 ms    100 runs
 
Summary
  ./target/release/puffin (install-warm) ran
    1.00 ± 0.07 times faster than ./target/release/puffin-main (install-warm)

```

---
