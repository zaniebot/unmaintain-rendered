```yaml
number: 6348
title: "Make the `statement` vector private on `SemanticModel`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/statement
created_at: 2023-08-04T17:33:39Z
updated_at: 2023-08-07T15:21:09Z
url: https://github.com/astral-sh/ruff/pull/6348
synced_at: 2026-01-12T15:55:21Z
```

# Make the `statement` vector private on `SemanticModel`

---

_@charliermarsh_

## Summary

Instead, expose these as methods, now that we can use a reasonable nomenclature on the API.


---

_Label `internal` added by @charliermarsh on 2023-08-04 17:33_

---

_@MichaReiser reviewed on 2023-08-04 17:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:941 on 2023-08-04 17:37_

Should this be `statement_parent` to be consistent with `scope_parent`?

---

_@charliermarsh reviewed on 2023-08-04 17:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:941 on 2023-08-04 17:37_

Yes, I did this before I changed the prior PR.

---

_@charliermarsh reviewed on 2023-08-04 18:01_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:941 on 2023-08-04 18:01_

(Settled on using `parent_scope` and `parent_statement`.)

---

_Comment by @github-actions[bot] on 2023-08-04 18:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.4±0.45ms     3.6 MB/sec    1.01     11.5±0.46ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.3±0.23ms     7.3 MB/sec    1.00      2.2±0.14ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00   247.5±18.59µs    11.9 MB/sec    1.03   254.9±18.48µs    11.6 MB/sec
formatter/pydantic/types.py                1.01      4.8±0.26ms     5.3 MB/sec    1.00      4.8±0.23ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.03     15.9±0.58ms     2.6 MB/sec    1.00     15.4±0.52ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.1±0.23ms     4.1 MB/sec    1.00      3.9±0.23ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   560.3±27.66µs     5.3 MB/sec    1.00   554.6±31.79µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.29ms     3.6 MB/sec    1.00      7.0±0.30ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.25ms     5.0 MB/sec    1.00      8.1±0.41ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1639.1±83.23µs    10.2 MB/sec    1.00  1627.7±62.02µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.02   203.1±14.64µs    14.5 MB/sec    1.00   199.7±12.01µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.16ms     7.3 MB/sec    1.00      3.5±0.11ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.6±0.07ms     4.2 MB/sec    1.01      9.7±0.07ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1877.9±23.43µs     8.9 MB/sec    1.00  1881.7±19.59µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    210.7±4.76µs    14.0 MB/sec    1.03   216.9±21.61µs    13.6 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.06ms     6.3 MB/sec    1.01      4.1±0.12ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.10ms     3.2 MB/sec    1.00     12.7±0.09ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.8 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    430.3±5.12µs     6.9 MB/sec    1.01   436.5±29.31µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.07ms     4.4 MB/sec    1.00      5.8±0.06ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.00      6.9±0.08ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1415.5±13.86µs    11.8 MB/sec    1.01  1427.3±15.70µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.6±2.36µs    17.9 MB/sec    1.01    165.4±3.78µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.05ms     8.4 MB/sec    1.01      3.1±0.04ms     8.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-08-07 07:58_

---

_Merged by @charliermarsh on 2023-08-07 15:02_

---

_Closed by @charliermarsh on 2023-08-07 15:02_

---

_Branch deleted on 2023-08-07 15:02_

---
