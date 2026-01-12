```yaml
number: 5659
title: "Audit some `SemanticModel#is_builtin` usages"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/builtin
created_at: 2023-07-10T16:57:43Z
updated_at: 2023-07-10T17:30:25Z
url: https://github.com/astral-sh/ruff/pull/5659
synced_at: 2026-01-12T03:36:55Z
```

# Audit some `SemanticModel#is_builtin` usages

---

_Pull request opened by @charliermarsh on 2023-07-10 16:57_

## Summary

Non-behavior-changing refactors to delay some `.is_builtin` calls in a few older rules. Cheaper pre-conditions should always be checked first.


---

_Label `internal` added by @charliermarsh on 2023-07-10 16:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/sys_exit_alias.rs`:64 on 2023-07-10 16:58_

No need to loop here.

---

_@charliermarsh reviewed on 2023-07-10 16:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_blind_except/rules/blind_except.rs`:66 on 2023-07-10 16:58_

No need to loop here.

---

_@charliermarsh reviewed on 2023-07-10 16:58_

---

_@charliermarsh reviewed on 2023-07-10 16:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/redundant_open_modes.rs`:195 on 2023-07-10 16:58_

Moved this up, and all helper utilities below it, per more recent conventions.

---

_@zanieb approved on 2023-07-10 17:01_

lgtm

---

_Comment by @github-actions[bot] on 2023-07-10 17:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.03ms     4.8 MB/sec    1.00      8.4±0.03ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1808.1±3.59µs     9.2 MB/sec    1.00  1802.1±14.67µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    198.7±1.88µs    14.8 MB/sec    1.00    198.3±0.62µs    14.9 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.02ms     6.2 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.05ms     2.8 MB/sec    1.00     14.3±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.7 MB/sec    1.00      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.3±1.07µs     8.1 MB/sec    1.00    363.8±0.96µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.3±0.04ms     4.0 MB/sec    1.00      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.7 MB/sec    1.00      7.2±0.03ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1467.7±2.27µs    11.3 MB/sec    1.00   1464.0±3.20µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.4±0.23µs    19.0 MB/sec    1.01    156.5±1.46µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.14ms     4.8 MB/sec    1.00      8.5±0.13ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1847.0±29.64µs     9.0 MB/sec    1.00  1855.2±58.42µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    206.7±5.74µs    14.3 MB/sec    1.01    208.8±6.33µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.08ms     6.2 MB/sec    1.01      4.2±0.09ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.24ms     2.8 MB/sec    1.04     15.0±0.29ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.06ms     4.4 MB/sec    1.03      3.9±0.12ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   455.4±18.68µs     6.5 MB/sec    1.04   473.8±10.35µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.12ms     4.0 MB/sec    1.04      6.7±0.18ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.10ms     5.6 MB/sec    1.04      7.5±0.19ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1534.3±26.11µs    10.9 MB/sec    1.01  1553.8±35.42µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.2±4.39µs    16.5 MB/sec    1.01   180.8±14.72µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.09ms     7.9 MB/sec    1.00      3.3±0.05ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-10 17:10_

---

_Closed by @charliermarsh on 2023-07-10 17:10_

---

_Branch deleted on 2023-07-10 17:10_

---
