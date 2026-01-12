```yaml
number: 4568
title: "Use `SemanticModel` in lieu of `Checker` in more methods"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/model
created_at: 2023-05-22T02:50:16Z
updated_at: 2023-05-22T08:33:27Z
url: https://github.com/astral-sh/ruff/pull/4568
synced_at: 2026-01-12T03:50:03Z
```

# Use `SemanticModel` in lieu of `Checker` in more methods

---

_Pull request opened by @charliermarsh on 2023-05-22 02:50_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-22 02:58_

---

_Closed by @charliermarsh on 2023-05-22 02:58_

---

_Branch deleted on 2023-05-22 02:58_

---

_Comment by @github-actions[bot] on 2023-05-22 03:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.03ms     2.9 MB/sec    1.00     14.1±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.00      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.3±0.45µs     6.9 MB/sec    1.00    427.3±0.75µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1465.4±2.98µs    11.4 MB/sec    1.00   1468.6±3.69µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.0±2.81µs    18.0 MB/sec    1.01    166.0±1.32µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.1±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.01      5.5±0.01ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1058.8±0.74µs    15.7 MB/sec    1.02   1075.9±0.89µs    15.5 MB/sec
parser/numpy/globals.py                    1.00    108.7±0.80µs    27.1 MB/sec    1.01    110.3±0.19µs    26.8 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.01      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.14ms     2.5 MB/sec    1.04     17.0±0.15ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.02      4.3±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   496.7±17.58µs     5.9 MB/sec    1.00    496.5±5.65µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.06ms     3.7 MB/sec    1.03      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     5.0 MB/sec    1.07      8.7±0.06ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1717.7±25.08µs     9.7 MB/sec    1.05  1797.5±16.00µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.3±2.92µs    15.3 MB/sec    1.05    203.2±6.07µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.05      3.9±0.03ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.7±0.05ms     6.1 MB/sec    1.00      6.7±0.05ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1268.8±18.05µs    13.1 MB/sec    1.01  1275.4±12.19µs    13.1 MB/sec
parser/numpy/globals.py                    1.01    132.0±2.37µs    22.3 MB/sec    1.00    131.3±1.64µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-05-22 08:33_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:781 on 2023-05-22 08:33_

@charliermarsh  Can we rename `model` here to `semantic_model`. I just reviewed a PR and was so confused by `self.model`. I was like, what's this? Where is this model coming from. It being `semantic_model` would make it clear (or `semantic` if you want to keep it short`)

---
