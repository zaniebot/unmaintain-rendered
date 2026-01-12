```yaml
number: 6566
title: "Remove `Stmt::TryStar`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/try-star
created_at: 2023-08-14T16:59:52Z
updated_at: 2023-08-14T17:39:45Z
url: https://github.com/astral-sh/ruff/pull/6566
synced_at: 2026-01-12T15:55:21Z
```

# Remove `Stmt::TryStar`

---

_@charliermarsh_

## Summary

Instead, we set an `is_star` flag on `Stmt::Try`. This is similar to the pattern we've migrated towards for `Stmt::For` (removing `Stmt::AsyncFor`) and friends. While these are significant differences for an interpreter, we tend to handle these cases identically or nearly identically.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-14 16:59_

---

_Label `internal` added by @charliermarsh on 2023-08-14 16:59_

---

_Comment by @github-actions[bot] on 2023-08-14 17:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.5±0.04ms     9.0 MB/sec    1.01      4.6±0.04ms     8.9 MB/sec
formatter/numpy/ctypeslib.py               1.00    891.5±4.13µs    18.7 MB/sec    1.03    918.1±4.73µs    18.1 MB/sec
formatter/numpy/globals.py                 1.00     90.1±0.63µs    32.7 MB/sec    1.04     93.4±1.29µs    31.6 MB/sec
formatter/pydantic/types.py                1.00  1797.2±26.35µs    14.2 MB/sec    1.03   1849.0±5.23µs    13.8 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.06ms     3.2 MB/sec    1.01     12.7±0.04ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    470.3±0.81µs     6.3 MB/sec    1.00    467.5±1.91µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.04ms     3.9 MB/sec    1.01      6.5±0.04ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.03ms     6.3 MB/sec    1.03      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1424.8±10.65µs    11.7 MB/sec    1.04   1475.9±4.31µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.2±3.60µs    17.4 MB/sec    1.02    172.5±3.84µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.06ms     8.7 MB/sec    1.03      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      4.3±0.06ms     9.5 MB/sec    1.00      4.2±0.06ms     9.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   835.7±26.64µs    19.9 MB/sec    1.00   832.6±12.29µs    20.0 MB/sec
formatter/numpy/globals.py                 1.00     83.5±1.22µs    35.3 MB/sec    1.02     85.2±2.85µs    34.6 MB/sec
formatter/pydantic/types.py                1.00  1710.5±31.41µs    14.9 MB/sec    1.00  1703.9±77.54µs    15.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.9±0.10ms     3.2 MB/sec    1.02     13.1±0.12ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.7 MB/sec    1.01      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    439.7±5.35µs     6.7 MB/sec    1.00    441.7±6.25µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.06ms     3.8 MB/sec    1.01      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.05ms     5.8 MB/sec    1.02      7.1±0.05ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1484.7±26.39µs    11.2 MB/sec    1.01  1496.9±15.37µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    174.8±2.85µs    16.9 MB/sec    1.00    173.5±2.91µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.1 MB/sec    1.01      3.2±0.04ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-08-14 17:37_

---

_Merged by @charliermarsh on 2023-08-14 17:39_

---

_Closed by @charliermarsh on 2023-08-14 17:39_

---

_Branch deleted on 2023-08-14 17:39_

---
