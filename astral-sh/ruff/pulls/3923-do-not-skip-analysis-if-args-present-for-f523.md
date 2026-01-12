```yaml
number: 3923
title: "Do not skip analysis if `*args` present for `F523`"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/f523-star-args
created_at: 2023-04-09T06:16:36Z
updated_at: 2023-04-10T04:57:42Z
url: https://github.com/astral-sh/ruff/pull/3923
synced_at: 2026-01-12T15:55:14Z
```

# Do not skip analysis if `*args` present for `F523`

---

_@dhruvmanila_

The `*args` argument will be skipped while collecting indices for extra positional arguments. Here, it's not required for the argument name to be `args`.

fixes: #2567 

---

_Comment by @github-actions[bot] on 2023-04-09 06:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.2±0.03ms     2.9 MB/sec    1.00     14.1±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    462.7±4.80µs     6.4 MB/sec    1.00    459.8±1.06µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.02ms     4.2 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.01ms     5.6 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1635.5±6.90µs    10.2 MB/sec    1.00   1619.6±4.26µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    180.8±0.49µs    16.3 MB/sec    1.00    179.6±1.48µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.00ms     7.6 MB/sec    1.00      3.3±0.00ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.09ms     2.5 MB/sec    1.00     16.1±0.08ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.01ms     4.0 MB/sec    1.00      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.9±4.30µs     6.8 MB/sec    1.00    433.4±4.83µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.03ms     3.7 MB/sec    1.00      6.9±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.03ms     5.0 MB/sec    1.01      8.3±0.02ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1771.3±9.30µs     9.4 MB/sec    1.02  1803.2±14.64µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.8±1.35µs    16.0 MB/sec    1.02    187.6±4.40µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.8 MB/sec    1.00      3.8±0.03ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-09 22:34_

---

_Closed by @charliermarsh on 2023-04-09 22:34_

---

_Comment by @charliermarsh on 2023-04-09 22:34_

Thanks!

---

_Branch deleted on 2023-04-10 04:57_

---
