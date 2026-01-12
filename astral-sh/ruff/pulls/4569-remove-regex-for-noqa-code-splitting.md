```yaml
number: 4569
title: Remove regex for noqa code splitting
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/split-regex
created_at: 2023-05-22T03:08:01Z
updated_at: 2023-05-22T05:06:07Z
url: https://github.com/astral-sh/ruff/pull/4569
synced_at: 2026-01-12T03:50:03Z
```

# Remove regex for noqa code splitting

---

_Pull request opened by @charliermarsh on 2023-05-22 03:08_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-05-22 03:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.04ms     2.9 MB/sec    1.01     14.3±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.01      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.1±0.87µs     6.9 MB/sec    1.01    429.6±0.66µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.01      6.0±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1470.2±1.29µs    11.3 MB/sec    1.00   1476.7±6.14µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.2±0.25µs    18.1 MB/sec    1.01    164.5±0.22µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.02      3.1±0.02ms     8.2 MB/sec
parser/large/dataset.py                    1.01      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.00ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.01   1066.0±0.75µs    15.6 MB/sec    1.00   1057.0±2.83µs    15.8 MB/sec
parser/numpy/globals.py                    1.01    109.9±0.23µs    26.9 MB/sec    1.00    108.3±0.16µs    27.3 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.01ms    11.0 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     17.3±0.17ms     2.4 MB/sec    1.00     16.8±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.10ms     3.8 MB/sec    1.00      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    508.0±7.66µs     5.8 MB/sec    1.00    501.8±6.98µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.2±0.09ms     3.5 MB/sec    1.00      7.1±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.5±0.08ms     4.8 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1790.4±19.53µs     9.3 MB/sec    1.00  1752.3±29.95µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.0±5.19µs    14.8 MB/sec    1.01    201.7±8.05µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.8 MB/sec    1.00      3.7±0.06ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.8±0.05ms     6.0 MB/sec    1.13      7.7±0.06ms     5.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1286.1±15.96µs    12.9 MB/sec    1.11  1426.8±17.33µs    11.7 MB/sec
parser/numpy/globals.py                    1.00    132.2±2.44µs    22.3 MB/sec    1.08    142.7±2.28µs    20.7 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.03ms     8.9 MB/sec    1.12      3.2±0.04ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-22 03:20_

---

_Closed by @charliermarsh on 2023-05-22 03:20_

---

_Branch deleted on 2023-05-22 03:20_

---

_Review comment by @MichaReiser on `crates/ruff/src/noqa.rs`:108 on 2023-05-22 05:05_

Our use of is_whitespace throughout the code base is probably incorrect because it, to my knowledge, includes Unicode whitespace that the python syntax does not support 

---

_@MichaReiser reviewed on 2023-05-22 05:06_

---
