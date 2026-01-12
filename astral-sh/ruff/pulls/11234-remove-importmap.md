```yaml
number: 11234
title: Remove ImportMap
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: remove-import-map
created_at: 2024-05-01T18:36:36Z
updated_at: 2024-05-02T18:26:03Z
url: https://github.com/astral-sh/ruff/pull/11234
synced_at: 2026-01-12T15:55:37Z
```

# Remove ImportMap

---

_@MichaReiser_

## Summary

This PR removes the `ImportMap` implementation and all its routing through ruff. 

The import map was added in https://github.com/astral-sh/ruff/pull/3243 but we then never ended up using it to do cross file analysis. 

We are now working on adding multifile analysis to ruff, and revisit import resolution as part of it.


```
hyperfine --warmup 10 --runs 20 --setup "./target/release/ruff clean" \
              "./target/release/ruff check crates/ruff_linter/resources/test/cpython -e -s --extend-select=I" \
              "./target/release/ruff-import check crates/ruff_linter/resources/test/cpython -e -s --extend-select=I" 
Benchmark 1: ./target/release/ruff check crates/ruff_linter/resources/test/cpython -e -s --extend-select=I
  Time (mean ± σ):      37.6 ms ±   0.9 ms    [User: 52.2 ms, System: 63.7 ms]
  Range (min … max):    35.8 ms …  39.8 ms    20 runs
 
Benchmark 2: ./target/release/ruff-import check crates/ruff_linter/resources/test/cpython -e -s --extend-select=I
  Time (mean ± σ):      36.0 ms ±   0.7 ms    [User: 50.3 ms, System: 58.4 ms]
  Range (min … max):    34.5 ms …  37.6 ms    20 runs
 
Summary
  ./target/release/ruff-import check crates/ruff_linter/resources/test/cpython -e -s --extend-select=I ran
    1.04 ± 0.03 times faster than ./target/release/ruff check crates/ruff_linter/resources/test/cpython -e -s --extend-select=I
```

I suspect that the performance improvement should even be more significant for users that otherwise don't have any diagnostics.


```
hyperfine --warmup 10 --runs 20 --setup "cd ../ecosystem/airflow && ../../ruff/target/release/ruff clean" \
              "./target/release/ruff check ../ecosystem/airflow -e -s --extend-select=I" \
              "./target/release/ruff-import check ../ecosystem/airflow -e -s --extend-select=I" 
Benchmark 1: ./target/release/ruff check ../ecosystem/airflow -e -s --extend-select=I
  Time (mean ± σ):      53.7 ms ±   1.8 ms    [User: 68.4 ms, System: 63.0 ms]
  Range (min … max):    51.1 ms …  58.7 ms    20 runs
 
Benchmark 2: ./target/release/ruff-import check ../ecosystem/airflow -e -s --extend-select=I
  Time (mean ± σ):      50.8 ms ±   1.4 ms    [User: 50.7 ms, System: 60.9 ms]
  Range (min … max):    48.5 ms …  55.3 ms    20 runs
 
Summary
  ./target/release/ruff-import check ../ecosystem/airflow -e -s --extend-select=I ran
    1.06 ± 0.05 times faster than ./target/release/ruff check ../ecosystem/airflow -e -s --extend-select=I

```

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-05-01 18:36_

---

_Comment by @github-actions[bot] on 2024-05-01 19:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2024-05-02 18:22_

---

_@charliermarsh approved on 2024-05-02 18:22_

---

_@charliermarsh approved on 2024-05-02 18:22_

---

_@charliermarsh approved on 2024-05-02 18:22_

---

_Merged by @charliermarsh on 2024-05-02 18:26_

---

_Closed by @charliermarsh on 2024-05-02 18:26_

---

_Branch deleted on 2024-05-02 18:26_

---
