```yaml
number: 4286
title: "Avoid fixing `PD002` in a lambda expression"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/PD002/no-assign-in-lambda
created_at: 2023-05-08T14:58:49Z
updated_at: 2023-05-08T23:15:09Z
url: https://github.com/astral-sh/ruff/pull/4286
synced_at: 2026-01-12T15:55:15Z
```

# Avoid fixing `PD002` in a lambda expression

---

_@dhruvmanila_

fixes: #4277

---

_Comment by @github-actions[bot] on 2023-05-08 15:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.1±0.03ms     2.9 MB/sec    1.00     13.8±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.3±0.00ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    418.8±1.50µs     7.0 MB/sec    1.00    414.9±0.92µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.9±0.01ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      7.0±0.04ms     5.8 MB/sec    1.00      6.8±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1505.2±10.11µs    11.1 MB/sec    1.00   1479.4±2.92µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01   164.3±10.11µs    18.0 MB/sec    1.00    162.5±0.23µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.5 MB/sec    1.01      5.5±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1053.9±0.69µs    15.8 MB/sec    1.01   1060.4±1.34µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    106.8±0.27µs    27.6 MB/sec    1.00    107.3±0.16µs    27.5 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.01ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.05ms     2.5 MB/sec    1.01     16.6±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.17ms     3.9 MB/sec    1.00      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    432.5±6.44µs     6.8 MB/sec    1.00    429.6±5.05µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.09ms     3.6 MB/sec    1.00      7.1±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.05ms     4.8 MB/sec    1.00      8.5±0.08ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1781.2±19.21µs     9.3 MB/sec    1.01   1793.2±9.39µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.6±2.51µs    15.4 MB/sec    1.01    192.5±3.46µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.7 MB/sec    1.00      3.8±0.02ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.7±0.02ms     6.0 MB/sec    1.04      7.0±0.06ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1285.0±7.42µs    13.0 MB/sec    1.03   1320.7±8.85µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    133.0±1.26µs    22.2 MB/sec    1.01    134.6±1.12µs    21.9 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.03      2.9±0.01ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-08 18:10_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:87 on 2023-05-08 18:10_

Ah yeah, the existing checks actually _should_ catch this, but it has to do with a limitation in the context we "restore" when parsing deferred bodies (like lambda bodies). In short, we don't restore the expression stack, since it's expensive to track all expressions, and I hadn't realized it was necessary, but it is here.


---

_@charliermarsh reviewed on 2023-05-08 18:10_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:87 on 2023-05-08 18:10_

I'll add a TODO and merge.

---

_Merged by @charliermarsh on 2023-05-08 18:24_

---

_Closed by @charliermarsh on 2023-05-08 18:24_

---

_@dhruvmanila reviewed on 2023-05-08 23:14_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:87 on 2023-05-08 23:14_

Ah right. I wondered why (2) was not catching it but didn't have time to dig deeper 😅

---

_Branch deleted on 2023-05-08 23:15_

---
