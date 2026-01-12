```yaml
number: 3789
title: "Clarify order of `pre-commit` hooks"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/pre-commit
created_at: 2023-03-28T23:08:58Z
updated_at: 2023-03-28T23:33:41Z
url: https://github.com/astral-sh/ruff/pull/3789
synced_at: 2026-01-12T04:39:45Z
```

# Clarify order of `pre-commit` hooks

---

_Pull request opened by @charliermarsh on 2023-03-28 23:08_

See: #3784.

---

_Label `documentation` added by @charliermarsh on 2023-03-28 23:09_

---

_Merged by @charliermarsh on 2023-03-28 23:15_

---

_Closed by @charliermarsh on 2023-03-28 23:15_

---

_Branch deleted on 2023-03-28 23:15_

---

_Comment by @github-actions[bot] on 2023-03-28 23:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.4±0.47ms     2.1 MB/sec    1.04     20.2±0.40ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.17ms     3.5 MB/sec    1.02      4.9±0.19ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   622.9±20.42µs     4.7 MB/sec    1.03   640.1±23.19µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.23ms     3.1 MB/sec    1.02      8.3±0.24ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.25ms     4.2 MB/sec    1.04     10.2±0.23ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.07ms     7.7 MB/sec    1.04      2.2±0.07ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   268.6±11.98µs    11.0 MB/sec    1.02   273.9±13.08µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.11ms     5.7 MB/sec    1.05      4.7±0.13ms     5.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.8±0.14ms     2.6 MB/sec    1.00     15.7±0.13ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.6±5.97µs     5.9 MB/sec    1.00    504.7±7.45µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.09ms     3.8 MB/sec    1.00      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     4.9 MB/sec    1.00      8.2±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1789.6±18.15µs     9.3 MB/sec    1.01  1803.5±19.39µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.2±2.84µs    15.2 MB/sec    1.01    196.3±6.55µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.00      3.8±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
