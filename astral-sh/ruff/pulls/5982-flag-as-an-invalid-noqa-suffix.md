```yaml
number: 5982
title: "Flag `[` as an invalid noqa suffix"
type: pull_request
state: merged
author: charliermarsh
labels:
  - suppression
assignees: []
merged: true
base: main
head: charlie/suffix
created_at: 2023-07-22T14:02:30Z
updated_at: 2023-07-22T14:33:12Z
url: https://github.com/astral-sh/ruff/pull/5982
synced_at: 2026-01-12T03:30:22Z
```

# Flag `[` as an invalid noqa suffix

---

_Pull request opened by @charliermarsh on 2023-07-22 14:02_

Closes https://github.com/astral-sh/ruff/issues/5960.

---

_Label `noqa` added by @charliermarsh on 2023-07-22 14:02_

---

_Comment by @github-actions[bot] on 2023-07-22 14:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.06ms     4.2 MB/sec    1.02      9.9±0.02ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1895.1±3.31µs     8.8 MB/sec    1.02   1923.8±4.82µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    205.8±2.79µs    14.3 MB/sec    1.01    208.1±1.78µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.02      4.3±0.00ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.03ms     3.0 MB/sec    1.01     13.6±0.16ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.9±0.93µs     8.0 MB/sec    1.00    367.9±1.12µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.02ms     5.8 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1424.8±2.42µs    11.7 MB/sec    1.00   1430.1±3.14µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.6±0.24µs    19.7 MB/sec    1.00    149.6±0.28µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.1 MB/sec    1.00      3.1±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.9±0.15ms     4.1 MB/sec    1.00      9.8±0.13ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1935.0±30.60µs     8.6 MB/sec    1.00  1913.6±33.56µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    215.2±6.59µs    13.7 MB/sec    1.02   219.5±16.83µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.08ms     6.0 MB/sec    1.00      4.3±0.08ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.20ms     2.9 MB/sec    1.01     14.1±0.26ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.05ms     4.6 MB/sec    1.00      3.6±0.07ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   440.4±11.12µs     6.7 MB/sec    1.00   437.6±10.84µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.4±0.15ms     4.0 MB/sec    1.00      6.3±0.09ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.02      7.3±0.11ms     5.6 MB/sec    1.00      7.2±0.06ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1512.1±26.67µs    11.0 MB/sec    1.00  1490.9±22.77µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    170.4±7.02µs    17.3 MB/sec    1.00    168.5±4.71µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.3±0.04ms     7.8 MB/sec    1.00      3.2±0.04ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-22 14:16_

---

_Closed by @charliermarsh on 2023-07-22 14:16_

---

_Branch deleted on 2023-07-22 14:16_

---
