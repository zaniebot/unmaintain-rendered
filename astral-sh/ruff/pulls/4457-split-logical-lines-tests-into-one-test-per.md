```yaml
number: 4457
title: Split logical lines tests into one test per assertion
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/split-tests
created_at: 2023-05-16T17:32:59Z
updated_at: 2023-05-16T17:59:40Z
url: https://github.com/astral-sh/ruff/pull/4457
synced_at: 2026-01-12T15:55:15Z
```

# Split logical lines tests into one test per assertion

---

_@charliermarsh_

Also moved the tests into the module defining `LogicalLines::from_tokens`.

---

_Label `internal` added by @charliermarsh on 2023-05-16 17:33_

---

_@MichaReiser approved on 2023-05-16 17:37_

---

_Merged by @charliermarsh on 2023-05-16 17:40_

---

_Closed by @charliermarsh on 2023-05-16 17:40_

---

_Branch deleted on 2023-05-16 17:40_

---

_Comment by @github-actions[bot] on 2023-05-16 17:41_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.01ms     2.9 MB/sec    1.00     13.9±0.01ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.2±0.82µs     6.9 MB/sec    1.00    426.5±1.31µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.0 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1461.3±4.26µs    11.4 MB/sec    1.00   1452.0±0.94µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.6±0.41µs    17.9 MB/sec    1.00    164.3±2.61µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.3±0.00ms     7.6 MB/sec    1.00      5.3±0.00ms     7.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1050.4±0.31µs    15.9 MB/sec    1.00   1048.3±0.61µs    15.9 MB/sec
parser/numpy/globals.py                    1.00    107.8±0.21µs    27.4 MB/sec    1.00    108.0±0.19µs    27.3 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.2 MB/sec    1.00      2.3±0.00ms    11.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.12ms     2.4 MB/sec    1.02     17.1±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     3.9 MB/sec    1.02      4.3±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.0±6.57µs     5.9 MB/sec    1.01    507.4±9.84µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.16ms     3.6 MB/sec    1.02      7.2±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     4.9 MB/sec    1.08      8.9±0.14ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1728.1±17.31µs     9.6 MB/sec    1.06  1832.1±27.63µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.8±2.51µs    15.0 MB/sec    1.06    208.0±6.34µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     7.0 MB/sec    1.08      3.9±0.06ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.6±0.04ms     6.1 MB/sec    1.00      6.6±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1267.9±13.97µs    13.1 MB/sec    1.00  1264.9±17.47µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    130.5±1.62µs    22.6 MB/sec    1.00    130.9±1.69µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
