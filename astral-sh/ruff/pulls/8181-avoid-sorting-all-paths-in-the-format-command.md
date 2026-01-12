```yaml
number: 8181
title: Avoid sorting all paths in the format command
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - formatter
assignees: []
merged: true
base: main
head: charlie/sort
created_at: 2023-10-24T20:09:16Z
updated_at: 2023-10-25T08:45:57Z
url: https://github.com/astral-sh/ruff/pull/8181
synced_at: 2026-01-12T02:32:42Z
```

# Avoid sorting all paths in the format command

---

_Pull request opened by @charliermarsh on 2023-10-24 20:09_

## Summary

Related to https://github.com/astral-sh/ruff/issues/8135.

If we're not printing a `--diff`, or a summary of `--check` changes, we can avoid sorting the list of results. Further, when sorting, we only need to sort a small subset of the entries, in the common case (i.e., in general, it's much more likely that a file is formatted than not).

## Test Plan

Local benchmarks suggest a 5-10% speedup on the cached behavior:

```
❯ hyperfine --warmup 3 "./target/release/ruff format ../airflow" "./target/release/sort format ../airflow"
Benchmark 1: ./target/release/ruff format ../airflow
  Time (mean ± σ):      70.3 ms ±   5.2 ms    [User: 52.1 ms, System: 59.0 ms]
  Range (min … max):    68.3 ms … 101.7 ms    42 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 2: ./target/release/sort format ../airflow
  Time (mean ± σ):      66.0 ms ±   1.4 ms    [User: 48.3 ms, System: 58.4 ms]
  Range (min … max):    64.7 ms …  71.8 ms    44 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Summary
  './target/release/sort format ../airflow' ran
    1.07 ± 0.08 times faster than './target/release/ruff format ../airflow'
```


---

_Review requested from @zanieb by @charliermarsh on 2023-10-24 20:09_

---

_Label `performance` added by @charliermarsh on 2023-10-24 20:09_

---

_Label `formatter` added by @charliermarsh on 2023-10-24 20:09_

---

_@zanieb approved on 2023-10-24 20:21_

---

_Merged by @charliermarsh on 2023-10-24 20:54_

---

_Closed by @charliermarsh on 2023-10-24 20:54_

---

_Branch deleted on 2023-10-24 20:54_

---

_Comment by @github-actions[bot] on 2023-10-24 21:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @konstin on 2023-10-25 08:45_

Thanks!

---
