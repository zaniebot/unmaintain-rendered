```yaml
number: 4506
title: Bump version to 0.0.269
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2023-05-18T19:36:36Z
updated_at: 2023-05-18T20:08:53Z
url: https://github.com/astral-sh/ruff/pull/4506
synced_at: 2026-01-12T03:50:03Z
```

# Bump version to 0.0.269

---

_Pull request opened by @charliermarsh on 2023-05-18 19:36_

I forgot to commit the version bump, so we have to cut a new release.

---

_Merged by @charliermarsh on 2023-05-18 19:45_

---

_Closed by @charliermarsh on 2023-05-18 19:45_

---

_Branch deleted on 2023-05-18 19:45_

---

_Comment by @github-actions[bot] on 2023-05-18 19:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.2±0.03ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.8 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    425.0±0.49µs     6.9 MB/sec    1.00    425.7±1.40µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.02ms     6.0 MB/sec    1.00      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1470.2±2.37µs    11.3 MB/sec    1.00   1458.2±4.69µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.0±0.22µs    18.1 MB/sec    1.00    162.5±0.43µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1069.9±0.59µs    15.6 MB/sec    1.00   1072.8±0.55µs    15.5 MB/sec
parser/numpy/globals.py                    1.00    110.0±0.51µs    26.8 MB/sec    1.00    110.2±0.29µs    26.8 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    10.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     21.0±0.40ms  1983.0 KB/sec    1.00     20.5±0.40ms  2035.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.3±0.15ms     3.1 MB/sec    1.00      5.2±0.16ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   619.1±19.74µs     4.8 MB/sec    1.01   623.8±18.58µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.8±0.28ms     2.9 MB/sec    1.00      8.6±0.27ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.02     10.4±0.26ms     3.9 MB/sec    1.00     10.1±0.18ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.2±0.05ms     7.7 MB/sec    1.00      2.1±0.03ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    244.2±8.86µs    12.1 MB/sec    1.00    245.3±7.32µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.6±0.10ms     5.5 MB/sec    1.00      4.5±0.15ms     5.6 MB/sec
parser/large/dataset.py                    1.00      8.2±0.19ms     5.0 MB/sec    1.00      8.1±0.12ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1540.5±48.65µs    10.8 MB/sec    1.00  1547.0±41.74µs    10.8 MB/sec
parser/numpy/globals.py                    1.01    159.1±4.49µs    18.5 MB/sec    1.00    158.2±4.12µs    18.6 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.08ms     7.4 MB/sec    1.00      3.4±0.07ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
