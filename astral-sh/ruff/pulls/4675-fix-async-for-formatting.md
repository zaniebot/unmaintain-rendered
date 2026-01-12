```yaml
number: 4675
title: "Fix `async for` formatting"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/doc-format
created_at: 2023-05-27T02:46:01Z
updated_at: 2023-05-27T03:18:19Z
url: https://github.com/astral-sh/ruff/pull/4675
synced_at: 2026-01-12T03:50:03Z
```

# Fix `async for` formatting

---

_Pull request opened by @charliermarsh on 2023-05-27 02:46_

(My mistake.)

---

_Merged by @charliermarsh on 2023-05-27 02:53_

---

_Closed by @charliermarsh on 2023-05-27 02:53_

---

_Branch deleted on 2023-05-27 02:53_

---

_Comment by @github-actions[bot] on 2023-05-27 02:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     17.7±0.08ms     2.3 MB/sec    1.00     16.8±0.07ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.1±0.01ms     4.1 MB/sec    1.00      4.0±0.01ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    506.9±1.10µs     5.8 MB/sec    1.00    504.9±1.14µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.02ms     3.6 MB/sec    1.00      7.0±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.04ms     5.0 MB/sec    1.00      8.1±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1786.0±7.35µs     9.3 MB/sec    1.00   1786.8±2.58µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.1±0.68µs    14.7 MB/sec    1.01    203.1±4.81µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.9 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
parser/large/dataset.py                    1.15      7.1±0.01ms     5.7 MB/sec    1.00      6.2±0.01ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.12   1380.9±0.81µs    12.1 MB/sec    1.00   1228.9±3.44µs    13.6 MB/sec
parser/numpy/globals.py                    1.09    138.7±1.73µs    21.3 MB/sec    1.00    127.2±0.40µs    23.2 MB/sec
parser/pydantic/types.py                   1.13      3.0±0.01ms     8.5 MB/sec    1.00      2.7±0.00ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.3±0.21ms     2.7 MB/sec    1.00     15.2±0.21ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.05ms     4.3 MB/sec    1.00      3.8±0.07ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.0±8.49µs     6.5 MB/sec    1.01   457.6±13.71µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.13ms     4.0 MB/sec    1.00      6.4±0.11ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.10ms     5.5 MB/sec    1.00      7.4±0.10ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1596.5±22.64µs    10.4 MB/sec    1.01  1615.3±25.59µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.8±4.77µs    16.1 MB/sec    1.02    186.7±4.47µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.04ms     7.5 MB/sec    1.01      3.4±0.06ms     7.5 MB/sec
parser/large/dataset.py                    1.01      5.8±0.06ms     7.0 MB/sec    1.00      5.8±0.07ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1099.6±22.68µs    15.1 MB/sec    1.00  1100.5±20.53µs    15.1 MB/sec
parser/numpy/globals.py                    1.00    112.2±2.41µs    26.3 MB/sec    1.00    111.6±2.40µs    26.4 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.04ms    10.3 MB/sec    1.01      2.5±0.05ms    10.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
