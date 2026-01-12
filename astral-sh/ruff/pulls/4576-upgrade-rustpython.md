```yaml
number: 4576
title: Upgrade RustPython
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
assignees: []
merged: true
base: main
head: upgrade-rust-python
created_at: 2023-05-22T11:45:49Z
updated_at: 2023-05-22T12:50:51Z
url: https://github.com/astral-sh/ruff/pull/4576
synced_at: 2026-01-12T15:55:15Z
```

# Upgrade RustPython

---

_@MichaReiser_

Pull in the improvements from https://github.com/RustPython/Parser/pull/60

---

_Label `performance` added by @MichaReiser on 2023-05-22 11:46_

---

_Comment by @github-actions[bot] on 2023-05-22 11:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     17.0±0.14ms     2.4 MB/sec    1.00     16.5±0.18ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.04ms     4.1 MB/sec    1.00      4.0±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02    505.5±5.61µs     5.8 MB/sec    1.00    494.3±3.90µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.0±0.05ms     3.6 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.03      8.1±0.04ms     5.0 MB/sec    1.00      7.8±0.06ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1754.0±14.10µs     9.5 MB/sec    1.00  1707.3±12.43µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.03    195.5±2.39µs    15.1 MB/sec    1.00    190.1±1.18µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.6±0.03ms     7.0 MB/sec    1.00      3.5±0.02ms     7.2 MB/sec
parser/large/dataset.py                    1.05      6.4±0.03ms     6.3 MB/sec    1.00      6.1±0.03ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.04   1270.0±4.86µs    13.1 MB/sec    1.00   1218.9±3.84µs    13.7 MB/sec
parser/numpy/globals.py                    1.06    130.4±1.21µs    22.6 MB/sec    1.00    123.1±1.23µs    24.0 MB/sec
parser/pydantic/types.py                   1.04      2.8±0.02ms     9.3 MB/sec    1.00      2.7±0.02ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.1±0.14ms     2.4 MB/sec    1.00     16.8±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.4±0.03ms     3.8 MB/sec    1.00      4.3±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.03    449.6±8.09µs     6.6 MB/sec    1.00    436.7±5.83µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.2±0.07ms     3.5 MB/sec    1.00      7.1±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.04      8.6±0.06ms     4.7 MB/sec    1.00      8.2±0.04ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1768.0±17.40µs     9.4 MB/sec    1.00  1722.7±15.01µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.02    189.8±3.25µs    15.5 MB/sec    1.00    185.8±4.27µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.03ms     6.6 MB/sec    1.00      3.8±0.04ms     6.8 MB/sec
parser/large/dataset.py                    1.04      7.0±0.04ms     5.8 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.03  1318.9±10.00µs    12.6 MB/sec    1.00   1278.8±8.51µs    13.0 MB/sec
parser/numpy/globals.py                    1.04    137.4±0.98µs    21.5 MB/sec    1.00    132.2±1.11µs    22.3 MB/sec
parser/pydantic/types.py                   1.02      2.9±0.02ms     8.7 MB/sec    1.00      2.9±0.02ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh approved on 2023-05-22 12:44_

---

_Comment by @MichaReiser on 2023-05-22 12:50_

Nice, a ~3% performance win.

---

_Merged by @MichaReiser on 2023-05-22 12:50_

---

_Closed by @MichaReiser on 2023-05-22 12:50_

---

_Branch deleted on 2023-05-22 12:50_

---
