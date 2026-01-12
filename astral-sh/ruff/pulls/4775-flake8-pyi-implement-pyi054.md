```yaml
number: 4775
title: "[`flake8-pyi`] Implement PYI054"
type: pull_request
state: merged
author: density
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI054
created_at: 2023-06-01T03:52:49Z
updated_at: 2023-06-02T03:37:08Z
url: https://github.com/astral-sh/ruff/pull/4775
synced_at: 2026-01-12T03:50:03Z
```

# [`flake8-pyi`] Implement PYI054

---

_Pull request opened by @density on 2023-06-01 03:52_

## Summary

Implement Y054 from https://github.com/PyCQA/flake8-pyi: `Only numeric literals with a string representation <=10 characters long are permitted.`

## Test Plan

Added snapshot tests

ref: #848

---

_Marked ready for review by @density on 2023-06-01 03:52_

---

_@density reviewed on 2023-06-01 03:53_

---

_Review comment by @density on `crates/ruff/src/rules/flake8_pyi/rules/long_numeric_literals_in_stubs.rs`:15 on 2023-06-01 03:53_

Again, questionable reasoning.

---

_Comment by @github-actions[bot] on 2023-06-01 04:05_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>typeshed (+1, -0)</summary>
<p>

```diff
+ stdlib/_winapi.pyi:60:39: PYI054 Numeric literals with a string representation longer than ten characters are not permitted
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI054 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.3±0.11ms     2.8 MB/sec    1.00     14.3±0.15ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.7±0.65µs     7.0 MB/sec    1.00    423.3±2.09µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.04ms     4.3 MB/sec    1.00      5.9±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1504.0±2.85µs    11.1 MB/sec    1.00   1504.6±2.09µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.9±0.94µs    17.3 MB/sec    1.01    172.8±3.01µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.1±0.01ms     7.9 MB/sec    1.01      5.2±0.01ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1009.8±2.46µs    16.5 MB/sec    1.00   1013.0±0.53µs    16.4 MB/sec
parser/numpy/globals.py                    1.00    104.3±0.18µs    28.3 MB/sec    1.00    104.6±0.27µs    28.2 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.5 MB/sec    1.01      2.2±0.01ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.12ms     2.4 MB/sec    1.00     16.9±0.11ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.06ms     3.8 MB/sec    1.00      4.3±0.02ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    443.7±5.73µs     6.7 MB/sec    1.01    445.9±5.25µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.04ms     3.6 MB/sec    1.01      7.2±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.04ms     4.8 MB/sec    1.00      8.5±0.09ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1799.9±25.99µs     9.3 MB/sec    1.00  1782.6±12.55µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    196.6±1.79µs    15.0 MB/sec    1.00    194.9±3.22µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.04ms     6.6 MB/sec    1.00      3.8±0.03ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.7±0.03ms     6.1 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1267.6±7.15µs    13.1 MB/sec    1.00  1272.4±11.88µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    131.9±0.86µs    22.4 MB/sec    1.00    131.8±1.04µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.00      2.8±0.02ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-01 04:43_

---

_@density reviewed on 2023-06-01 04:48_

---

_Review comment by @density on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:144 on 2023-06-01 04:48_

Removed the checks on numeric literal length since it's now covered by PYI054.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/long_numeric_literals_in_stub.rs`:15 on 2023-06-01 06:18_

Can we add an example on how using ellipses looks? I wouldn't know what to do from reading the message itself.

---

_@MichaReiser approved on 2023-06-01 06:18_

---

_@density reviewed on 2023-06-01 13:12_

---

_Review comment by @density on `crates/ruff/src/rules/flake8_pyi/rules/long_numeric_literals_in_stub.rs`:15 on 2023-06-01 13:12_

@MichaReiser added an example and updated the comment - lmk if looks good. thanks!

---

_Label `rule` added by @charliermarsh on 2023-06-02 01:03_

---

_Merged by @charliermarsh on 2023-06-02 01:21_

---

_Closed by @charliermarsh on 2023-06-02 01:21_

---

_Branch deleted on 2023-06-02 03:37_

---
