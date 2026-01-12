```yaml
number: 4350
title: Tweak capitalization of B021 message
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/f-string-docstring
created_at: 2023-05-10T15:35:43Z
updated_at: 2023-05-10T16:17:52Z
url: https://github.com/astral-sh/ruff/pull/4350
synced_at: 2026-01-12T03:56:39Z
```

# Tweak capitalization of B021 message

---

_Pull request opened by @charliermarsh on 2023-05-10 15:35_

_No description provided._

---

_Merged by @charliermarsh on 2023-05-10 15:59_

---

_Closed by @charliermarsh on 2023-05-10 15:59_

---

_Branch deleted on 2023-05-10 15:59_

---

_Comment by @github-actions[bot] on 2023-05-10 16:17_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     14.4±0.09ms     2.8 MB/sec    1.00     14.0±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.03ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.1±1.16µs     7.1 MB/sec    1.00    417.5±0.97µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.9±0.06ms     4.3 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.9±0.04ms     5.9 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1488.6±2.94µs    11.2 MB/sec    1.00  1477.0±10.79µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.2±0.32µs    18.1 MB/sec    1.00    162.9±0.17µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.01      5.5±0.01ms     7.4 MB/sec    1.00      5.5±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.01   1073.3±1.47µs    15.5 MB/sec    1.00   1065.6±0.67µs    15.6 MB/sec
parser/numpy/globals.py                    1.01    109.2±0.65µs    27.0 MB/sec    1.00    108.2±1.16µs    27.3 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.01ms    10.9 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.9±0.47ms     2.0 MB/sec    1.00     19.9±0.42ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.12ms     3.3 MB/sec    1.02      5.1±0.16ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   605.1±19.82µs     4.9 MB/sec    1.00   606.0±20.88µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.5±0.30ms     3.0 MB/sec    1.00      8.5±0.22ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.32ms     4.0 MB/sec    1.02     10.3±0.27ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.06ms     7.8 MB/sec    1.00      2.1±0.06ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   255.5±11.53µs    11.6 MB/sec    1.01   257.0±13.02µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.14ms     5.5 MB/sec    1.00      4.6±0.15ms     5.6 MB/sec
parser/large/dataset.py                    1.00      8.2±0.13ms     5.0 MB/sec    1.05      8.6±0.24ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.01  1642.2±44.41µs    10.1 MB/sec    1.00  1631.1±59.59µs    10.2 MB/sec
parser/numpy/globals.py                    1.00    165.0±8.28µs    17.9 MB/sec    1.00    165.3±6.02µs    17.9 MB/sec
parser/pydantic/types.py                   1.00      3.6±0.11ms     7.2 MB/sec    1.01      3.6±0.10ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
