```yaml
number: 4546
title: "Ignore `#region` code folding marks in eradicate rules"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/fold
created_at: 2023-05-20T16:34:23Z
updated_at: 2023-05-20T17:08:27Z
url: https://github.com/astral-sh/ruff/pull/4546
synced_at: 2026-01-12T03:50:03Z
```

# Ignore `#region` code folding marks in eradicate rules

---

_Pull request opened by @charliermarsh on 2023-05-20 16:34_

Closes #4538.

---

_Merged by @charliermarsh on 2023-05-20 16:45_

---

_Closed by @charliermarsh on 2023-05-20 16:45_

---

_Branch deleted on 2023-05-20 16:45_

---

_Comment by @github-actions[bot] on 2023-05-20 16:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     18.8±0.99ms     2.2 MB/sec    1.00     18.3±0.90ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.10      4.5±0.17ms     3.7 MB/sec    1.00      4.1±0.16ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   538.8±52.23µs     5.5 MB/sec    1.01   544.6±30.42µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.9±0.49ms     3.2 MB/sec    1.00      7.5±0.30ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      8.3±0.33ms     4.9 MB/sec    1.00      8.1±0.41ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1782.8±41.89µs     9.3 MB/sec    1.00  1738.9±67.16µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.04   216.7±20.48µs    13.6 MB/sec    1.00   207.7±20.47µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.7±0.24ms     6.9 MB/sec    1.00      3.6±0.18ms     7.0 MB/sec
parser/large/dataset.py                    1.01      6.7±0.19ms     6.1 MB/sec    1.00      6.6±0.27ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.03  1345.3±92.24µs    12.4 MB/sec    1.00  1303.1±61.44µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    132.1±4.51µs    22.3 MB/sec    1.01    133.8±8.66µs    22.0 MB/sec
parser/pydantic/types.py                   1.07      2.9±0.10ms     8.9 MB/sec    1.00      2.7±0.10ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.22ms     2.5 MB/sec    1.03     16.9±0.28ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.02      4.3±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   488.6±26.93µs     6.0 MB/sec    1.03   503.7±11.79µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.07ms     3.7 MB/sec    1.04      7.1±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.03      8.4±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1693.6±15.20µs     9.8 MB/sec    1.04  1765.4±28.93µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.5±2.71µs    15.6 MB/sec    1.04    197.2±6.89µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.0 MB/sec    1.02      3.7±0.07ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.6±0.04ms     6.2 MB/sec    1.02      6.7±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1233.6±11.27µs    13.5 MB/sec    1.03  1273.0±16.33µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    128.8±2.84µs    22.9 MB/sec    1.02    131.2±1.94µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.2 MB/sec    1.03      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
