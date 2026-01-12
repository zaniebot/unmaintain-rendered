```yaml
number: 5105
title: "Add `target-version` link to relevant rules"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/target-version
created_at: 2023-06-15T00:03:45Z
updated_at: 2023-06-15T00:39:00Z
url: https://github.com/astral-sh/ruff/pull/5105
synced_at: 2026-01-12T03:43:30Z
```

# Add `target-version` link to relevant rules

---

_Pull request opened by @charliermarsh on 2023-06-15 00:03_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-06-15 00:03_

---

_Merged by @charliermarsh on 2023-06-15 00:12_

---

_Closed by @charliermarsh on 2023-06-15 00:12_

---

_Branch deleted on 2023-06-15 00:12_

---

_Comment by @github-actions[bot] on 2023-06-15 00:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      7.0±0.13ms     5.8 MB/sec    1.00      6.8±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.02   1424.5±2.01µs    11.7 MB/sec    1.00   1400.9±4.29µs    11.9 MB/sec
formatter/numpy/globals.py                 1.02    140.0±0.30µs    21.1 MB/sec    1.00    137.2±0.16µs    21.5 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.0 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.09ms     2.8 MB/sec    1.00     14.4±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.01      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    365.7±1.46µs     8.1 MB/sec    1.02    371.5±1.81µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.7 MB/sec    1.00      7.2±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1518.0±3.59µs    11.0 MB/sec    1.00   1514.9±6.40µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.2±0.54µs    17.8 MB/sec    1.00    166.7±0.47µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.08ms     5.2 MB/sec    1.08      8.4±0.16ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1609.4±47.20µs    10.3 MB/sec    1.05  1684.7±25.73µs     9.9 MB/sec
formatter/numpy/globals.py                 1.00    153.2±3.05µs    19.3 MB/sec    1.06    162.0±6.33µs    18.2 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.05ms     8.0 MB/sec    1.07      3.4±0.05ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.03     16.9±0.45ms     2.4 MB/sec    1.00     16.5±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.06ms     3.9 MB/sec    1.00      4.2±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    507.1±8.78µs     5.8 MB/sec    1.00    502.7±8.74µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.4±0.30ms     3.5 MB/sec    1.00      7.2±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.5±0.09ms     4.8 MB/sec    1.00      8.4±0.17ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1789.4±42.93µs     9.3 MB/sec    1.00  1773.1±17.92µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.9±3.99µs    14.5 MB/sec    1.00    203.1±3.25µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.00      3.8±0.04ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
