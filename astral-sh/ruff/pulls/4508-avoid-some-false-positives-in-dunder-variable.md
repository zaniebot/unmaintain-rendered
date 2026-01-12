```yaml
number: 4508
title: Avoid some false positives in dunder variable assigments
type: pull_request
state: merged
author: scop
labels: []
assignees: []
merged: true
base: main
head: fix/dunder-assignment
created_at: 2023-05-18T19:47:45Z
updated_at: 2023-05-19T10:09:48Z
url: https://github.com/astral-sh/ruff/pull/4508
synced_at: 2026-01-12T03:50:03Z
```

# Avoid some false positives in dunder variable assigments

---

_Pull request opened by @scop on 2023-05-18 19:47_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-05-18 19:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.2±0.19ms     2.7 MB/sec    1.00     15.2±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.5 MB/sec    1.00      3.7±0.01ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    379.4±3.13µs     7.8 MB/sec    1.00    378.1±1.94µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.01ms     5.5 MB/sec    1.00      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1553.6±7.97µs    10.7 MB/sec    1.00   1535.7±2.80µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.2±2.26µs    17.8 MB/sec    1.00    165.5±0.71µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      6.0±0.01ms     6.8 MB/sec    1.03      6.2±0.00ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1177.4±14.00µs    14.1 MB/sec    1.02   1201.5±3.33µs    13.9 MB/sec
parser/numpy/globals.py                    1.00    121.3±0.44µs    24.3 MB/sec    1.03    124.5±0.34µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.00ms    10.0 MB/sec    1.01      2.6±0.00ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.12ms     2.5 MB/sec    1.00     16.7±0.08ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.00      4.3±0.02ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    439.9±4.18µs     6.7 MB/sec    1.00    436.3±4.21µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.07ms     3.6 MB/sec    1.00      7.1±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.02ms     4.8 MB/sec    1.01      8.5±0.02ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1740.7±9.83µs     9.6 MB/sec    1.02   1782.5±8.00µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.2±1.65µs    15.6 MB/sec    1.04   196.0±14.28µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.7 MB/sec    1.02      3.9±0.16ms     6.5 MB/sec
parser/large/dataset.py                    1.00      7.0±0.02ms     5.8 MB/sec    1.00      7.0±0.02ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1319.3±8.22µs    12.6 MB/sec    1.00   1321.0±7.36µs    12.6 MB/sec
parser/numpy/globals.py                    1.00    138.0±1.59µs    21.4 MB/sec    1.00    138.0±0.98µs    21.4 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.02ms     8.7 MB/sec    1.00      2.9±0.01ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-05-18 23:23_

Hey @scop it'd probably be useful to include a test case covering the false positive that this would previously detect

---

_Comment by @charliermarsh on 2023-05-19 01:49_

Agreed -- I can infer the error in this case, so went ahead and added a test (and removed the regex -- I think it's unnecessary for this simple check), but a good practice in general for sure.

---

_Merged by @charliermarsh on 2023-05-19 02:11_

---

_Closed by @charliermarsh on 2023-05-19 02:11_

---

_Branch deleted on 2023-05-19 10:09_

---
