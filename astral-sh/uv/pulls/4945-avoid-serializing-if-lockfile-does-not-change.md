```yaml
number: 4945
title: Avoid serializing if lockfile does not change
type: pull_request
state: merged
author: ibraheemdev
labels:
  - performance
assignees: []
merged: true
base: main
head: ibraheem/fast-lock
created_at: 2024-07-09T20:51:38Z
updated_at: 2024-07-09T21:08:28Z
url: https://github.com/astral-sh/uv/pull/4945
synced_at: 2026-01-12T16:06:32Z
```

# Avoid serializing if lockfile does not change

---

_@ibraheemdev_

## Summary

Avoid serializing and writing the lockfile if a cheap comparison shows that the contents have not changed.

## Test Plan

Shaves ~10ms off of https://github.com/astral-sh/uv/issues/4860 for me.

```
➜  transformers hyperfine "../../uv/target/profiling/uv lock" "../../uv/target/profiling/baseline lock" --warmup 3
Benchmark 1: ../../uv/target/profiling/uv lock
  Time (mean ± σ):     130.5 ms ±   2.5 ms    [User: 130.3 ms, System: 85.0 ms]
  Range (min … max):   126.8 ms … 136.9 ms    23 runs
 
Benchmark 2: ../../uv/target/profiling/baseline lock
  Time (mean ± σ):     140.5 ms ±   5.0 ms    [User: 142.8 ms, System: 85.5 ms]
  Range (min … max):   133.2 ms … 153.3 ms    21 runs
 
Summary
  ../../uv/target/profiling/uv lock ran
    1.08 ± 0.04 times faster than ../../uv/target/profiling/baseline lock
```

---

_Review requested from @konstin by @ibraheemdev on 2024-07-09 20:51_

---

_Label `performance` added by @ibraheemdev on 2024-07-09 20:51_

---

_@charliermarsh approved on 2024-07-09 20:52_

---

_@charliermarsh reviewed on 2024-07-09 20:52_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:178 on 2024-07-09 20:52_

Minor nit: maybe `existing_lock` and `lock`, rather than `lock` and `new_lock`?

---

_Merged by @ibraheemdev on 2024-07-09 21:08_

---

_Closed by @ibraheemdev on 2024-07-09 21:08_

---

_Branch deleted on 2024-07-09 21:08_

---
