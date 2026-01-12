```yaml
number: 5966
title: Remove nested f-string flag
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/nested
created_at: 2023-07-22T02:35:32Z
updated_at: 2023-07-22T03:06:38Z
url: https://github.com/astral-sh/ruff/pull/5966
synced_at: 2026-01-12T15:55:20Z
```

# Remove nested f-string flag

---

_@charliermarsh_

## Summary

Not worth taking up a slot in the semantic model flags.

---

_Marked ready for review by @charliermarsh on 2023-07-22 02:35_

---

_Label `internal` added by @charliermarsh on 2023-07-22 02:35_

---

_Merged by @charliermarsh on 2023-07-22 02:51_

---

_Closed by @charliermarsh on 2023-07-22 02:51_

---

_Branch deleted on 2023-07-22 02:51_

---

_Comment by @github-actions[bot] on 2023-07-22 02:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.02ms     4.2 MB/sec    1.00      9.8±0.02ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1890.7±2.66µs     8.8 MB/sec    1.00   1899.7±3.56µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    205.2±0.37µs    14.4 MB/sec    1.00    204.9±0.26µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.02      4.3±0.01ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.07     14.5±0.03ms     2.8 MB/sec    1.00     13.5±0.02ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.6±0.01ms     4.6 MB/sec    1.00      3.5±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.02    381.6±1.68µs     7.7 MB/sec    1.00    374.6±1.13µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.06      6.4±0.01ms     4.0 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.14      8.0±0.01ms     5.1 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.11   1575.9±2.71µs    10.6 MB/sec    1.00   1425.5±4.68µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.07    160.5±1.37µs    18.4 MB/sec    1.00    149.6±4.98µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.12      3.5±0.06ms     7.3 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.6±1.29ms     3.5 MB/sec    1.02     11.8±0.09ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.39ms     7.5 MB/sec    1.01      2.2±0.01ms     7.4 MB/sec
formatter/numpy/globals.py                 1.00    228.3±1.53µs    12.9 MB/sec    1.06    242.4±8.63µs    12.2 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.02ms     5.5 MB/sec    1.08      5.0±0.03ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.05ms     2.7 MB/sec    1.01     15.4±0.36ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.00      4.0±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.1±5.97µs     7.1 MB/sec    1.00    414.1±4.67µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.03ms     3.7 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.36ms     5.0 MB/sec    1.00      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1618.5±7.45µs    10.3 MB/sec    1.00  1624.4±13.10µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    173.3±6.56µs    17.0 MB/sec    1.00    172.4±1.58µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
