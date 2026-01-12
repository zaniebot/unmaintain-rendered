```yaml
number: 6639
title: "Implement `Ranged` on more structs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/ranged
created_at: 2023-08-17T04:02:45Z
updated_at: 2023-08-17T15:22:40Z
url: https://github.com/astral-sh/ruff/pull/6639
synced_at: 2026-01-12T15:55:22Z
```

# Implement `Ranged` on more structs

---

_@charliermarsh_

## Summary

I noticed some inconsistencies around uses of `.range.start()`, structs that have a `TextRange` field but don't implement `Ranged`, etc.

## Test Plan

`cargo test`


---

_Label `internal` added by @charliermarsh on 2023-08-17 04:02_

---

_Comment by @github-actions[bot] on 2023-08-17 04:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.6±0.02ms    11.3 MB/sec    1.00      3.6±0.03ms    11.3 MB/sec
formatter/numpy/ctypeslib.py               1.00    736.3±8.82µs    22.6 MB/sec    1.00    736.2±3.72µs    22.6 MB/sec
formatter/numpy/globals.py                 1.02     77.2±6.48µs    38.2 MB/sec    1.00     76.0±0.35µs    38.8 MB/sec
formatter/pydantic/types.py                1.00   1466.6±6.15µs    17.4 MB/sec    1.00  1471.4±33.41µs    17.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.09ms     3.9 MB/sec    1.00     10.3±0.10ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     6.0 MB/sec    1.00      2.8±0.03ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   399.9±22.51µs     7.4 MB/sec    1.00    392.9±1.80µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.04ms     4.8 MB/sec    1.00      5.4±0.05ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.4 MB/sec    1.00      5.5±0.02ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1209.0±6.33µs    13.8 MB/sec    1.00   1209.4±4.70µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    141.6±0.77µs    20.8 MB/sec    1.01    142.9±2.70µs    20.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.4 MB/sec    1.00      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.8±0.31ms     8.4 MB/sec     1.02      5.0±0.33ms     8.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   954.0±53.52µs    17.5 MB/sec     1.00   945.0±59.99µs    17.6 MB/sec
formatter/numpy/globals.py                 1.01     99.6±7.76µs    29.6 MB/sec     1.00     98.6±7.13µs    29.9 MB/sec
formatter/pydantic/types.py                1.00  1984.4±121.72µs    12.9 MB/sec    1.00  1984.1±143.07µs    12.9 MB/sec
linter/all-rules/large/dataset.py          1.01     17.0±0.61ms     2.4 MB/sec     1.00     16.8±0.61ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.20ms     3.6 MB/sec     1.00      4.6±0.24ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   577.0±51.14µs     5.1 MB/sec     1.00   578.6±37.52µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.35ms     2.9 MB/sec     1.01      8.8±0.37ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.2±0.41ms     4.4 MB/sec     1.01      9.3±0.41ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1954.0±104.40µs     8.5 MB/sec    1.00  1947.4±93.12µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   240.0±13.72µs    12.3 MB/sec     1.04   249.3±21.95µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.2±0.20ms     6.1 MB/sec     1.00      4.1±0.20ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-08-17 05:35_

---

_Merged by @charliermarsh on 2023-08-17 15:22_

---

_Closed by @charliermarsh on 2023-08-17 15:22_

---

_Branch deleted on 2023-08-17 15:22_

---
