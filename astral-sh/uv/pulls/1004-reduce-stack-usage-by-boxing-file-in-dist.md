```yaml
number: 1004
title: "Reduce stack usage by boxing `File` in `Dist`, `CachePolicy` and large futures"
type: pull_request
state: merged
author: konstin
labels:
  - windows
assignees: []
merged: true
base: main
head: konsti/reduce-stack-usage
created_at: 2024-01-19T09:36:41Z
updated_at: 2024-01-19T09:38:37Z
url: https://github.com/astral-sh/uv/pull/1004
synced_at: 2026-01-12T16:04:21Z
```

# Reduce stack usage by boxing `File` in `Dist`, `CachePolicy` and large futures

---

_@konstin_

This is https://github.com/astral-sh/puffin/pull/947 again but this time merging into main instead of downstack, sorry for the noise.

---

Windows has a default stack size of 1MB, which makes puffin often fail with stack overflows. The PR reduces stack size by three changes:

* Boxing `File` in `Dist`, reducing the size from 496 to 240.
* Boxing the largest futures.
* Boxing `CachePolicy`

## Method

Debugging happened on linux using https://github.com/astral-sh/puffin/pull/941 to limit the stack size to 1MB. Used ran the command below.

```
RUSTFLAGS=-Zprint-type-sizes cargo +nightly build -p puffin-cli -j 1 > type-sizes.txt && top-type-sizes -w -s -h 10 < type-sizes.txt > sizes.txt
```

The main drawback is top-type-sizes not saying what the `__awaitee` is, so it requires manually looking up with a future with matching size.

When the `brotli` features on `reqwest` is active, a lot of brotli types show up. Toggling this feature however seems to have no effect. I assume they are false positives since the `brotli` crate has elaborate control about allocation. The sizes are therefore shown with the feature off.

## Results

The largest future goes from 12208B to 6416B, the largest type (`PrioritizedDistribution`, see also #948) from 17448B to 9264B. Full diff: https://gist.github.com/konstin/62635c0d12110a616a1b2bfcde21304f

For the second commit, i iteratively boxed the largest file until the tests passed, then with an 800KB stack limit looked through the backtrace of a failing test and added some more boxing.

Quick benchmarking showed no difference:

```console
$ hyperfine --warmup 2 "target/profiling/main-dev resolve meine_stadt_transparent" "target/profiling/puffin-dev resolve meine_stadt_transparent" 
Benchmark 1: target/profiling/main-dev resolve meine_stadt_transparent
  Time (mean ± σ):      49.2 ms ±   3.0 ms    [User: 39.8 ms, System: 24.0 ms]
  Range (min … max):    46.6 ms …  63.0 ms    55 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Benchmark 2: target/profiling/puffin-dev resolve meine_stadt_transparent
  Time (mean ± σ):      47.4 ms ±   3.2 ms    [User: 41.3 ms, System: 20.6 ms]
  Range (min … max):    44.6 ms …  60.5 ms    62 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Summary
  target/profiling/puffin-dev resolve meine_stadt_transparent ran
    1.04 ± 0.09 times faster than target/profiling/main-dev resolve meine_stadt_transparent
```

---

_Label `windows` added by @konstin on 2024-01-19 09:37_

---

_Merged by @konstin on 2024-01-19 09:38_

---

_Closed by @konstin on 2024-01-19 09:38_

---

_Branch deleted on 2024-01-19 09:38_

---
