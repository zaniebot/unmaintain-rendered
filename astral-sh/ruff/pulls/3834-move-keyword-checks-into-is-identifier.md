```yaml
number: 3834
title: "Move keyword checks into `is_identifier`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/kw
created_at: 2023-03-31T18:53:25Z
updated_at: 2023-03-31T20:56:35Z
url: https://github.com/astral-sh/ruff/pull/3834
synced_at: 2026-01-12T04:28:19Z
```

# Move keyword checks into `is_identifier`

---

_Pull request opened by @charliermarsh on 2023-03-31 18:53_

## Summary

Everywhere that we check `is_identifier`, we subsequently check whether the identifier is a keyword. You can't use keywords as identifiers, so I think this should just be part of that check.

---

_Comment by @charliermarsh on 2023-03-31 18:53_

\cc @JonathanPlasse - wdyt?

---

_Label `internal` added by @charliermarsh on 2023-03-31 18:53_

---

_Review comment by @charliermarsh on `crates/ruff_python_stdlib/src/keyword.rs`:2 on 2023-03-31 18:53_

(Can now be `pub(crate)`.)

---

_@charliermarsh reviewed on 2023-03-31 18:53_

---

_Comment by @github-actions[bot] on 2023-03-31 19:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.08ms     2.4 MB/sec    1.00     16.7±0.07ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     4.0 MB/sec    1.01      4.2±0.01ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    536.3±1.93µs     5.5 MB/sec    1.02    549.5±6.25µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.02ms     3.6 MB/sec    1.01      7.1±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.03ms     4.7 MB/sec    1.01      8.7±0.03ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1955.0±5.97µs     8.5 MB/sec    1.01   1975.9±4.49µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    218.1±2.58µs    13.5 MB/sec    1.00    218.8±1.33µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.01ms     6.4 MB/sec    1.01      4.0±0.02ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     21.5±0.91ms  1937.7 KB/sec    1.00     20.7±0.77ms  2015.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.15ms     3.1 MB/sec    1.01      5.5±0.31ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.04   676.0±57.40µs     4.4 MB/sec    1.00   649.9±24.29µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.34ms     2.8 MB/sec    1.01      9.1±0.57ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.01     11.0±0.29ms     3.7 MB/sec    1.00     10.9±0.30ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.5±0.15ms     6.7 MB/sec    1.00      2.4±0.14ms     6.8 MB/sec
linter/default-rules/numpy/globals.py      1.05   273.2±38.87µs    10.8 MB/sec    1.00   260.0±18.35µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      5.0±0.24ms     5.1 MB/sec    1.00      4.9±0.17ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-03-31 19:05_

It is less error-prone for future usage so I am all for it.

---

_Merged by @charliermarsh on 2023-03-31 20:56_

---

_Closed by @charliermarsh on 2023-03-31 20:56_

---

_Branch deleted on 2023-03-31 20:56_

---
