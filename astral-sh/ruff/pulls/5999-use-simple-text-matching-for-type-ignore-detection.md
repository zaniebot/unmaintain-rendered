```yaml
number: 5999
title: "Use simple text matching for `type: ignore` detection"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ignore
created_at: 2023-07-23T01:25:01Z
updated_at: 2023-07-23T02:08:28Z
url: https://github.com/astral-sh/ruff/pull/5999
synced_at: 2026-01-12T03:30:22Z
```

# Use simple text matching for `type: ignore` detection

---

_Pull request opened by @charliermarsh on 2023-07-23 01:25_

Closes #5980.

---

_Renamed from "Use simple text matching for type: ignore detection" to "Use simple text matching for `type: ignore` detection" by @charliermarsh on 2023-07-23 01:34_

---

_Merged by @charliermarsh on 2023-07-23 01:45_

---

_Closed by @charliermarsh on 2023-07-23 01:45_

---

_Branch deleted on 2023-07-23 01:45_

---

_Comment by @github-actions[bot] on 2023-07-23 01:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.10     12.2±0.04ms     3.3 MB/sec    1.00     11.0±0.04ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.08      2.4±0.00ms     7.0 MB/sec    1.00      2.2±0.02ms     7.6 MB/sec
formatter/numpy/globals.py                 1.05    254.0±3.19µs    11.6 MB/sec    1.00    242.9±3.38µs    12.1 MB/sec
formatter/pydantic/types.py                1.08      5.2±0.01ms     4.9 MB/sec    1.00      4.8±0.02ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.01     15.7±0.11ms     2.6 MB/sec    1.00     15.5±0.05ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.01ms     4.3 MB/sec    1.00      3.9±0.01ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    508.6±0.92µs     5.8 MB/sec    1.01    511.8±0.72µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.11ms     5.1 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1666.2±3.42µs    10.0 MB/sec    1.01   1677.0±4.62µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.1±0.60µs    16.0 MB/sec    1.02    187.0±0.38µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.2 MB/sec    1.00      3.6±0.01ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.03ms     3.7 MB/sec    1.03     11.2±0.14ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.01ms     7.9 MB/sec    1.02      2.2±0.02ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    228.8±1.51µs    12.9 MB/sec    1.02    233.2±6.45µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.04ms     5.4 MB/sec    1.02      4.8±0.05ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.23ms     2.6 MB/sec    1.01     15.7±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.04      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    410.9±4.72µs     7.2 MB/sec    1.02    418.3±6.38µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.02ms     3.7 MB/sec    1.04      7.2±0.10ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.02ms     5.1 MB/sec    1.03      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1624.3±8.28µs    10.3 MB/sec    1.03  1672.6±16.57µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.1±1.18µs    17.0 MB/sec    1.00    173.6±1.78µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.02      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
