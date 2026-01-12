```yaml
number: 3871
title: Allow starred arguments in B030
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/stars
created_at: 2023-04-03T22:50:35Z
updated_at: 2023-04-03T23:37:05Z
url: https://github.com/astral-sh/ruff/pull/3871
synced_at: 2026-01-12T04:28:19Z
```

# Allow starred arguments in B030

---

_Pull request opened by @charliermarsh on 2023-04-03 22:50_

Closes #3869.

---

_Merged by @charliermarsh on 2023-04-03 23:20_

---

_Closed by @charliermarsh on 2023-04-03 23:20_

---

_Branch deleted on 2023-04-03 23:20_

---

_Comment by @github-actions[bot] on 2023-04-03 23:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.04ms     2.9 MB/sec    1.01     14.2±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.01      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    459.0±0.89µs     6.4 MB/sec    1.01    462.9±0.97µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.01      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.01ms     5.7 MB/sec    1.01      7.3±0.03ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1639.7±7.81µs    10.2 MB/sec    1.01   1651.5±6.76µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.3±0.40µs    16.3 MB/sec    1.00    182.1±1.04µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.01      3.4±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.9±0.21ms     2.6 MB/sec    1.00     15.9±0.26ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.01      4.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    486.0±7.32µs     6.1 MB/sec    1.00    485.6±7.71µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.10ms     3.8 MB/sec    1.00      6.7±0.07ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.08ms     5.0 MB/sec    1.00      8.2±0.09ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1776.0±22.41µs     9.4 MB/sec    1.01  1796.2±44.01µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.6±4.40µs    15.3 MB/sec    1.01    195.1±5.10µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.00      3.7±0.05ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
