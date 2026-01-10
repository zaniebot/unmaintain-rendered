```yaml
number: 4004
title: Avoid work-stealing in bytecode compilation
type: pull_request
state: merged
author: ibraheemdev
labels:
  - performance
assignees: []
merged: true
base: main
head: compile-scheduler
created_at: 2024-06-04T00:11:02Z
updated_at: 2024-06-04T14:48:24Z
url: https://github.com/astral-sh/uv/pull/4004
synced_at: 2026-01-10T13:54:02Z
```

# Avoid work-stealing in bytecode compilation

---

_Pull request opened by @ibraheemdev on 2024-06-04 00:11_

## Summary

Avoid using work-stealing Tokio workers for bytecode compilation, favoring instead dedicated threads. Tokio's work-stealing does not really benefit us because we're spawning Python workers and scheduling tasks ourselves — we don't want Tokio to re-balance our workers. Because we're doing scheduling ourselves and compilation is a primarily compute-bound task, we can also create dedicated runtimes for each worker and avoid some synchronization overhead.

This is part of a general desire to avoid relying on Tokio's work-stealing scheduler and be smarter about our workload. In this case we already had the custom scheduler in place, Tokio was just getting in the way (though the overhead is very minor).

## Test Plan

This improves performance by ~5% on my machine.

```
$ hyperfine --warmup 1 --prepare "target/profiling/uv-dev clear-compile .venv" "target/profiling/uv-dev compile .venv" "target/profiling/uv-dev-dedicated compile .venv"
Benchmark 1: target/profiling/uv-dev compile .venv
  Time (mean ± σ):      1.279 s ±  0.011 s    [User: 13.803 s, System: 2.998 s]
  Range (min … max):    1.261 s …  1.296 s    10 runs
 
Benchmark 2: target/profiling/uv-dev-dedicated compile .venv
  Time (mean ± σ):      1.220 s ±  0.021 s    [User: 13.997 s, System: 3.330 s]
  Range (min … max):    1.198 s …  1.272 s    10 runs

Summary
  target/profiling/uv-dev-dedicated compile .venv ran
    1.05 ± 0.02 times faster than target/profiling/uv-dev compile .venv

$ hyperfine --warmup 1 --prepare "target/profiling/uv-dev clear-compile .venv" "target/profiling/uv-dev compile .venv" "target/profiling/uv-dev-dedicated compile .venv"
Benchmark 1: target/profiling/uv-dev compile .venv
  Time (mean ± σ):      3.631 s ±  0.078 s    [User: 47.205 s, System: 4.996 s]
  Range (min … max):    3.564 s …  3.832 s    10 runs
 
Benchmark 2: target/profiling/uv-dev-dedicated compile .venv
  Time (mean ± σ):      3.521 s ±  0.024 s    [User: 48.201 s, System: 5.392 s]
  Range (min … max):    3.484 s …  3.566 s    10 runs
 
Summary
  target/profiling/uv-dev-dedicated compile .venv ran
    1.03 ± 0.02 times faster than target/profiling/uv-dev compile .venv
```


---

_Review requested from @konstin by @ibraheemdev on 2024-06-04 00:11_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-06-04 00:11_

---

_@charliermarsh approved on 2024-06-04 00:30_

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:108 on 2024-06-04 08:49_

Does it still makes sense for the worker to be async? We're not really using any async-specific features there except for timeouts.

---

_Review comment by @konstin on `crates/uv-installer/src/compile.rs`:114 on 2024-06-04 08:49_

Nit: Can we give this a message with `.expect()`?


---

_@konstin approved on 2024-06-04 08:50_

Cool find, i didn't realize we were paying a premium for work-stealing!

---

_Label `performance` added by @konstin on 2024-06-04 08:50_

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:103 on 2024-06-04 10:50_

I'm probably missing something silly here, but why the `catch_unwind`? You have a thread boundary here, so you should be able to just extract the panic from the result of joining the thread?

---

_@BurntSushi approved on 2024-06-04 10:50_

---

_@ibraheemdev reviewed on 2024-06-04 14:32_

---

_Review comment by @ibraheemdev on `crates/uv-installer/src/compile.rs`:103 on 2024-06-04 14:32_

We wait on the threads asynchronously, which is why we use the oneshot channel instead of blocking on `join`.

---

_@ibraheemdev reviewed on 2024-06-04 14:33_

---

_Review comment by @ibraheemdev on `crates/uv-installer/src/compile.rs`:108 on 2024-06-04 14:33_

I looked into making them non-async very briefly, but getting the timeouts to work with synchronous I/O is not trivial so I left it for now. I don't think it's a huge deal because most of the work is run on a separate process anyways, but we could probably get some minor gains from stripping out the async.

---

_@BurntSushi reviewed on 2024-06-04 14:39_

---

_Review comment by @BurntSushi on `crates/uv-installer/src/compile.rs`:103 on 2024-06-04 14:39_

Hmmm okay, I think that makes sense to me. Thanks!

---

_Merged by @ibraheemdev on 2024-06-04 14:48_

---

_Closed by @ibraheemdev on 2024-06-04 14:48_

---
