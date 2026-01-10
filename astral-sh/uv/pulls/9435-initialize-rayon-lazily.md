```yaml
number: 9435
title: Initialize rayon lazily
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/lazy-rayon
created_at: 2024-11-26T09:03:04Z
updated_at: 2024-11-26T15:34:17Z
url: https://github.com/astral-sh/uv/pull/9435
synced_at: 2026-01-10T12:00:00Z
```

# Initialize rayon lazily

---

_Pull request opened by @konstin on 2024-11-26 09:03_

When performing a noop sync, we don't need the rayon threadpool, yet we pay for its initialization:

![Screenshot from 2024-11-26 08-59-07](https://github.com/user-attachments/assets/d918f50d-b5b7-4bdd-820d-cbe71b633aaa)

Be making the initialization lazy, we avoid that cost:

![Screenshot from 2024-11-26 09-53-08](https://github.com/user-attachments/assets/193baea0-667f-4b9d-9a75-886a86f0f837)

This code runs every time before user code in `uv run`.

This means that before calling rayon, one now needs to call `LazyLock::force(&RAYON_INITIALIZE);`.

Performance mode (CPU 0 is a perf core):
```
$ taskset -c 0 hyperfine --warmup 5 -N "/home/konsti/projects/uv/uv-main sync" "/home/konsti/projects/uv/target/profiling/uv sync"
Benchmark 1: /home/konsti/projects/uv/uv-main sync
  Time (mean ± σ):       4.5 ms ±   0.1 ms    [User: 2.7 ms, System: 1.8 ms]
  Range (min … max):     4.4 ms …   6.4 ms    640 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Benchmark 2: /home/konsti/projects/uv/target/profiling/uv sync
  Time (mean ± σ):       4.4 ms ±   0.1 ms    [User: 2.7 ms, System: 1.6 ms]
  Range (min … max):     4.3 ms …   5.0 ms    679 runs
 
Summary
  /home/konsti/projects/uv/target/profiling/uv sync ran
    1.03 ± 0.04 times faster than /home/konsti/projects/uv/uv-main sync
```

Power saver mode:
```
$ hyperfine --warmup 5 -N "/home/konsti/projects/uv/uv-main sync" "/home/konsti/projects/uv/target/profiling/uv sync"
Benchmark 1: /home/konsti/projects/uv/uv-main sync
  Time (mean ± σ):      28.1 ms ±   1.2 ms    [User: 15.5 ms, System: 20.3 ms]
  Range (min … max):    25.7 ms …  31.9 ms    102 runs
 
Benchmark 2: /home/konsti/projects/uv/target/profiling/uv sync
  Time (mean ± σ):      24.0 ms ±   1.2 ms    [User: 13.8 ms, System: 9.9 ms]
  Range (min … max):    22.2 ms …  28.2 ms    122 runs
 
Summary
  /home/konsti/projects/uv/target/profiling/uv sync ran
    1.17 ± 0.08 times faster than /home/konsti/projects/uv/uv-main sync
```

---

_Label `performance` added by @konstin on 2024-11-26 09:03_

---

_@charliermarsh approved on 2024-11-26 14:02_

Thanks!

---

_@charliermarsh reviewed on 2024-11-26 14:18_

---

_Review comment by @charliermarsh on `crates/uv/src/lib.rs`:8 on 2024-11-26 14:18_

I normally don't comment on this stuff but can we keep this as `std` then third-party? (Only because this moves a lot of imports around unnecessarily.)

---

_Merged by @konstin on 2024-11-26 14:58_

---

_Closed by @konstin on 2024-11-26 14:58_

---

_Branch deleted on 2024-11-26 14:58_

---

_Comment by @zanieb on 2024-11-26 15:34_

Nice!

---
