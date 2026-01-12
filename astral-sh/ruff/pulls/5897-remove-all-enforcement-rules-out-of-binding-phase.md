```yaml
number: 5897
title: "Remove `__all__` enforcement rules out of binding phase"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/all
created_at: 2023-07-19T19:30:50Z
updated_at: 2023-07-19T21:38:36Z
url: https://github.com/astral-sh/ruff/pull/5897
synced_at: 2026-01-12T03:30:22Z
```

# Remove `__all__` enforcement rules out of binding phase

---

_Pull request opened by @charliermarsh on 2023-07-19 19:30_

## Summary

This PR moves two rules (`invalid-all-format` and `invalid-all-object`) out of the name-binding phase, and into the dedicated pass over all bindings that occurs at the end of the `Checker`. This is part of my continued quest to separate the semantic model-building logic from the actual rule enforcement.


---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/binding.rs`:266 on 2023-07-19 19:34_

I considered putting these flags on `BindingKind::Export` but it increases the size of the enum variant and felt unnecessary.

---

_@charliermarsh reviewed on 2023-07-19 19:34_

---

_Comment by @github-actions[bot] on 2023-07-19 19:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.5±0.29ms     3.5 MB/sec    1.00     11.5±0.37ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.08ms     7.2 MB/sec    1.00      2.3±0.09ms     7.2 MB/sec
formatter/numpy/globals.py                 1.00    262.8±6.10µs    11.2 MB/sec    1.01   266.6±16.09µs    11.1 MB/sec
formatter/pydantic/types.py                1.02      5.0±0.24ms     5.1 MB/sec    1.00      4.9±0.17ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.40ms     2.5 MB/sec    1.01     16.5±0.45ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.13ms     4.0 MB/sec    1.00      4.1±0.09ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   563.1±31.76µs     5.2 MB/sec    1.00   559.5±22.24µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.5±0.24ms     3.4 MB/sec    1.00      7.4±0.20ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.23ms     4.9 MB/sec    1.00      8.4±0.56ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1752.7±38.57µs     9.5 MB/sec    1.00  1750.5±46.64µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    210.2±7.18µs    14.0 MB/sec    1.01   211.3±11.16µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.8±0.23ms     6.7 MB/sec    1.00      3.7±0.09ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.1±0.11ms     3.7 MB/sec    1.00     11.1±0.30ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.01ms     7.7 MB/sec    1.00      2.1±0.05ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    232.7±1.94µs    12.7 MB/sec    1.00    233.8±5.92µs    12.6 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.03ms     5.3 MB/sec    1.00      4.8±0.03ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.11ms     2.7 MB/sec    1.00     15.3±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.1 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    416.9±6.12µs     7.1 MB/sec    1.01    420.9±5.88µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.04ms     3.7 MB/sec    1.01      7.0±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.02ms     5.0 MB/sec    1.01      8.1±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1630.2±12.39µs    10.2 MB/sec    1.01  1652.9±87.85µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.7±1.16µs    17.2 MB/sec    1.01    174.0±1.91µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `internal` added by @charliermarsh on 2023-07-19 21:07_

---

_Merged by @charliermarsh on 2023-07-19 21:18_

---

_Closed by @charliermarsh on 2023-07-19 21:18_

---

_Branch deleted on 2023-07-19 21:18_

---
