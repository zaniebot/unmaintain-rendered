```yaml
number: 6757
title: "Add docs for `DTZ007`"
type: pull_request
state: merged
author: klistwan
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/DTZ007
created_at: 2023-08-22T07:56:05Z
updated_at: 2023-08-31T02:58:11Z
url: https://github.com/astral-sh/ruff/pull/6757
synced_at: 2026-01-12T15:55:22Z
```

# Add docs for `DTZ007`

---

_@klistwan_

Changes:
- Adds docs for `DTZ007`

Related to: https://github.com/astral-sh/ruff/issues/2646

---

_Comment by @github-actions[bot] on 2023-08-22 08:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.1±0.10ms    13.0 MB/sec    1.13      3.5±0.11ms    11.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   673.9±38.66µs    24.7 MB/sec    1.12   754.4±32.61µs    22.1 MB/sec
formatter/numpy/globals.py                 1.00     79.1±3.59µs    37.3 MB/sec    1.06     83.5±3.86µs    35.4 MB/sec
formatter/pydantic/types.py                1.00  1299.7±69.44µs    19.6 MB/sec    1.11  1440.5±62.04µs    17.7 MB/sec
linter/all-rules/large/dataset.py          1.00     10.0±0.59ms     4.1 MB/sec    1.07     10.7±0.27ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.6±0.09ms     6.4 MB/sec    1.11      2.9±0.06ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   396.4±24.96µs     7.4 MB/sec    1.03   409.8±12.07µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.34ms     4.8 MB/sec    1.05      5.6±0.13ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      5.1±0.12ms     8.0 MB/sec    1.13      5.8±0.12ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1150.7±40.12µs    14.5 MB/sec    1.12  1285.0±34.19µs    13.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   139.5±10.78µs    21.1 MB/sec    1.04    145.6±4.32µs    20.3 MB/sec
linter/default-rules/pydantic/types.py     1.03      2.7±0.12ms     9.5 MB/sec    1.00      2.6±0.07ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      2.8±0.02ms    14.4 MB/sec    1.01      2.8±0.02ms    14.3 MB/sec
formatter/numpy/ctypeslib.py               1.00    560.0±5.79µs    29.7 MB/sec    1.00    559.4±7.79µs    29.8 MB/sec
formatter/numpy/globals.py                 1.00     55.8±0.69µs    52.9 MB/sec    1.02     56.8±1.06µs    51.9 MB/sec
formatter/pydantic/types.py                1.00  1165.8±14.85µs    21.9 MB/sec    1.01  1173.3±17.85µs    21.7 MB/sec
linter/all-rules/large/dataset.py          1.00      9.9±0.04ms     4.1 MB/sec    1.01     10.0±0.07ms     4.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    300.5±2.77µs     9.8 MB/sec    1.00    301.5±5.03µs     9.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.03ms     5.0 MB/sec    1.00      5.1±0.03ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.03ms     7.3 MB/sec    1.02      5.6±0.03ms     7.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1143.4±13.92µs    14.6 MB/sec    1.01  1156.7±11.41µs    14.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    121.0±1.37µs    24.4 MB/sec    1.02    123.8±1.41µs    23.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.5 MB/sec    1.01      2.5±0.02ms    10.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_datetimez/rules/call_datetime_strptime_without_zone.rs`:21 on 2023-08-22 13:54_

I think we want to retain this comment

---

_@zanieb reviewed on 2023-08-22 13:54_

---

_Label `documentation` added by @charliermarsh on 2023-08-22 21:03_

---

_Merged by @charliermarsh on 2023-08-22 21:12_

---

_Closed by @charliermarsh on 2023-08-22 21:12_

---

_Branch deleted on 2023-08-31 02:58_

---
