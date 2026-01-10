```yaml
number: 10475
title: "Use `arcstr` for package, extra, and group names"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/pkg
created_at: 2025-01-10T19:15:08Z
updated_at: 2025-01-10T19:46:37Z
url: https://github.com/astral-sh/uv/pull/10475
synced_at: 2026-01-10T11:44:51Z
```

# Use `arcstr` for package, extra, and group names

---

_Pull request opened by @charliermarsh on 2025-01-10 19:15_

## Summary

This appears to be a consistent 1% performance improvement and should also reduce memory quite a bit. We've also decided to use these for markers, so it's nice to use the same optimization here.

```
❯ hyperfine "./uv pip compile --universal scripts/requirements/airflow.in" "./arcstr pip compile --universal scripts/requirements/airflow.in" --min-runs 50 --warmup 20
Benchmark 1: ./uv pip compile --universal scripts/requirements/airflow.in
  Time (mean ± σ):     136.3 ms ±   4.0 ms    [User: 139.1 ms, System: 241.9 ms]
  Range (min … max):   131.5 ms … 149.5 ms    50 runs

Benchmark 2: ./arcstr pip compile --universal scripts/requirements/airflow.in
  Time (mean ± σ):     134.9 ms ±   3.2 ms    [User: 137.6 ms, System: 239.0 ms]
  Range (min … max):   130.1 ms … 151.8 ms    50 runs

Summary
  ./arcstr pip compile --universal scripts/requirements/airflow.in ran
    1.01 ± 0.04 times faster than ./uv pip compile --universal scripts/requirements/airflow.in
```

---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-10 19:15_

---

_Review requested from @konstin by @charliermarsh on 2025-01-10 19:15_

---

_Label `performance` added by @charliermarsh on 2025-01-10 19:15_

---

_@zanieb approved on 2025-01-10 19:16_

---

_Review comment by @BurntSushi on `crates/uv-normalize/src/small_string.rs`:8 on 2025-01-10 19:45_

I approve this encapsulation.

---

_@BurntSushi approved on 2025-01-10 19:45_

---

_@charliermarsh reviewed on 2025-01-10 19:45_

---

_Review comment by @charliermarsh on `crates/uv-normalize/src/small_string.rs`:8 on 2025-01-10 19:45_

Thank you!

---

_Merged by @charliermarsh on 2025-01-10 19:46_

---

_Closed by @charliermarsh on 2025-01-10 19:46_

---

_Branch deleted on 2025-01-10 19:46_

---
