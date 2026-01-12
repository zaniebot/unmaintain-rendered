```yaml
number: 4310
title: Revert the B027 autofix logic
type: pull_request
state: merged
author: aacunningham
labels:
  - fixes
assignees: []
merged: true
base: main
head: revert-B027-autofix
created_at: 2023-05-09T16:10:55Z
updated_at: 2023-05-09T17:08:24Z
url: https://github.com/astral-sh/ruff/pull/4310
synced_at: 2026-01-12T15:55:15Z
```

# Revert the B027 autofix logic

---

_@aacunningham_

_Un_-resolves #4163. After discussion, the autofix is too risky to keep. Adding the `@abstractmethod` decorator can cause concrete implementations that didn't implement that method to raise errors that it wasn't previously.

This can be brought back as an unsafe fix when the work to implement fix safety levels is finished.

---

_Comment by @github-actions[bot] on 2023-05-09 16:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.07ms     2.9 MB/sec    1.01     14.2±0.14ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     5.0 MB/sec    1.00      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    414.3±0.79µs     7.1 MB/sec    1.00    414.8±0.87µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.02ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1477.6±2.95µs    11.3 MB/sec    1.00   1479.2±6.56µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.5±0.27µs    18.3 MB/sec    1.01    163.0±0.24µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.00ms     8.3 MB/sec    1.01      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.01      5.5±0.00ms     7.4 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.01   1073.9±1.17µs    15.5 MB/sec    1.00   1059.9±0.68µs    15.7 MB/sec
parser/numpy/globals.py                    1.01    108.6±0.50µs    27.2 MB/sec    1.00    107.7±0.20µs    27.4 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    10.9 MB/sec    1.00      2.3±0.01ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     18.9±1.53ms     2.1 MB/sec     1.01     19.1±1.28ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.32ms     3.5 MB/sec     1.02      4.8±0.29ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.02   567.3±32.27µs     5.2 MB/sec     1.00   554.2±35.85µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.54ms     3.3 MB/sec     1.09      8.4±0.58ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.07     10.3±0.53ms     4.0 MB/sec     1.00      9.6±0.71ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.13ms     8.2 MB/sec     1.08      2.2±0.12ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   234.6±18.23µs    12.6 MB/sec     1.08   253.3±17.69µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.37ms     5.8 MB/sec     1.01      4.5±0.31ms     5.7 MB/sec
parser/large/dataset.py                    1.01      8.2±0.42ms     4.9 MB/sec     1.00      8.2±0.50ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1591.9±103.74µs    10.5 MB/sec    1.01  1610.5±97.77µs    10.3 MB/sec
parser/numpy/globals.py                    1.06    167.6±7.74µs    17.6 MB/sec     1.00   158.3±14.47µs    18.6 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.22ms     7.4 MB/sec     1.02      3.5±0.18ms     7.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-09 17:08_

---

_Closed by @charliermarsh on 2023-05-09 17:08_

---

_Label `autofix` added by @charliermarsh on 2023-05-09 17:08_

---
