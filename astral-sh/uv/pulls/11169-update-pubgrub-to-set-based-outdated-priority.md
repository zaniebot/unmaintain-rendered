```yaml
number: 11169
title: Update pubgrub to set-based outdated priority tracking
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - performance
assignees: []
merged: true
base: main
head: konsti/update-pubgrub-set
created_at: 2025-02-02T17:37:01Z
updated_at: 2025-02-03T12:08:53Z
url: https://github.com/astral-sh/uv/pull/11169
synced_at: 2026-01-12T16:09:42Z
```

# Update pubgrub to set-based outdated priority tracking

---

_@konstin_

Looks like the set based prioritize tracking from https://github.com/pubgrub-rs/pubgrub/pull/313 is a slight speedup.

I assume the changed derivation tree in the error snapshot is due to out-of-sync virtual package priorities, while the main package priority defining the solution remains stable.

```
$ hyperfine --warmup 2 "./uv-main pip compile --no-progress scripts/requirements/airflow.in --universal" "./uv-branch pip compile --no-progress scripts/requirements/airflow.in --universal"
  Benchmark 1: ./uv-main pip compile --no-progress scripts/requirements/airflow.in --universal
    Time (mean ± σ):     115.0 ms ±   4.8 ms    [User: 131.0 ms, System: 113.6 ms]
    Range (min … max):   108.1 ms … 125.8 ms    25 runs

  Benchmark 2: ./uv-branch pip compile --no-progress scripts/requirements/airflow.in --universal
    Time (mean ± σ):     105.4 ms ±   2.6 ms    [User: 118.5 ms, System: 113.5 ms]
    Range (min … max):   101.1 ms … 111.9 ms    28 runs

  Summary
    ./uv-branch pip compile --no-progress scripts/requirements/airflow.in --universal ran
      1.09 ± 0.05 times faster than ./uv-main pip compile --no-progress scripts/requirements/airflow.in --universal
```

---

_Label `enhancement` added by @konstin on 2025-02-02 17:37_

---

_@charliermarsh approved on 2025-02-02 17:57_

Nice!

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-02 17:57_

---

_Unassigned @charliermarsh by @charliermarsh on 2025-02-02 17:57_

---

_Label `performance` added by @charliermarsh on 2025-02-02 17:57_

---

_Merged by @konstin on 2025-02-03 12:08_

---

_Closed by @konstin on 2025-02-03 12:08_

---

_Branch deleted on 2025-02-03 12:08_

---
