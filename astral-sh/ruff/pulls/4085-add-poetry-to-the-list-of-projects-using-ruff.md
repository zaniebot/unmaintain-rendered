```yaml
number: 4085
title: Add Poetry to the list of projects using Ruff
type: pull_request
state: merged
author: Secrus
labels: []
assignees: []
merged: true
base: main
head: add-poetry
created_at: 2023-04-24T22:35:15Z
updated_at: 2023-04-24T23:48:36Z
url: https://github.com/astral-sh/ruff/pull/4085
synced_at: 2026-01-12T04:28:19Z
```

# Add Poetry to the list of projects using Ruff

---

_Pull request opened by @Secrus on 2023-04-24 22:35_

As in the title. Poetry migrates its projects to using Ruff, so let's add it to the list.

---

_Comment by @github-actions[bot] on 2023-04-24 22:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.05ms     2.8 MB/sec    1.00     14.5±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.00ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    466.0±3.49µs     6.3 MB/sec    1.00    464.0±0.93µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.02ms     5.5 MB/sec    1.01      7.5±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1674.4±1.44µs     9.9 MB/sec    1.00   1674.8±2.79µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.4±0.27µs    16.0 MB/sec    1.00    184.7±1.29µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.5 MB/sec    1.00      3.4±0.01ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     6.9 MB/sec    1.00      5.9±0.00ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.01   1159.2±0.99µs    14.4 MB/sec    1.00   1151.3±0.45µs    14.5 MB/sec
parser/numpy/globals.py                    1.01    114.9±0.27µs    25.7 MB/sec    1.00    114.0±0.79µs    25.9 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.1 MB/sec    1.00      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.11ms     2.4 MB/sec    1.00     16.8±0.10ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.05ms     3.8 MB/sec    1.00      4.4±0.05ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    537.2±4.82µs     5.5 MB/sec    1.00    538.6±5.77µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.07ms     3.6 MB/sec    1.00      7.2±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.05ms     4.7 MB/sec    1.00      8.7±0.04ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1911.3±28.91µs     8.7 MB/sec    1.00  1917.4±21.11µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.2±2.80µs    13.8 MB/sec    1.02    218.9±5.85µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.03ms     6.5 MB/sec    1.01      4.0±0.05ms     6.4 MB/sec
parser/large/dataset.py                    1.01      6.9±0.05ms     5.9 MB/sec    1.00      6.8±0.04ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1316.1±14.13µs    12.7 MB/sec    1.00  1297.4±12.56µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    131.6±1.82µs    22.4 MB/sec    1.00    131.1±2.61µs    22.5 MB/sec
parser/pydantic/types.py                   1.01      3.0±0.02ms     8.6 MB/sec    1.00      2.9±0.03ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-04-24 23:48_

Amazing! Thanks @Secrus.

---

_Merged by @charliermarsh on 2023-04-24 23:48_

---

_Closed by @charliermarsh on 2023-04-24 23:48_

---
