```yaml
number: 3938
title: Add linting badge that can be used to display usage
type: pull_request
state: merged
author: OMEGARAZER
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2023-04-11T20:20:57Z
updated_at: 2023-05-12T18:21:36Z
url: https://github.com/astral-sh/ruff/pull/3938
synced_at: 2026-01-12T03:50:02Z
```

# Add linting badge that can be used to display usage

---

_Pull request opened by @OMEGARAZER on 2023-04-11 20:20_

Was looking for a badge to go along with the one from Black and made a small edit to the one at the top of the readme that I thought looked good. Provided markdown, rst and HTML versions.

Wasn't 100% on the best place to put it but figured under "Who's using" would be a decent spot if it's added.

---

_Comment by @github-actions[bot] on 2023-04-11 20:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.7±0.06ms     2.8 MB/sec    1.00     14.5±0.06ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    363.7±7.11µs     8.1 MB/sec    1.00    360.9±1.37µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.2±0.04ms     4.1 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.02ms     5.6 MB/sec    1.00      7.2±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1516.8±4.90µs    11.0 MB/sec    1.00   1502.7±4.17µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    164.1±0.32µs    18.0 MB/sec    1.00    162.7±0.31µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
parser/large/dataset.py                    1.00      5.8±0.01ms     7.0 MB/sec    1.00      5.8±0.00ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1127.9±3.05µs    14.8 MB/sec    1.00   1127.4±1.05µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    115.0±0.39µs    25.7 MB/sec    1.01    115.7±0.29µs    25.5 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.01ms    10.3 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     20.7±0.86ms  2011.7 KB/sec    1.00     20.2±0.84ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.2±0.41ms     3.2 MB/sec    1.00      5.0±0.21ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   579.0±23.46µs     5.1 MB/sec    1.05   607.1±52.95µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.34ms     3.1 MB/sec    1.00      8.2±0.47ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.01     10.0±0.36ms     4.1 MB/sec    1.00      9.9±0.37ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.10ms     7.7 MB/sec    1.02      2.2±0.11ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   242.3±16.08µs    12.2 MB/sec    1.00   243.1±12.16µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.5±0.18ms     5.6 MB/sec    1.00      4.5±0.18ms     5.7 MB/sec
parser/large/dataset.py                    1.04      8.5±0.18ms     4.8 MB/sec    1.00      8.2±0.24ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.06  1663.3±67.74µs    10.0 MB/sec    1.00  1567.0±60.30µs    10.6 MB/sec
parser/numpy/globals.py                    1.00    160.4±8.35µs    18.4 MB/sec    1.00    160.0±7.93µs    18.4 MB/sec
parser/pydantic/types.py                   1.01      3.7±0.12ms     7.0 MB/sec    1.00      3.6±0.14ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-05-12 17:46_

Thanks :)

---

_Merged by @charliermarsh on 2023-05-12 17:58_

---

_Closed by @charliermarsh on 2023-05-12 17:58_

---
