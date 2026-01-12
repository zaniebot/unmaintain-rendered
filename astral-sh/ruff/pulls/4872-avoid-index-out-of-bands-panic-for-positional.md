```yaml
number: 4872
title: Avoid index-out-of-bands panic for positional placeholders
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/F523
created_at: 2023-06-05T18:24:38Z
updated_at: 2023-06-05T18:58:25Z
url: https://github.com/astral-sh/ruff/pull/4872
synced_at: 2026-01-12T15:55:16Z
```

# Avoid index-out-of-bands panic for positional placeholders

---

_@charliermarsh_

Closes #4863.

---

_Merged by @charliermarsh on 2023-06-05 18:31_

---

_Closed by @charliermarsh on 2023-06-05 18:31_

---

_Branch deleted on 2023-06-05 18:31_

---

_Comment by @github-actions[bot] on 2023-06-05 18:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     14.3±0.14ms     2.8 MB/sec    1.00     13.8±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.4±1.24µs     7.2 MB/sec    1.02    417.7±0.91µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.05ms     4.4 MB/sec    1.01      5.9±0.09ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.01ms     6.0 MB/sec    1.00      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1457.0±1.89µs    11.4 MB/sec    1.01   1470.1±3.29µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.4±0.88µs    18.2 MB/sec    1.01    164.7±1.48µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.4 MB/sec    1.00      3.0±0.00ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.8 MB/sec    1.02      5.3±0.00ms     7.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1022.5±0.54µs    16.3 MB/sec    1.01   1032.7±4.68µs    16.1 MB/sec
parser/numpy/globals.py                    1.00    107.1±0.91µs    27.6 MB/sec    1.02    108.7±0.21µs    27.2 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.01      2.3±0.01ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.9±0.13ms     2.4 MB/sec    1.00     16.8±0.20ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.9 MB/sec    1.00      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.6±5.44µs     6.9 MB/sec    1.00    429.7±6.53µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.06ms     3.5 MB/sec    1.00      7.2±0.13ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.05ms     4.8 MB/sec    1.01      8.5±0.07ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1757.0±25.98µs     9.5 MB/sec    1.03  1810.3±56.00µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.7±4.03µs    15.7 MB/sec    1.01    189.8±3.91µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.7 MB/sec    1.00      3.8±0.03ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.7±0.09ms     6.1 MB/sec    1.01      6.8±0.07ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1272.3±10.87µs    13.1 MB/sec    1.00   1276.9±7.98µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    132.4±2.56µs    22.3 MB/sec    1.00    132.8±1.61µs    22.2 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.05ms     9.0 MB/sec    1.01      2.9±0.02ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
