```yaml
number: 954
title: Add an incremental resolution benchmark
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/bench
created_at: 2024-01-17T19:45:42Z
updated_at: 2024-01-18T18:18:38Z
url: https://github.com/astral-sh/uv/pull/954
synced_at: 2026-01-10T15:39:02Z
```

# Add an incremental resolution benchmark

---

_Pull request opened by @charliermarsh on 2024-01-17 19:45_

## Summary

This adds a benchmark in which we reuse the lockfile, but add a new dependency to the input requirements.

Running `python -m scripts.bench --poetry --puffin --pip-compile scripts/requirements/trio.in --benchmark resolve-warm --benchmark resolve-incremental`:

```text
Benchmark 1: pip-compile (resolve-warm)
  Time (mean ± σ):      1.169 s ±  0.023 s    [User: 0.675 s, System: 0.112 s]
  Range (min … max):    1.129 s …  1.198 s    10 runs

Benchmark 2: poetry (resolve-warm)
  Time (mean ± σ):     610.7 ms ±  10.4 ms    [User: 528.1 ms, System: 60.3 ms]
  Range (min … max):   599.9 ms … 632.6 ms    10 runs

Benchmark 3: puffin (resolve-warm)
  Time (mean ± σ):      19.3 ms ±   0.6 ms    [User: 13.5 ms, System: 13.1 ms]
  Range (min … max):    17.9 ms …  22.1 ms    122 runs

Summary
  'puffin (resolve-warm)' ran
   31.63 ± 1.19 times faster than 'poetry (resolve-warm)'
   60.53 ± 2.37 times faster than 'pip-compile (resolve-warm)'
Benchmark 1: pip-compile (resolve-incremental)
  Time (mean ± σ):      1.554 s ±  0.059 s    [User: 0.974 s, System: 0.130 s]
  Range (min … max):    1.473 s …  1.652 s    10 runs

Benchmark 2: poetry (resolve-incremental)
  Time (mean ± σ):     474.2 ms ±   2.4 ms    [User: 411.7 ms, System: 54.0 ms]
  Range (min … max):   470.6 ms … 477.7 ms    10 runs

Benchmark 3: puffin (resolve-incremental)
  Time (mean ± σ):      28.0 ms ±   1.1 ms    [User: 21.7 ms, System: 14.6 ms]
  Range (min … max):    26.7 ms …  34.4 ms    89 runs

Summary
  'puffin (resolve-incremental)' ran
   16.94 ± 0.67 times faster than 'poetry (resolve-incremental)'
   55.52 ± 3.02 times faster than 'pip-compile (resolve-incremental)'
```

---

_Review requested from @zanieb by @charliermarsh on 2024-01-17 19:45_

---

_Review requested from @konstin by @charliermarsh on 2024-01-17 19:45_

---

_@charliermarsh reviewed on 2024-01-17 19:46_

---

_Review comment by @charliermarsh on `scripts/bench/__main__.py`:110 on 2024-01-17 19:46_

Open to suggestions on what to use here.

---

_Review comment by @konstin on `scripts/bench/__main__.py`:110 on 2024-01-18 10:20_

Django maybe? Doesn't have native code and i remember updating it regularly. But it doesn't matter much imho

---

_@konstin approved on 2024-01-18 10:22_

Thanks!

---

_Label `internal` added by @charliermarsh on 2024-01-18 18:17_

---

_Merged by @charliermarsh on 2024-01-18 18:18_

---

_Closed by @charliermarsh on 2024-01-18 18:18_

---

_Branch deleted on 2024-01-18 18:18_

---
