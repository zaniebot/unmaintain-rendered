```yaml
number: 3633
title: Add autofix for ANN204
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: autofix/ann204
created_at: 2023-03-20T18:35:32Z
updated_at: 2023-03-20T23:19:28Z
url: https://github.com/astral-sh/ruff/pull/3633
synced_at: 2026-01-12T15:55:13Z
```

# Add autofix for ANN204

---

_@JonathanPlasse_

- Related to #1640.
- Add autofix only for simple cases, incomplete cases would be adding `typing.Iterator` for `__iter__`.

---

_Comment by @github-actions[bot] on 2023-03-20 18:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.33ms     2.9 MB/sec    1.03     14.4±0.32ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.10ms     4.7 MB/sec    1.06      3.7±0.22ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    480.6±0.95µs     6.1 MB/sec    1.04    497.7±1.06µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.02      6.0±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.7 MB/sec    1.01      7.3±0.06ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1603.4±2.50µs    10.4 MB/sec    1.02   1634.9±4.17µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.4±0.60µs    16.6 MB/sec    1.05    185.8±0.43µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.02      3.4±0.04ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.08ms     2.8 MB/sec    1.00     14.7±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.1 MB/sec    1.00      4.0±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    521.6±4.90µs     5.7 MB/sec    1.01    526.5±5.55µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.05ms     3.9 MB/sec    1.01      6.6±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      8.2±0.22ms     4.9 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06  1894.3±19.98µs     8.8 MB/sec    1.00  1784.3±15.01µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.03    206.4±3.44µs    14.3 MB/sec    1.00    199.7±5.09µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.07      4.0±0.05ms     6.4 MB/sec    1.00      3.8±0.03ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@twoertwein reviewed on 2023-03-20 19:51_

---

_Review comment by @twoertwein on `crates/ruff_python_stdlib/src/typing.rs`:218 on 2023-03-20 19:51_

The Readme also lists `__exit__` and `__aexit__` but adding annotations for them might require import statements (for `TracebackType` and on older python versions also for `typing.Optional` and `typing.Type`).

---

_@JonathanPlasse reviewed on 2023-03-20 20:03_

---

_Review comment by @JonathanPlasse on `crates/ruff_python_stdlib/src/typing.rs`:218 on 2023-03-20 20:03_

This PR implements auto-fix for the return types of magic methods, not for the argument types.

---

_Comment by @charliermarsh on 2023-03-20 22:46_

Looks good, going to rerun the benchmark to see if that's noise...

---

_Merged by @charliermarsh on 2023-03-20 23:19_

---

_Closed by @charliermarsh on 2023-03-20 23:19_

---

_Branch deleted on 2023-03-20 23:19_

---
