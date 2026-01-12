```yaml
number: 3944
title: Remove unused import
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/unused
created_at: 2023-04-12T03:49:58Z
updated_at: 2023-04-12T04:13:08Z
url: https://github.com/astral-sh/ruff/pull/3944
synced_at: 2026-01-12T04:28:19Z
```

# Remove unused import

---

_Pull request opened by @charliermarsh on 2023-04-12 03:49_

## Summary

Not sure how this made it past Clippy + our branch protection rules, must've become unused through a rebase.

---

_Merged by @charliermarsh on 2023-04-12 03:55_

---

_Closed by @charliermarsh on 2023-04-12 03:55_

---

_Branch deleted on 2023-04-12 03:55_

---

_Comment by @github-actions[bot] on 2023-04-12 04:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     14.5±0.13ms     2.8 MB/sec    1.00     14.3±0.12ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    464.5±1.27µs     6.4 MB/sec    1.00    462.6±1.50µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.03ms     4.2 MB/sec    1.00      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.03ms     5.6 MB/sec    1.00      7.2±0.03ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1632.5±6.05µs    10.2 MB/sec    1.00   1621.8±4.71µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.6±0.45µs    16.4 MB/sec    1.01    181.1±4.04µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.24ms     2.5 MB/sec    1.00     16.6±0.25ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     3.9 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   506.6±12.56µs     5.8 MB/sec    1.00    496.2±6.44µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.7 MB/sec    1.00      6.9±0.11ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.09ms     4.9 MB/sec    1.01      8.4±0.12ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1839.0±21.56µs     9.1 MB/sec    1.00  1830.3±19.53µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.6±4.38µs    14.9 MB/sec    1.00    198.0±4.37µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.05ms     6.7 MB/sec    1.00      3.8±0.08ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
