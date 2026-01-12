```yaml
number: 4405
title: Enable autofix for split-assertions at top level
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/split-assert
created_at: 2023-05-12T21:35:44Z
updated_at: 2023-05-12T22:03:51Z
url: https://github.com/astral-sh/ruff/pull/4405
synced_at: 2026-01-12T15:55:15Z
```

# Enable autofix for split-assertions at top level

---

_@charliermarsh_

It looks like the `PT018` fix _assumed_ that the assertions would be within a function or otherwise not at the top level. This PR addresses that oversight, borrowing patterns from other existing autofixes.

Closes #4403.

---

_Merged by @charliermarsh on 2023-05-12 21:35_

---

_Closed by @charliermarsh on 2023-05-12 21:35_

---

_Branch deleted on 2023-05-12 21:35_

---

_Comment by @github-actions[bot] on 2023-05-12 21:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.13ms     2.5 MB/sec    1.00     16.2±0.11ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    483.3±3.71µs     6.1 MB/sec    1.00    483.9±3.38µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.05ms     3.8 MB/sec    1.00      6.8±0.04ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.05ms     5.2 MB/sec    1.00      7.9±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1696.1±12.11µs     9.8 MB/sec    1.01   1710.9±5.62µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.7±2.30µs    15.6 MB/sec    1.01    191.1±1.95µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.01      3.6±0.02ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.4±0.05ms     6.4 MB/sec    1.01      6.4±0.06ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00   1239.0±9.17µs    13.4 MB/sec    1.00   1243.5±7.47µs    13.4 MB/sec
parser/numpy/globals.py                    1.00    125.5±1.13µs    23.5 MB/sec    1.00    125.9±0.96µs    23.4 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.01ms     9.4 MB/sec    1.01      2.7±0.01ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.13ms     2.5 MB/sec    1.00     16.1±0.14ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.06ms     4.0 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    490.8±5.78µs     6.0 MB/sec    1.00    485.3±4.69µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.08ms     3.7 MB/sec    1.00      6.8±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.06ms     5.0 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1725.6±17.37µs     9.6 MB/sec    1.00  1721.3±14.15µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.2±2.77µs    15.2 MB/sec    1.02    197.3±6.40µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     7.0 MB/sec    1.01      3.7±0.05ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.8±0.04ms     6.0 MB/sec    1.00      6.7±0.04ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1278.7±11.49µs    13.0 MB/sec    1.00  1275.4±12.38µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    130.1±2.17µs    22.7 MB/sec    1.00    130.4±2.15µs    22.6 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.03ms     8.9 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
