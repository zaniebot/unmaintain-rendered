```yaml
number: 3774
title: Improve test performances
type: pull_request
state: closed
author: JonathanPlasse
labels: []
assignees: []
draft: true
base: main
head: improve-test-performance
created_at: 2023-03-28T15:55:49Z
updated_at: 2023-03-28T17:31:49Z
url: https://github.com/astral-sh/ruff/pull/3774
synced_at: 2026-01-12T04:39:45Z
```

# Improve test performances

---

_Pull request opened by @JonathanPlasse on 2023-03-28 15:55_

- Close #3752

This PR is to check if `nextest` is faster.

---

_Comment by @github-actions[bot] on 2023-03-28 16:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.08ms     2.9 MB/sec    1.04     14.5±0.10ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.02ms     4.7 MB/sec    1.02      3.6±0.02ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    462.2±1.24µs     6.4 MB/sec    1.02    472.9±0.98µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.03      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.7 MB/sec    1.07      7.7±0.02ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1628.5±7.40µs    10.2 MB/sec    1.04   1697.9±5.80µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.6±2.35µs    16.2 MB/sec    1.03    186.9±0.42µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.05      3.5±0.01ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.6±0.11ms     2.6 MB/sec    1.01     15.8±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.0 MB/sec    1.00      4.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    437.6±5.81µs     6.7 MB/sec    1.00    435.1±6.80µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.03ms     3.8 MB/sec    1.00      6.8±0.03ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.14ms     4.9 MB/sec    1.00      8.2±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1786.0±10.66µs     9.3 MB/sec    1.00  1785.3±10.62µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.8±1.82µs    15.8 MB/sec    1.01    188.3±3.48µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.03ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-03-28 17:20_

The tests are 13 seconds faster for Linux and 54 seconds slower for Windows.
It is not worth changing to `nextest`.
Closing.

---

_Closed by @JonathanPlasse on 2023-03-28 17:20_

---

_Branch deleted on 2023-03-28 17:31_

---
