```yaml
number: 9446
title: Skip extra settings resolution when namespace packages are empty
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/look
created_at: 2024-01-09T06:15:18Z
updated_at: 2024-01-09T13:33:23Z
url: https://github.com/astral-sh/ruff/pull/9446
synced_at: 2026-01-10T22:57:09Z
```

# Skip extra settings resolution when namespace packages are empty

---

_Pull request opened by @charliermarsh on 2024-01-09 06:15_

Saves 2% on Airflow:

```shell
❯ hyperfine --warmup 20 -i "./target/release/main format ../airflow" "./target/release/ruff format ../airflow"
Benchmark 1: ./target/release/main format ../airflow
  Time (mean ± σ):      72.7 ms ±   0.4 ms    [User: 48.7 ms, System: 75.5 ms]
  Range (min … max):    72.0 ms …  73.7 ms    40 runs

Benchmark 2: ./target/release/ruff format ../airflow
  Time (mean ± σ):      71.4 ms ±   0.6 ms    [User: 46.2 ms, System: 76.2 ms]
  Range (min … max):    70.3 ms …  73.8 ms    41 runs

Summary
  './target/release/ruff format ../airflow' ran
    1.02 ± 0.01 times faster than './target/release/main format ../airflow'
```

---

_Label `internal` added by @charliermarsh on 2024-01-09 06:15_

---

_Comment by @github-actions[bot] on 2024-01-09 06:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-01-09 08:25_

---

_Merged by @charliermarsh on 2024-01-09 13:33_

---

_Closed by @charliermarsh on 2024-01-09 13:33_

---

_Branch deleted on 2024-01-09 13:33_

---
