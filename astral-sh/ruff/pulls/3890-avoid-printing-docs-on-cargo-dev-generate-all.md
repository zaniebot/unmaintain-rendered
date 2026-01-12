```yaml
number: 3890
title: Avoid printing docs on cargo dev generate-all
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/print
created_at: 2023-04-05T17:59:12Z
updated_at: 2023-04-05T18:26:29Z
url: https://github.com/astral-sh/ruff/pull/3890
synced_at: 2026-01-12T15:55:14Z
```

# Avoid printing docs on cargo dev generate-all

---

_@charliermarsh_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-04-05 18:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.5±0.22ms     2.8 MB/sec    1.00     14.4±0.09ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    459.9±0.84µs     6.4 MB/sec    1.00    460.8±1.32µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.01      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.6 MB/sec    1.01      7.3±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1630.3±7.30µs    10.2 MB/sec    1.01   1643.1±5.07µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.5±0.88µs    16.3 MB/sec    1.01    182.0±1.56µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.17ms     2.5 MB/sec    1.01     16.4±0.27ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.01      4.2±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   502.0±11.59µs     5.9 MB/sec    1.00    495.7±7.32µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.07ms     3.7 MB/sec    1.00      6.8±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.06ms     4.9 MB/sec    1.00      8.2±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1819.1±23.72µs     9.2 MB/sec    1.01  1829.0±26.96µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.3±4.06µs    15.0 MB/sec    1.00    197.7±5.65µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.7 MB/sec    1.00      3.8±0.07ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-05 18:18_

---

_Closed by @charliermarsh on 2023-04-05 18:18_

---

_Branch deleted on 2023-04-05 18:18_

---
