```yaml
number: 4733
title: Add Rust toolchain as documentation dependency in CONTRIBUTING.md
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/rust
created_at: 2023-05-30T17:30:45Z
updated_at: 2023-05-30T17:59:40Z
url: https://github.com/astral-sh/ruff/pull/4733
synced_at: 2026-01-12T03:50:03Z
```

# Add Rust toolchain as documentation dependency in CONTRIBUTING.md

---

_Pull request opened by @charliermarsh on 2023-05-30 17:30_

Closes #4719.

---

_Label `documentation` added by @charliermarsh on 2023-05-30 17:30_

---

_Merged by @charliermarsh on 2023-05-30 17:39_

---

_Closed by @charliermarsh on 2023-05-30 17:39_

---

_Branch deleted on 2023-05-30 17:39_

---

_Comment by @github-actions[bot] on 2023-05-30 17:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.4±0.07ms     2.6 MB/sec    1.00     15.2±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.01ms     4.5 MB/sec    1.00      3.7±0.01ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.01    384.3±6.19µs     7.7 MB/sec    1.00    379.1±5.02µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.05ms     4.0 MB/sec    1.00      6.3±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.03      7.8±0.02ms     5.2 MB/sec    1.00      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1650.5±3.42µs    10.1 MB/sec    1.00   1607.3±4.08µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    179.0±0.44µs    16.5 MB/sec    1.00    175.8±0.81µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.5±0.01ms     7.3 MB/sec    1.00      3.4±0.02ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.7±0.01ms     7.1 MB/sec    1.01      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1129.3±5.54µs    14.7 MB/sec    1.00   1132.0±0.90µs    14.7 MB/sec
parser/numpy/globals.py                    1.00    115.7±0.24µs    25.5 MB/sec    1.00    116.0±0.48µs    25.4 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.4 MB/sec    1.01      2.5±0.02ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.4±0.56ms  2043.0 KB/sec    1.00     20.4±0.54ms  2046.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.2±0.21ms     3.2 MB/sec    1.00      5.2±0.16ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   608.5±19.01µs     4.8 MB/sec    1.01   615.5±27.58µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.27ms     3.0 MB/sec    1.01      8.7±0.34ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.33ms     4.0 MB/sec    1.00     10.2±0.32ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.05ms     7.8 MB/sec    1.01      2.2±0.08ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.01   253.7±18.21µs    11.6 MB/sec    1.00   251.1±10.65µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.16ms     5.5 MB/sec    1.01      4.6±0.15ms     5.5 MB/sec
parser/large/dataset.py                    1.00      7.7±0.25ms     5.3 MB/sec    1.05      8.1±0.24ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1506.9±67.06µs    11.0 MB/sec    1.02  1533.7±59.84µs    10.9 MB/sec
parser/numpy/globals.py                    1.00    150.0±6.23µs    19.7 MB/sec    1.03    154.0±8.39µs    19.2 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.09ms     7.7 MB/sec    1.02      3.4±0.12ms     7.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
