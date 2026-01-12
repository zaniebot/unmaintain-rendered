```yaml
number: 4588
title: Add Pyflakes docs
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: f4-docs
created_at: 2023-05-22T21:42:07Z
updated_at: 2023-07-10T09:55:33Z
url: https://github.com/astral-sh/ruff/pull/4588
synced_at: 2026-01-12T15:55:15Z
```

# Add Pyflakes docs

---

_@tjkuson_

Add rules for the F4 docs.

Related to #2646.

---

_Comment by @github-actions[bot] on 2023-05-22 22:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.2±0.11ms     2.4 MB/sec    1.00     17.2±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.01ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.04   534.1±13.38µs     5.5 MB/sec    1.00    513.0±1.44µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.02ms     3.6 MB/sec    1.00      7.1±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.02ms     5.0 MB/sec    1.01      8.2±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1772.4±30.31µs     9.4 MB/sec    1.00   1774.2±3.89µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.6±0.91µs    15.0 MB/sec    1.07   211.1±11.09µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.01      3.7±0.01ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.4±0.01ms     6.3 MB/sec    1.09      7.0±0.00ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1270.5±1.38µs    13.1 MB/sec    1.07   1361.7±0.86µs    12.2 MB/sec
parser/numpy/globals.py                    1.00    130.6±1.01µs    22.6 MB/sec    1.05    137.4±0.31µs    21.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.00ms     9.2 MB/sec    1.07      3.0±0.00ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.3±0.14ms     3.1 MB/sec    1.01     13.5±0.15ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.1 MB/sec    1.00      3.3±0.03ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    340.0±2.29µs     8.7 MB/sec    1.00    341.2±2.62µs     8.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.02ms     4.6 MB/sec    1.00      5.5±0.02ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.14ms     6.0 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1348.6±13.09µs    12.3 MB/sec    1.00   1353.2±5.33µs    12.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    146.7±0.91µs    20.1 MB/sec    1.01    148.0±3.41µs    19.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.02ms     8.6 MB/sec    1.01      3.0±0.03ms     8.6 MB/sec
parser/large/dataset.py                    1.00      5.5±0.03ms     7.4 MB/sec    1.02      5.6±0.01ms     7.2 MB/sec
parser/numpy/ctypeslib.py                  1.00   1029.4±4.20µs    16.2 MB/sec    1.02   1051.2±6.45µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    103.2±1.53µs    28.6 MB/sec    1.03    106.0±0.84µs    27.8 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.01ms    11.1 MB/sec    1.03      2.4±0.02ms    10.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @MichaReiser on 2023-05-23 05:22_

---

_Label `documentation` added by @charliermarsh on 2023-05-24 00:38_

---

_Merged by @charliermarsh on 2023-05-24 00:45_

---

_Closed by @charliermarsh on 2023-05-24 00:45_

---

_Branch deleted on 2023-07-10 09:55_

---
