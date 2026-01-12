```yaml
number: 4847
title: Invert parent-shadowed bindings map
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/shadow
created_at: 2023-06-04T04:01:05Z
updated_at: 2023-06-04T04:30:55Z
url: https://github.com/astral-sh/ruff/pull/4847
synced_at: 2026-01-12T15:55:16Z
```

# Invert parent-shadowed bindings map

---

_@charliermarsh_

## Summary

Rather than storing a map from binding to bindings that shadow it, it's easier (and more consistent with the within-scope shadowing) to store a map from binding to binding that it shadows.


---

_Comment by @github-actions[bot] on 2023-06-04 04:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     20.0±0.36ms     2.0 MB/sec    1.00     19.6±0.49ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.6±0.14ms     3.6 MB/sec    1.00      4.6±0.12ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   578.7±27.03µs     5.1 MB/sec    1.01   582.5±25.19µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.2±0.19ms     3.1 MB/sec    1.00      8.1±0.20ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.03      9.4±0.27ms     4.3 MB/sec    1.00      9.1±0.19ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.0±0.09ms     8.3 MB/sec    1.00  1992.7±77.05µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    234.7±7.46µs    12.6 MB/sec    1.00    234.5±9.65µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.11ms     6.1 MB/sec    1.00      4.2±0.14ms     6.1 MB/sec
parser/large/dataset.py                    1.00      7.4±0.25ms     5.5 MB/sec    1.00      7.4±0.15ms     5.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1443.2±46.30µs    11.5 MB/sec    1.00  1436.9±37.64µs    11.6 MB/sec
parser/numpy/globals.py                    1.04    149.8±8.62µs    19.7 MB/sec    1.00    143.3±4.53µs    20.6 MB/sec
parser/pydantic/types.py                   1.00      3.2±0.07ms     8.0 MB/sec    1.01      3.2±0.21ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.16ms     2.5 MB/sec    1.01     16.7±0.26ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   496.2±10.85µs     5.9 MB/sec    1.00    494.0±5.58µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.13ms     3.6 MB/sec    1.00      7.0±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.06ms     4.9 MB/sec    1.00      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1745.9±30.98µs     9.5 MB/sec    1.00  1753.3±17.63µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.9±4.10µs    14.6 MB/sec    1.03   207.6±22.61µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.01      3.7±0.04ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.4±0.05ms     6.4 MB/sec    1.01      6.5±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1205.9±11.94µs    13.8 MB/sec    1.01  1215.4±13.35µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    124.2±2.83µs    23.8 MB/sec    1.00    124.7±1.95µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.02ms     9.4 MB/sec    1.01      2.8±0.04ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-04 04:18_

---

_Closed by @charliermarsh on 2023-06-04 04:18_

---

_Branch deleted on 2023-06-04 04:18_

---
