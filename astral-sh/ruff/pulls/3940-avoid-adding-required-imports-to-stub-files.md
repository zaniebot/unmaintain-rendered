```yaml
number: 3940
title: Avoid adding required imports to stub files
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: charlie/requires-imports
created_at: 2023-04-12T02:20:05Z
updated_at: 2023-04-12T02:42:52Z
url: https://github.com/astral-sh/ruff/pull/3940
synced_at: 2026-01-12T15:55:14Z
```

# Avoid adding required imports to stub files

---

_@charliermarsh_

## Summary

For now, it seems simplest to just avoid adding any `__future__` imports to stub files. I think it's pretty rare to use this feature for anything _other_ than `__future__` imports, but in theory I don't see why non-`__future__` imports would need to be omitted.

Closes #3928.


---

_Label `bug` added by @charliermarsh on 2023-04-12 02:20_

---

_Label `isort` added by @charliermarsh on 2023-04-12 02:20_

---

_Merged by @charliermarsh on 2023-04-12 02:31_

---

_Closed by @charliermarsh on 2023-04-12 02:31_

---

_Branch deleted on 2023-04-12 02:31_

---

_Comment by @github-actions[bot] on 2023-04-12 02:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.4±0.06ms     2.8 MB/sec    1.00     14.3±0.07ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    461.8±3.00µs     6.4 MB/sec    1.00    457.2±0.87µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.03ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.6 MB/sec    1.00      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1623.1±6.00µs    10.3 MB/sec    1.01   1644.9±4.19µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.1±0.54µs    16.3 MB/sec    1.01    182.4±0.95µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.00ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     17.7±0.19ms     2.3 MB/sec     1.01     17.9±0.32ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.12ms     3.5 MB/sec     1.00      4.7±0.10ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   533.2±14.96µs     5.5 MB/sec     1.01   539.3±14.04µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.6±0.15ms     3.3 MB/sec     1.00      7.7±0.12ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.11ms     4.6 MB/sec     1.02      9.0±0.17ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1923.9±184.40µs     8.7 MB/sec    1.01  1950.0±38.97µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    207.2±5.33µs    14.2 MB/sec     1.02    210.5±6.22µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.10ms     6.3 MB/sec     1.01      4.1±0.09ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
