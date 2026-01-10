```yaml
number: 10029
title: Batch prefetch per fork
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/batch-prefetch-per-fork
created_at: 2024-12-19T14:32:44Z
updated_at: 2024-12-19T14:47:04Z
url: https://github.com/astral-sh/uv/pull/10029
synced_at: 2026-01-10T12:00:01Z
```

# Batch prefetch per fork

---

_Pull request opened by @konstin on 2024-12-19 14:32_

Previously, the batch prefetcher was part of the solver loop, used across forks. This would lead to each preference in a fork being counted as a tried version, so that after 5 forks with the identical version, we would start batch prefetching. The reported numbers of tried versions are also reported. By tracking the batch prefetcher on the fork the numbers are corrected.

An alternative would be tracking the actually tried versions, but that would mean more overhead in the top level solver loop when the current heuristic works.

In `ecosystem/transformers`:

```
$ hyperfine --runs 10 --prepare "rm -f uv.lock" "../../target/release/uv lock --exclude-newer 2024-08-08T00:00:00Z" "uv lock --exclude-newer 2024-08-08T00:00:00Z"
Benchmark 1: ../../target/release/uv lock --exclude-newer 2024-08-08T00:00:00Z
  Time (mean ± σ):     386.2 ms ±   6.1 ms    [User: 396.0 ms, System: 144.5 ms]
  Range (min … max):   378.5 ms … 397.9 ms    10 runs

Benchmark 2: uv lock --exclude-newer 2024-08-08T00:00:00Z
  Time (mean ± σ):     422.0 ms ±   5.5 ms    [User: 459.6 ms, System: 190.3 ms]
  Range (min … max):   415.0 ms … 430.5 ms    10 runs

Summary
  ../../target/release/uv lock --exclude-newer 2024-08-08T00:00:00Z ran
    1.09 ± 0.02 times faster than uv lock --exclude-newer 2024-08-08T00:00:00Z
```

---

_Review requested from @charliermarsh by @konstin on 2024-12-19 14:32_

---

_@charliermarsh approved on 2024-12-19 14:43_

Nice, thanks -- this makes sense.

---

_Label `performance` added by @charliermarsh on 2024-12-19 14:44_

---

_Merged by @konstin on 2024-12-19 14:47_

---

_Closed by @konstin on 2024-12-19 14:47_

---

_Branch deleted on 2024-12-19 14:47_

---
