```yaml
number: 4644
title: "Docs: mention `task-tags` option in two rules"
type: pull_request
state: merged
author: bersbersbers
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-05-24T20:11:30Z
updated_at: 2023-05-24T20:42:57Z
url: https://github.com/astral-sh/ruff/pull/4644
synced_at: 2026-01-12T15:55:16Z
```

# Docs: mention `task-tags` option in two rules

---

_@bersbersbers_

## Summary

Mention `task-tags` option in two rules. Close #4618.


---

_Comment by @github-actions[bot] on 2023-05-24 20:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.0±0.18ms     2.4 MB/sec    1.00     16.8±0.05ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.1 MB/sec    1.00      4.0±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.2±0.90µs     5.9 MB/sec    1.00    502.8±0.81µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.02ms     3.6 MB/sec    1.00      7.0±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.02ms     5.0 MB/sec    1.00      8.1±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1806.2±5.68µs     9.2 MB/sec    1.00   1795.9±3.77µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.4±0.80µs    14.5 MB/sec    1.00    202.7±0.43µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     6.8 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.2±0.01ms     6.6 MB/sec    1.00      6.2±0.01ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1221.8±1.89µs    13.6 MB/sec    1.00   1220.3±1.58µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    124.8±0.71µs    23.6 MB/sec    1.00    124.3±0.28µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.01ms     9.6 MB/sec    1.00      2.7±0.00ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.6±0.33ms     2.6 MB/sec    1.00     15.5±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.07ms     4.5 MB/sec    1.01      3.7±0.09ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    396.4±2.85µs     7.4 MB/sec    1.00    397.7±3.47µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.05ms     4.0 MB/sec    1.04      6.6±0.63ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.05ms     5.2 MB/sec    1.02      7.9±0.18ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1603.6±18.17µs    10.4 MB/sec    1.00  1597.1±26.20µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.0±2.74µs    16.6 MB/sec    1.00    177.2±3.47µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.03ms     7.4 MB/sec    1.00      3.4±0.05ms     7.4 MB/sec
parser/large/dataset.py                    1.02      6.3±0.04ms     6.4 MB/sec    1.00      6.2±0.03ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.02  1169.1±12.80µs    14.2 MB/sec    1.00   1148.3±6.32µs    14.5 MB/sec
parser/numpy/globals.py                    1.03    121.1±0.99µs    24.4 MB/sec    1.00    117.8±0.86µs    25.1 MB/sec
parser/pydantic/types.py                   1.02      2.6±0.02ms     9.7 MB/sec    1.00      2.6±0.02ms     9.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-24 20:31_

---

_Closed by @charliermarsh on 2023-05-24 20:31_

---

_Label `documentation` added by @charliermarsh on 2023-05-24 20:31_

---
