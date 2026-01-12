```yaml
number: 4804
title: Switch PyFormatter lifetimes
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: pyformatter_lifetimes
created_at: 2023-06-02T08:11:43Z
updated_at: 2023-06-02T10:26:40Z
url: https://github.com/astral-sh/ruff/pull/4804
synced_at: 2026-01-12T03:50:03Z
```

# Switch PyFormatter lifetimes

---

_Pull request opened by @konstin on 2023-06-02 08:11_

Stylistic change to have the input lifetime first and the output lifetime second. I'll rebase my other PR on top of this.

Test plan: `cargo clippy`

---

_Review requested from @MichaReiser by @konstin on 2023-06-02 08:11_

---

_Comment by @github-actions[bot] on 2023-06-02 08:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.2±0.04ms     2.9 MB/sec    1.00     14.0±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    427.2±0.81µs     6.9 MB/sec    1.00    424.1±0.91µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.02ms     4.3 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.02      6.9±0.01ms     5.9 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1520.2±2.91µs    11.0 MB/sec    1.00   1504.7±4.31µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.9±0.28µs    17.3 MB/sec    1.00    170.1±0.53µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.1 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.01      5.2±0.01ms     7.8 MB/sec    1.00      5.1±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.01   1016.7±4.44µs    16.4 MB/sec    1.00   1010.5±0.54µs    16.5 MB/sec
parser/numpy/globals.py                    1.00    104.9±0.17µs    28.1 MB/sec    1.00    104.9±0.11µs    28.1 MB/sec
parser/pydantic/types.py                   1.01      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.00ms    11.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.8±0.29ms     2.4 MB/sec    1.00     16.9±0.24ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     3.9 MB/sec    1.01      4.3±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    505.8±9.28µs     5.8 MB/sec    1.00   505.8±15.41µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.14ms     3.6 MB/sec    1.00      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.08ms     5.0 MB/sec    1.01      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1761.0±38.15µs     9.5 MB/sec    1.01  1773.1±21.79µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.0±4.32µs    14.6 MB/sec    1.04   210.0±22.46µs    14.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.8 MB/sec    1.02      3.8±0.09ms     6.7 MB/sec
parser/large/dataset.py                    1.01      6.4±0.06ms     6.3 MB/sec    1.00      6.4±0.04ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.01  1213.3±38.73µs    13.7 MB/sec    1.00  1198.5±21.08µs    13.9 MB/sec
parser/numpy/globals.py                    1.01    124.8±3.24µs    23.6 MB/sec    1.00    124.0±2.69µs    23.8 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.04ms     9.3 MB/sec    1.00      2.7±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-06-02 08:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/lib.rs`:35 on 2023-06-02 08:49_

Isn't it necessary to update existing usages of `PyFormatter` that specify a lifetime?

---

_@MichaReiser reviewed on 2023-06-02 08:49_

---

_@konstin reviewed on 2023-06-02 09:02_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/lib.rs`:35 on 2023-06-02 09:02_

surprisingly they all seem to be inferred atm

---

_Merged by @konstin on 2023-06-02 10:26_

---

_Closed by @konstin on 2023-06-02 10:26_

---

_Branch deleted on 2023-06-02 10:26_

---
