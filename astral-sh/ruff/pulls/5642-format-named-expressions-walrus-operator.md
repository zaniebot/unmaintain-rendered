```yaml
number: 5642
title: Format named expressions (walrus operator)
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: named_expr_stash
created_at: 2023-07-10T11:52:05Z
updated_at: 2023-07-10T12:56:25Z
url: https://github.com/astral-sh/ruff/pull/5642
synced_at: 2026-01-12T15:55:19Z
```

# Format named expressions (walrus operator)

---

_@konstin_

## Summary

Format named expressions (walrus operator) such a `value := f()`. 

Unlike tuples, named expression parentheses are not part of the range even when mandatory, so mapping optional parentheses to always gives us decent formatting without implementing all [PEP 572](https://peps.python.org/pep-0572/) rules on when we need parentheses where other expressions wouldn't. We might want to revisit this decision later and implement special cases, but for now this gives us what we need.

## Test Plan

black fixtures, i added some fixtures and checked django and cpython for stability.

Closes #5613

---

_@MichaReiser approved on 2023-07-10 12:08_

---

_@MichaReiser reviewed on 2023-07-10 12:13_

Hmm, this shouldn't be necessary at all. Let me check our test setup. It should automatically delete fixed snapshots

---

_Merged by @konstin on 2023-07-10 12:32_

---

_Closed by @konstin on 2023-07-10 12:32_

---

_Branch deleted on 2023-07-10 12:32_

---

_Comment by @github-actions[bot] on 2023-07-10 12:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.5±0.01ms     4.8 MB/sec    1.00      8.5±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1799.7±4.08µs     9.3 MB/sec    1.01   1810.7±3.80µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    198.5±1.17µs    14.9 MB/sec    1.01    199.7±1.68µs    14.8 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.01ms     6.2 MB/sec    1.00      4.1±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.03ms     2.9 MB/sec    1.00     14.1±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.7 MB/sec    1.00      3.5±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.9±1.37µs     8.1 MB/sec    1.00    362.7±0.79µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.02ms     4.1 MB/sec    1.01      6.3±0.05ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.6 MB/sec    1.00      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1480.9±11.19µs    11.2 MB/sec    1.00   1473.6±1.92µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.0±0.39µs    18.8 MB/sec    1.00    156.3±0.20µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.01ms     7.9 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.3±0.61ms     3.3 MB/sec    1.06     13.1±0.39ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.10ms     6.4 MB/sec    1.11      2.9±0.13ms     5.7 MB/sec
formatter/numpy/globals.py                 1.00   305.1±23.08µs     9.7 MB/sec    1.06   324.7±23.77µs     9.1 MB/sec
formatter/pydantic/types.py                1.00      5.6±0.23ms     4.5 MB/sec    1.14      6.4±0.22ms     4.0 MB/sec
linter/all-rules/large/dataset.py          1.00     20.8±0.81ms  2006.2 KB/sec    1.02     21.2±0.48ms  1962.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.20ms     3.2 MB/sec    1.04      5.4±0.17ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   625.7±35.54µs     4.7 MB/sec    1.06   660.3±23.72µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.29ms     2.9 MB/sec    1.08      9.6±0.36ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     10.5±0.30ms     3.9 MB/sec    1.11     11.7±0.36ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.09ms     7.4 MB/sec    1.11      2.5±0.09ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   258.0±12.35µs    11.4 MB/sec    1.10   283.9±10.65µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.27ms     5.2 MB/sec    1.06      5.2±0.21ms     4.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
