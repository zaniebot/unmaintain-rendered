```yaml
number: 3833
title: Fix pre-commit CI job exit code
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - internal
assignees: []
merged: true
base: main
head: fix-pre-commit-ci
created_at: 2023-03-31T18:29:28Z
updated_at: 2023-03-31T18:56:28Z
url: https://github.com/astral-sh/ruff/pull/3833
synced_at: 2026-01-12T04:28:19Z
```

# Fix pre-commit CI job exit code

---

_Pull request opened by @JonathanPlasse on 2023-03-31 18:29_

The pre-commit job always exited with 0.
Fix scripts/add_rule.py

---

_@charliermarsh reviewed on 2023-03-31 18:29_

---

_Review comment by @charliermarsh on `scripts/add_rule.py`:18 on 2023-03-31 18:29_

Let's turn this rule off rather than ignore.

---

_Comment by @github-actions[bot] on 2023-03-31 18:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.93ms     2.5 MB/sec    1.31     21.2±1.38ms  1968.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.20ms     4.2 MB/sec    1.24      5.0±0.30ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   548.6±51.54µs     5.4 MB/sec    1.00   542.0±29.35µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.31ms     3.6 MB/sec    1.00      7.0±0.43ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.39ms     4.7 MB/sec    1.04      9.0±0.44ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.08ms     8.3 MB/sec    1.01      2.0±0.12ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.14   272.8±17.15µs    10.8 MB/sec    1.00   240.3±16.46µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.09      4.5±0.22ms     5.7 MB/sec    1.00      4.1±0.22ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.25ms     2.5 MB/sec    1.01     16.5±0.40ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.13ms     4.0 MB/sec    1.04      4.4±0.08ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.5±8.64µs     5.9 MB/sec    1.02   512.0±15.40µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.13ms     3.7 MB/sec    1.04      7.1±0.17ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.11ms     4.8 MB/sec    1.00      8.5±0.14ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1846.6±21.54µs     9.0 MB/sec    1.00  1837.1±21.34µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    199.9±5.25µs    14.8 MB/sec    1.01    201.6±7.32µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.04ms     6.6 MB/sec    1.00      3.8±0.04ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-31 18:47_

---

_Closed by @charliermarsh on 2023-03-31 18:47_

---

_Label `internal` added by @charliermarsh on 2023-03-31 18:47_

---

_Branch deleted on 2023-03-31 18:50_

---
