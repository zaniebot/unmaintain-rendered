```yaml
number: 11660
title: Reduce overhead in converting resolutions
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/reduce-edge-hashing
created_at: 2025-02-20T11:02:43Z
updated_at: 2025-02-20T20:13:04Z
url: https://github.com/astral-sh/uv/pull/11660
synced_at: 2026-01-10T11:10:38Z
```

# Reduce overhead in converting resolutions

---

_Pull request opened by @konstin on 2025-02-20 11:02_

Solving spent a chunk of its time just converting resolutions, the left two blocks:

![image](https://github.com/user-attachments/assets/6f266440-c6e2-447c-ad7f-f92244f9d09b)

These blocks are `ResolverOutput::from_state` with 1.3% and `ForkState::into_resolution` with 4.1% of resolver thread runtime for apache airflow universal.

We reduce the overhead spent in those functions, to now 1.1% and 2.1% of resolver time spend in those functions by:

Commit 1: Replace the hash set for the edges with a vec in `ForkState::into_resolution`. We deduplicate edges anyway when collecting them, and the hash-and-insert was slow.

Commit 2: Reduce the distribution clonign in `ResolverOutput::from_state` by using an `Arc`.

The same profile excerpt for the resolver with the branch (note that there is now an unrelated block between the two we optimized):

![image](https://github.com/user-attachments/assets/e36c205d-2cf8-4fe6-a2dd-3020c0515922)

Wall times are noisy, but the profiles show those changes as improvements.

```
$ hyperfine --warmup 2 "./uv-main pip compile --no-progress scripts/requirements/airflow.in --universal" "./uv-branch pip compile --no-progress scripts/requirements/airflow.in --universal"
Benchmark 1: ./uv-main pip compile --no-progress scripts/requirements/airflow.in --universal
  Time (mean ± σ):      99.1 ms ±   3.8 ms    [User: 111.8 ms, System: 115.5 ms]
  Range (min … max):    93.6 ms … 110.4 ms    29 runs
 
Benchmark 2: ./uv-branch pip compile --no-progress scripts/requirements/airflow.in --universal
  Time (mean ± σ):      97.1 ms ±   4.3 ms    [User: 114.8 ms, System: 112.0 ms]
  Range (min … max):    90.9 ms … 112.4 ms    29 runs
 
Summary
  ./uv-branch pip compile --no-progress scripts/requirements/airflow.in --universal ran
    1.02 ± 0.06 times faster than ./uv-main pip compile --no-progress scripts/requirements/airflow.in --universal
```

---

_@konstin reviewed on 2025-02-20 11:03_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:3080 on 2025-02-20 11:03_

That's the big perf improvement 

---

_@konstin reviewed on 2025-02-20 11:04_

---

_Review comment by @konstin on `crates/uv-installer/src/plan.rs`:397 on 2025-02-20 11:04_

This is isn't necessary but we avoid the clone as we already have `Arc`s here.

---

_Label `performance` added by @zanieb on 2025-02-20 14:11_

---

_@zanieb approved on 2025-02-20 14:11_

---

_Merged by @charliermarsh on 2025-02-20 20:13_

---

_Closed by @charliermarsh on 2025-02-20 20:13_

---

_Branch deleted on 2025-02-20 20:13_

---
