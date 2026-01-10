```yaml
number: 4934
title: Switch to Current-Thread Tokio Runtime
type: pull_request
state: merged
author: ibraheemdev
labels:
  - performance
assignees: []
merged: true
base: main
head: ibraheem/current-thread
created_at: 2024-07-09T18:39:03Z
updated_at: 2024-07-09T22:21:18Z
url: https://github.com/astral-sh/uv/pull/4934
synced_at: 2026-01-10T13:42:52Z
```

# Switch to Current-Thread Tokio Runtime

---

_Pull request opened by @ibraheemdev on 2024-07-09 18:39_

## Summary

Move completely off tokio's multi-threaded runtime. We've slowly been making changes to be smarter about scheduling in various places instead of depending on tokio's general purpose work-stealing, notably https://github.com/astral-sh/uv/pull/3627 and https://github.com/astral-sh/uv/pull/4004. We now no longer benefit from the multi-threaded runtime, as we run on all I/O on the main thread. There's one remaining instance of `block_in_place` that can be swapped for `rayon::spawn`.

This change is a small performance improvement due to removing some unnecessary overhead of the multi-threaded runtime (e.g. spawning threads), but nothing major. It also removes some noise from profiles.

## Test Plan

```
Benchmark 1: ./target/profiling/uv (resolve-warm)
  Time (mean ± σ):      14.9 ms ±   0.3 ms    [User: 3.0 ms, System: 17.3 ms]
  Range (min … max):    14.1 ms …  15.8 ms    169 runs
 
Benchmark 2: ./target/profiling/baseline (resolve-warm)
  Time (mean ± σ):      16.1 ms ±   0.3 ms    [User: 3.9 ms, System: 18.7 ms]
  Range (min … max):    15.1 ms …  17.3 ms    162 runs
 
Summary
  ./target/profiling/uv (resolve-warm) ran
    1.08 ± 0.03 times faster than ./target/profiling/baseline (resolve-warm)
```

---

_Label `performance` added by @ibraheemdev on 2024-07-09 18:39_

---

_@charliermarsh approved on 2024-07-09 21:02_

Trusting you on this one :)

---

_Merged by @ibraheemdev on 2024-07-09 22:21_

---

_Closed by @ibraheemdev on 2024-07-09 22:21_

---

_Branch deleted on 2024-07-09 22:21_

---
