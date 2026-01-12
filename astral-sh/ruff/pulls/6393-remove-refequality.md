```yaml
number: 6393
title: "Remove `RefEquality`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/ref-equality
created_at: 2023-08-07T15:32:24Z
updated_at: 2023-08-07T16:19:55Z
url: https://github.com/astral-sh/ruff/pull/6393
synced_at: 2026-01-12T02:52:04Z
```

# Remove `RefEquality`

---

_Pull request opened by @charliermarsh on 2023-08-07 15:32_

## Summary

See discussion in https://github.com/astral-sh/ruff/pull/6351#discussion_r1284996979. We can remove `RefEquality` entirely and instead use a text offset for statement keys, since no two statements can start at the same text offset.

## Test Plan

`cargo test`


---

_Marked ready for review by @charliermarsh on 2023-08-07 15:33_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-07 15:33_

---

_Label `internal` added by @charliermarsh on 2023-08-07 15:35_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/statements.rs`:37 on 2023-08-07 15:37_

Nit: We should expand on **why** this is true.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/statements.rs`:38 on 2023-08-07 15:37_

```suggestion
#[derive(Debug, Copy, Clone, Hash, PartialEq, Eq)]
```

---

_@MichaReiser approved on 2023-08-07 15:37_

---

_Comment by @github-actions[bot] on 2023-08-07 15:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.04ms     4.9 MB/sec    1.00      8.4±0.03ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1602.1±11.84µs    10.4 MB/sec    1.01   1615.7±6.59µs    10.3 MB/sec
formatter/numpy/globals.py                 1.00    173.2±0.33µs    17.0 MB/sec    1.00    173.3±0.19µs    17.0 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.02ms     7.3 MB/sec    1.00      3.5±0.02ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.6±0.09ms     3.8 MB/sec    1.01     10.7±0.11ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     5.9 MB/sec    1.00      2.9±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    317.6±1.49µs     9.3 MB/sec    1.00    318.7±1.28µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.03ms     5.3 MB/sec    1.00      4.9±0.03ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.01ms     7.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1117.1±5.53µs    14.9 MB/sec    1.00   1121.3±4.60µs    14.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    116.1±1.16µs    25.4 MB/sec    1.01    117.0±1.94µs    25.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.6 MB/sec    1.01      2.4±0.01ms    10.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.2±0.21ms     4.0 MB/sec    1.00     10.0±0.19ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1932.3±23.65µs     8.6 MB/sec    1.01  1947.9±28.30µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    215.2±8.42µs    13.7 MB/sec    1.02   219.7±11.17µs    13.4 MB/sec
formatter/pydantic/types.py                1.02      4.3±0.08ms     6.0 MB/sec    1.00      4.2±0.06ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.01     13.1±0.22ms     3.1 MB/sec    1.00     13.0±0.21ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.06ms     4.7 MB/sec    1.00      3.5±0.06ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    432.2±6.68µs     6.8 MB/sec    1.01    434.5±8.28µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.14ms     4.3 MB/sec    1.00      6.0±0.11ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.14ms     5.8 MB/sec    1.00      7.0±0.10ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1436.3±30.17µs    11.6 MB/sec    1.00  1436.7±20.92µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    162.6±6.63µs    18.1 MB/sec    1.00    160.6±4.62µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.04ms     8.2 MB/sec    1.00      3.1±0.04ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-07 16:04_

---

_Closed by @charliermarsh on 2023-08-07 16:04_

---

_Branch deleted on 2023-08-07 16:04_

---
