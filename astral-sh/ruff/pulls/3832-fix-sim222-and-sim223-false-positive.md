```yaml
number: 3832
title: Fix SIM222 and SIM223 false positive
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-sim222-sim223-false-positive
created_at: 2023-03-31T17:19:22Z
updated_at: 2023-03-31T18:51:28Z
url: https://github.com/astral-sh/ruff/pull/3832
synced_at: 2026-01-12T04:28:19Z
```

# Fix SIM222 and SIM223 false positive

---

_Pull request opened by @JonathanPlasse on 2023-03-31 17:19_

- Close #3830

---

_@charliermarsh approved on 2023-03-31 17:28_

---

_@charliermarsh reviewed on 2023-03-31 17:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/ast_bool_op.rs`:516 on 2023-03-31 17:29_

Should we modify the fixtures, such that they would've caught this?

---

_Comment by @github-actions[bot] on 2023-03-31 17:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.13ms     2.8 MB/sec    1.00     14.6±0.08ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.05ms     4.4 MB/sec    1.00      3.8±0.02ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    405.1±5.20µs     7.3 MB/sec    1.01    410.7±6.99µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.04ms     4.0 MB/sec    1.00      6.3±0.08ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.01ms     5.2 MB/sec    1.00      7.9±0.05ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1732.0±5.92µs     9.6 MB/sec    1.00  1737.4±16.76µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.5±5.80µs    16.2 MB/sec    1.02    185.7±7.63µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     15.9±0.22ms     2.6 MB/sec    1.00     15.4±0.12ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.2±0.09ms     4.0 MB/sec    1.00      4.1±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.1±6.85µs     6.9 MB/sec    1.00    424.8±5.53µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.12ms     3.7 MB/sec    1.00      6.8±0.24ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.08ms     4.9 MB/sec    1.00      8.2±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1775.0±10.60µs     9.4 MB/sec    1.00   1777.5±8.98µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.5±0.91µs    15.9 MB/sec    1.00    186.3±4.69µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.8 MB/sec    1.00      3.8±0.06ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-31 18:50_

---

_Closed by @charliermarsh on 2023-03-31 18:50_

---

_Label `bug` added by @charliermarsh on 2023-03-31 18:50_

---

_Branch deleted on 2023-03-31 18:51_

---
