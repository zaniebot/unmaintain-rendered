```yaml
number: 10346
title: Speed up file pins
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/faster-file-pins
created_at: 2025-01-07T09:31:48Z
updated_at: 2025-01-07T13:51:16Z
url: https://github.com/astral-sh/uv/pull/10346
synced_at: 2026-01-10T11:44:44Z
```

# Speed up file pins

---

_Pull request opened by @konstin on 2025-01-07 09:31_

Ref https://github.com/astral-sh/uv/issues/10344

Avoid the nested hashmap, file pinning is called in the version selection hot loop.

```
$ hyperfine --warmup 1 --prepare "uv venv -p 3.12" "./uv-2 pip compile scripts/requirements/airflow.in" "./uv-1 pip compile scripts/requirements/airflow.in"
    Finished `profiling` profile [optimized + debuginfo] target(s) in 0.12s
Benchmark 1: ./uv-2 pip compile scripts/requirements/airflow.in
  Time (mean ± σ):     420.1 ms ±   4.7 ms    [User: 585.4 ms, System: 195.1 ms]
  Range (min … max):   413.1 ms … 429.2 ms    10 runs

Benchmark 2: ./uv-1 pip compile scripts/requirements/airflow.in
  Time (mean ± σ):     473.0 ms ±   4.9 ms    [User: 654.4 ms, System: 209.4 ms]
  Range (min … max):   468.0 ms … 481.1 ms    10 runs

Summary
  ./uv-2 pip compile scripts/requirements/airflow.in ran
    1.13 ± 0.02 times faster than ./uv-1 pip compile scripts/requirements/airflow.in
```


---

_Label `performance` added by @konstin on 2025-01-07 09:31_

---

_Review requested from @charliermarsh by @konstin on 2025-01-07 09:31_

---

_@konstin reviewed on 2025-01-07 09:32_

---

_Review comment by @konstin on `crates/uv-resolver/src/pins.rs`:20 on 2025-01-07 09:32_

It's also important to only clone when we didn't insert the exact value yet in previous decision.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pins.rs`:29 on 2025-01-07 13:48_

This probably makes more sense as a PR comment than in code, since it won't be clear from the code alone what "faster" means. Like faster relative to what?

---

_@charliermarsh reviewed on 2025-01-07 13:48_

---

_@charliermarsh approved on 2025-01-07 13:48_

---

_Merged by @charliermarsh on 2025-01-07 13:51_

---

_Closed by @charliermarsh on 2025-01-07 13:51_

---

_Branch deleted on 2025-01-07 13:51_

---

_Comment by @charliermarsh on 2025-01-07 13:51_

Very nice!

---
