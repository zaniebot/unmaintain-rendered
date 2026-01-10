```yaml
number: 10439
title: Use faster disjointness check for markers
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/faster-is-disjoint
created_at: 2025-01-09T18:10:53Z
updated_at: 2025-01-09T18:31:53Z
url: https://github.com/astral-sh/uv/pull/10439
synced_at: 2026-01-10T11:44:49Z
```

# Use faster disjointness check for markers

---

_Pull request opened by @konstin on 2025-01-09 18:10_

Motivated by #10438. Ideally we wouldn't be calling this code that much in the first place, but it's the more idiomatic way to write this code.

Crude-ish benchmark based on #10438 with the `without_markers` patched out:

```
hyperfine --warmup 1 "./uv-1 pip compile scripts/requirements/airflow2-req.in -c scripts/requirements/airflow2-constraints.txt --universal --python-version 3.8" "./uv-2 pip compile scripts/requirements/airflow2-req.in -c scripts/requirements/airflow2-constraints.txt --universal --python-version 3.8"
Benchmark 1: ./uv-1 pip compile scripts/requirements/airflow2-req.in -c scripts/requirements/airflow2-constraints.txt --universal --python-version 3.8
  Time (mean ± σ):      1.229 s ±  0.018 s    [User: 1.206 s, System: 0.111 s]
  Range (min … max):    1.211 s …  1.269 s    10 runs

Benchmark 2: ./uv-2 pip compile scripts/requirements/airflow2-req.in -c scripts/requirements/airflow2-constraints.txt --universal --python-version 3.8
  Time (mean ± σ):     873.8 ms ±   5.1 ms    [User: 851.2 ms, System: 112.5 ms]
  Range (min … max):   865.2 ms … 880.4 ms    10 runs

Summary
  ./uv-2 pip compile scripts/requirements/airflow2-req.in -c scripts/requirements/airflow2-constraints.txt --universal --python-version 3.8 ran
    1.41 ± 0.02 times faster than ./uv-1 pip compile scripts/requirements/airflow2-req.in -c scripts/requirements/airflow2-constraints.txt --universal --python-version 3.8
```

---

_Label `performance` added by @konstin on 2025-01-09 18:10_

---

_@charliermarsh approved on 2025-01-09 18:20_

---

_Merged by @konstin on 2025-01-09 18:31_

---

_Closed by @konstin on 2025-01-09 18:31_

---

_Branch deleted on 2025-01-09 18:31_

---
