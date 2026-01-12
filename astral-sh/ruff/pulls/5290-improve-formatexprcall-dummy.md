```yaml
number: 5290
title: Improve FormatExprCall dummy
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: better_expr_call_dummy
created_at: 2023-06-22T08:12:48Z
updated_at: 2023-06-22T08:59:32Z
url: https://github.com/astral-sh/ruff/pull/5290
synced_at: 2026-01-12T15:55:18Z
```

# Improve FormatExprCall dummy

---

_@konstin_

This solves an instability when formatting cpython. It also introduces another one, but i think it's still a worthwhile change for now.

There's no proper testing since this is just a dummy.

---

_Label `internal` added by @konstin on 2023-06-22 08:12_

---

_Review requested from @MichaReiser by @konstin on 2023-06-22 08:13_

---

_Label `formatter` added by @konstin on 2023-06-22 08:13_

---

_@MichaReiser approved on 2023-06-22 08:29_

---

_Comment by @github-actions[bot] on 2023-06-22 08:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.03ms     5.2 MB/sec    1.01      7.9±0.05ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   1677.2±9.85µs     9.9 MB/sec    1.00  1668.8±16.95µs    10.0 MB/sec
formatter/numpy/globals.py                 1.01    159.2±2.17µs    18.5 MB/sec    1.00    157.8±2.21µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.05ms     7.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.04ms     2.6 MB/sec    1.06     16.7±0.05ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.04      4.1±0.01ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    508.9±1.42µs     5.8 MB/sec    1.05    533.0±5.10µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.01ms     3.7 MB/sec    1.06      7.3±0.03ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.02ms     5.2 MB/sec    1.15      9.0±0.03ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1742.6±6.45µs     9.6 MB/sec    1.10   1912.8±8.18µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.7±1.23µs    15.0 MB/sec    1.07    210.6±2.85µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.11      4.0±0.01ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.10ms     5.3 MB/sec    1.01      7.8±0.13ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1503.4±13.62µs    11.1 MB/sec    1.01  1521.5±19.30µs    10.9 MB/sec
formatter/numpy/globals.py                 1.00    143.2±2.25µs    20.6 MB/sec    1.01    144.6±3.69µs    20.4 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.08ms     8.3 MB/sec    1.01      3.1±0.06ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.02     15.1±0.24ms     2.7 MB/sec    1.00     14.9±0.17ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.8±0.07ms     4.4 MB/sec    1.00      3.7±0.06ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    408.3±6.87µs     7.2 MB/sec    1.00    406.9±6.02µs     7.3 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.6±0.09ms     3.9 MB/sec    1.00      6.4±0.11ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.02      7.7±0.13ms     5.3 MB/sec    1.00      7.6±0.09ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1518.0±13.46µs    11.0 MB/sec    1.00  1522.9±12.92µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    168.5±3.20µs    17.5 MB/sec    1.00    167.7±2.91µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.7 MB/sec    1.00      3.3±0.04ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-06-22 08:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_call.rs`:14 on 2023-06-22 08:46_

We could also consider correctly implementing the no-arguments case rather than stubbing it out. The only "challenge" is to handle dangling comments.

---

_Merged by @konstin on 2023-06-22 08:59_

---

_Closed by @konstin on 2023-06-22 08:59_

---

_Branch deleted on 2023-06-22 08:59_

---
