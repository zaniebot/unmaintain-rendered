```yaml
number: 4469
title: Add PLW docs
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: plw-docs
created_at: 2023-05-17T12:15:35Z
updated_at: 2023-07-10T09:55:36Z
url: https://github.com/astral-sh/ruff/pull/4469
synced_at: 2026-01-12T03:36:54Z
```

# Add PLW docs

---

_Pull request opened by @tjkuson on 2023-05-17 12:15_

Add documentation to the Pylint Warning (PLW) rules. Complete the PLW ruleset.

Related to #2646.

---

_Comment by @github-actions[bot] on 2023-05-17 12:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.13     14.1±0.05ms     2.9 MB/sec    1.00     12.5±0.04ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.13      3.4±0.01ms     4.9 MB/sec    1.00      3.0±0.01ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.2±0.50µs     6.9 MB/sec    1.00    427.9±0.42µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.13      5.8±0.02ms     4.4 MB/sec    1.00      5.2±0.01ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1470.6±3.81µs    11.3 MB/sec    1.00   1462.5±2.97µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    144.3±0.33µs    20.4 MB/sec    1.13    163.3±0.53µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.14      5.4±0.01ms     7.6 MB/sec    1.00      4.7±0.00ms     8.7 MB/sec
parser/numpy/ctypeslib.py                  1.13   1053.1±2.20µs    15.8 MB/sec    1.00    928.1±0.93µs    17.9 MB/sec
parser/numpy/globals.py                    1.00    108.2±0.19µs    27.3 MB/sec    1.00    108.3±0.19µs    27.2 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.2 MB/sec    1.00      2.3±0.07ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.2±0.35ms     2.4 MB/sec    1.00     17.1±0.29ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.06ms     3.9 MB/sec    1.00      4.3±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   509.3±10.13µs     5.8 MB/sec    1.00    504.9±8.55µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.15ms     3.6 MB/sec    1.02      7.2±0.18ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.08ms     4.8 MB/sec    1.01      8.5±0.09ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1777.9±30.63µs     9.4 MB/sec    1.01  1802.6±38.52µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.4±6.06µs    14.7 MB/sec    1.00    201.2±5.64µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.8 MB/sec    1.01      3.8±0.06ms     6.7 MB/sec
parser/large/dataset.py                    1.01      6.8±0.08ms     6.0 MB/sec    1.00      6.8±0.10ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1291.9±26.54µs    12.9 MB/sec    1.00  1287.4±16.26µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    131.8±3.55µs    22.4 MB/sec    1.00    131.9±2.58µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.9 MB/sec    1.00      2.9±0.05ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-05-17 18:21_

---

_Comment by @charliermarsh on 2023-05-17 18:21_

Thank you!

---

_Merged by @charliermarsh on 2023-05-17 18:30_

---

_Closed by @charliermarsh on 2023-05-17 18:30_

---

_Branch deleted on 2023-07-10 09:55_

---
