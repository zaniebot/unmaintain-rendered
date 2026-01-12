```yaml
number: 5104
title: "Uniformly put `## Options` at the end of documentation"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/options
created_at: 2023-06-14T23:55:52Z
updated_at: 2023-06-15T00:25:44Z
url: https://github.com/astral-sh/ruff/pull/5104
synced_at: 2026-01-12T15:55:17Z
```

# Uniformly put `## Options` at the end of documentation

---

_@charliermarsh_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-06-14 23:55_

---

_Merged by @charliermarsh on 2023-06-15 00:04_

---

_Closed by @charliermarsh on 2023-06-15 00:04_

---

_Branch deleted on 2023-06-15 00:04_

---

_Comment by @github-actions[bot] on 2023-06-15 00:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.14      9.1±0.33ms     4.5 MB/sec    1.00      8.0±0.28ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.11  1858.5±99.11µs     9.0 MB/sec    1.00  1674.7±62.33µs     9.9 MB/sec
formatter/numpy/globals.py                 1.03   175.5±12.29µs    16.8 MB/sec    1.00   169.7±10.89µs    17.4 MB/sec
formatter/pydantic/types.py                1.13      3.7±0.16ms     6.9 MB/sec    1.00      3.3±0.12ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     19.4±0.55ms     2.1 MB/sec    1.05     20.3±0.89ms  2047.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.12ms     3.7 MB/sec    1.02      4.6±0.17ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.04   581.3±19.68µs     5.1 MB/sec    1.00   558.3±17.45µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.24ms     3.0 MB/sec    1.00      8.4±0.21ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.25     10.9±0.66ms     3.7 MB/sec    1.00      8.7±0.29ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.17      2.2±0.09ms     7.4 MB/sec    1.00  1912.3±74.02µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.11   247.1±11.02µs    11.9 MB/sec    1.00   222.5±12.34µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.22      4.9±0.16ms     5.2 MB/sec    1.00      4.0±0.21ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.11      9.0±0.77ms     4.5 MB/sec     1.00      8.1±0.36ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.05  1803.1±112.62µs     9.2 MB/sec    1.00  1720.5±110.87µs     9.7 MB/sec
formatter/numpy/globals.py                 1.01   174.4±11.06µs    16.9 MB/sec     1.00   173.3±14.76µs    17.0 MB/sec
formatter/pydantic/types.py                1.08      3.6±0.24ms     7.0 MB/sec     1.00      3.4±0.16ms     7.6 MB/sec
linter/all-rules/large/dataset.py          1.09     19.4±1.30ms     2.1 MB/sec     1.00     17.8±1.22ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.7±0.32ms     3.5 MB/sec     1.00      4.5±0.25ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.04   568.0±56.55µs     5.2 MB/sec     1.00   543.7±33.09µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.07      8.2±0.55ms     3.1 MB/sec     1.00      7.7±0.44ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.06      9.3±0.54ms     4.4 MB/sec     1.00      8.8±0.48ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07      2.0±0.13ms     8.1 MB/sec     1.00  1917.7±151.28µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.14   250.0±16.68µs    11.8 MB/sec     1.00   218.4±10.88µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.3±0.29ms     5.9 MB/sec     1.00      4.2±0.30ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
