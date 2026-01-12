```yaml
number: 5623
title: Refactor isort directive skips to use iterators
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/split
created_at: 2023-07-08T18:55:22Z
updated_at: 2023-07-08T19:20:31Z
url: https://github.com/astral-sh/ruff/pull/5623
synced_at: 2026-01-12T15:55:19Z
```

# Refactor isort directive skips to use iterators

---

_@charliermarsh_

## Summary

We're doing some unsafe accesses to advance these iterators. It's easier to model these as actual iterators to ensure safety everywhere. Also added some additional test cases.

Closes #5621.

---

_Label `bug` added by @charliermarsh on 2023-07-08 18:55_

---

_Merged by @charliermarsh on 2023-07-08 19:05_

---

_Closed by @charliermarsh on 2023-07-08 19:05_

---

_Branch deleted on 2023-07-08 19:05_

---

_Comment by @github-actions[bot] on 2023-07-08 19:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.04ms     5.2 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1743.8±6.60µs     9.5 MB/sec    1.00   1742.9±4.55µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    196.0±0.24µs    15.1 MB/sec    1.00    196.6±0.26µs    15.0 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.6 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.10ms     3.0 MB/sec    1.00     13.8±0.08ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    438.9±3.38µs     6.7 MB/sec    1.00    435.8±0.68µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.3 MB/sec    1.00      6.0±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.0 MB/sec    1.00      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1475.4±1.64µs    11.3 MB/sec    1.00   1475.4±2.49µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.5±0.34µs    17.3 MB/sec    1.00    170.2±0.33µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.07ms     4.4 MB/sec    1.01      9.3±0.07ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.03ms     8.3 MB/sec    1.01      2.0±0.04ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    229.6±4.24µs    12.9 MB/sec    1.02   234.9±10.12µs    12.6 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.05ms     5.7 MB/sec    1.01      4.5±0.06ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.5±0.09ms     2.6 MB/sec    1.00     15.5±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    504.0±7.59µs     5.9 MB/sec    1.00    504.1±5.99µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.07ms     5.1 MB/sec    1.00      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1714.7±26.03µs     9.7 MB/sec    1.00  1720.6±14.99µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.6±2.76µs    14.5 MB/sec    1.00    203.3±2.86µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
