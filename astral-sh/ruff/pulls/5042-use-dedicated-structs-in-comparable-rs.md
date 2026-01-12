```yaml
number: 5042
title: "Use dedicated structs in `comparable.rs`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/comp
created_at: 2023-06-13T03:45:24Z
updated_at: 2023-06-13T04:15:05Z
url: https://github.com/astral-sh/ruff/pull/5042
synced_at: 2026-01-12T03:43:30Z
```

# Use dedicated structs in `comparable.rs`

---

_Pull request opened by @charliermarsh on 2023-06-13 03:45_

## Summary

Updating to match the updated AST structure, for consistency.

---

_Label `internal` added by @charliermarsh on 2023-06-13 03:45_

---

_Merged by @charliermarsh on 2023-06-13 03:57_

---

_Closed by @charliermarsh on 2023-06-13 03:57_

---

_Branch deleted on 2023-06-13 03:57_

---

_Comment by @github-actions[bot] on 2023-06-13 04:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.1±0.19ms     6.7 MB/sec    1.03      6.3±0.14ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1297.6±58.13µs    12.8 MB/sec    1.01  1309.2±32.45µs    12.7 MB/sec
formatter/numpy/globals.py                 1.00    124.8±4.19µs    23.7 MB/sec    1.04    129.7±7.21µs    22.7 MB/sec
formatter/pydantic/types.py                1.00      2.5±0.10ms    10.1 MB/sec    1.04      2.6±0.13ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.01     13.9±0.38ms     2.9 MB/sec    1.00     13.8±0.42ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.4±0.15ms     4.9 MB/sec    1.00      3.3±0.14ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   425.1±15.53µs     6.9 MB/sec    1.00   421.0±19.89µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.24ms     4.3 MB/sec    1.00      6.0±0.27ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.03      6.9±0.24ms     5.9 MB/sec    1.00      6.7±0.25ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1471.8±60.16µs    11.3 MB/sec    1.00  1428.0±46.60µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    173.6±8.50µs    17.0 MB/sec    1.00   169.9±10.11µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.1±0.12ms     8.3 MB/sec    1.00      3.0±0.10ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      8.2±0.08ms     5.0 MB/sec    1.00      7.7±0.06ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.04  1650.5±15.83µs    10.1 MB/sec    1.00  1589.4±16.33µs    10.5 MB/sec
formatter/numpy/globals.py                 1.01    157.6±2.89µs    18.7 MB/sec    1.00    156.1±8.84µs    18.9 MB/sec
formatter/pydantic/types.py                1.04      3.3±0.03ms     7.7 MB/sec    1.00      3.2±0.05ms     8.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.30ms     2.5 MB/sec    1.00     16.3±0.14ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    501.7±5.78µs     5.9 MB/sec    1.00    500.0±7.80µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.06ms     3.6 MB/sec    1.00      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.06ms     4.9 MB/sec    1.00      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1759.9±20.13µs     9.5 MB/sec    1.00  1753.4±15.45µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.6±2.26µs    14.8 MB/sec    1.01    201.2±7.32µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.8 MB/sec    1.00      3.7±0.03ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
