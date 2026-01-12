```yaml
number: 4835
title: Respect mixed variable assignment in RET504
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/ret504
created_at: 2023-06-03T19:23:33Z
updated_at: 2023-06-03T19:58:37Z
url: https://github.com/astral-sh/ruff/pull/4835
synced_at: 2026-01-12T15:55:16Z
```

# Respect mixed variable assignment in RET504

---

_@charliermarsh_

## Summary

In RET504, we haven't been considering function and class assignments at all. This PR modifies the rule to track multiple declarations, but only flag for unused _assignments_.

Closes #4824.


---

_Comment by @github-actions[bot] on 2023-06-03 19:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.8±0.14ms     2.4 MB/sec    1.00     16.7±0.13ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.1 MB/sec    1.00      4.0±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    502.5±4.88µs     5.9 MB/sec    1.00    500.0±5.38µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1770.3±14.49µs     9.4 MB/sec    1.00  1772.6±12.92µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.5±2.13µs    14.6 MB/sec    1.01    203.7±1.57µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.02ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.1±0.04ms     6.6 MB/sec    1.01      6.2±0.04ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1199.3±11.52µs    13.9 MB/sec    1.01  1208.4±11.63µs    13.8 MB/sec
parser/numpy/globals.py                    1.00    123.8±1.70µs    23.8 MB/sec    1.01    124.6±1.76µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.02ms     9.7 MB/sec    1.01      2.7±0.02ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.3±0.15ms     2.3 MB/sec    1.00     17.1±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.04ms     3.8 MB/sec    1.00      4.4±0.02ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.03    455.3±8.16µs     6.5 MB/sec    1.00    443.8±3.86µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.08ms     3.5 MB/sec    1.00      7.3±0.06ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.05ms     4.8 MB/sec    1.01      8.6±0.03ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1803.0±36.58µs     9.2 MB/sec    1.01  1825.5±12.62µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.0±1.81µs    15.0 MB/sec    1.01    198.6±3.42µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.03ms     6.6 MB/sec    1.02      4.0±0.16ms     6.4 MB/sec
parser/large/dataset.py                    1.00      6.6±0.03ms     6.1 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1258.1±36.86µs    13.2 MB/sec    1.00   1256.8±9.40µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    131.3±1.64µs    22.5 MB/sec    1.00    131.6±1.17µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.1 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-06-03 19:39_

---

_Closed by @charliermarsh on 2023-06-03 19:39_

---

_Branch deleted on 2023-06-03 19:39_

---
