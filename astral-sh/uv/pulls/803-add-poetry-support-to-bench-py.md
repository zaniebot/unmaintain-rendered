```yaml
number: 803
title: "Add Poetry support to `bench.py`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/poetry
created_at: 2024-01-05T21:54:52Z
updated_at: 2024-01-06T02:52:57Z
url: https://github.com/astral-sh/uv/pull/803
synced_at: 2026-01-10T15:44:44Z
```

# Add Poetry support to `bench.py`

---

_Pull request opened by @charliermarsh on 2024-01-05 21:54_

## Summary

Enables benchmarking against Poetry for resolution and installation:

```
Benchmark 1: pip-tools (resolve-cold)
  Time (mean ± σ):     962.7 ms ± 241.9 ms    [User: 322.8 ms, System: 80.5 ms]
  Range (min … max):   714.9 ms … 1459.4 ms    10 runs

Benchmark 1: puffin (resolve-cold)
  Time (mean ± σ):     193.2 ms ±   8.2 ms    [User: 31.3 ms, System: 22.8 ms]
  Range (min … max):   179.8 ms … 206.4 ms    14 runs

Benchmark 1: poetry (resolve-cold)
  Time (mean ± σ):     900.7 ms ±  21.2 ms    [User: 371.6 ms, System: 92.1 ms]
  Range (min … max):   855.7 ms … 933.4 ms    10 runs

Benchmark 1: pip-tools (resolve-warm)
  Time (mean ± σ):     386.0 ms ±  19.1 ms    [User: 255.8 ms, System: 46.2 ms]
  Range (min … max):   368.7 ms … 434.5 ms    10 runs

Benchmark 1: puffin (resolve-warm)
  Time (mean ± σ):       8.1 ms ±   0.4 ms    [User: 4.4 ms, System: 5.1 ms]
  Range (min … max):     7.5 ms …  11.1 ms    183 runs

Benchmark 1: poetry (resolve-warm)
  Time (mean ± σ):     336.3 ms ±   0.6 ms    [User: 283.6 ms, System: 44.7 ms]
  Range (min … max):   335.0 ms … 337.3 ms    10 runs
```

---

_Marked ready for review by @charliermarsh on 2024-01-05 22:04_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-05 22:05_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-05 22:05_

---

_Review requested from @konstin by @charliermarsh on 2024-01-05 22:05_

---

_Renamed from "Add Poetry support to `benches.py`" to "Add Poetry support to `bench.py`" by @charliermarsh on 2024-01-05 22:05_

---

_@zanieb reviewed on 2024-01-05 22:07_

---

_Review comment by @zanieb on `scripts/requirements.in`:1 on 2024-01-05 22:07_

Can you put all these files in a `bench` directory or something?

---

_@zanieb approved on 2024-01-05 22:07_

---

_Merged by @charliermarsh on 2024-01-06 02:52_

---

_Closed by @charliermarsh on 2024-01-06 02:52_

---

_Branch deleted on 2024-01-06 02:52_

---
