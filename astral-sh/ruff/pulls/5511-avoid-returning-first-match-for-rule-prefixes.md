```yaml
number: 5511
title: Avoid returning first-match for rule prefixes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/prefix
created_at: 2023-07-04T19:17:38Z
updated_at: 2023-07-04T19:42:45Z
url: https://github.com/astral-sh/ruff/pull/5511
synced_at: 2026-01-12T03:36:55Z
```

# Avoid returning first-match for rule prefixes

---

_Pull request opened by @charliermarsh on 2023-07-04 19:17_

Closes #5495, but there's a TODO here to improve this further. The current `from_code` implementation feels really indirect.

---

_@charliermarsh reviewed on 2023-07-04 19:17_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/rule.rs`:34 on 2023-07-04 19:17_

(Unused, we already do this a few lines up.)

---

_Label `bug` added by @charliermarsh on 2023-07-04 19:18_

---

_Merged by @charliermarsh on 2023-07-04 19:23_

---

_Closed by @charliermarsh on 2023-07-04 19:23_

---

_Branch deleted on 2023-07-04 19:23_

---

_Comment by @github-actions[bot] on 2023-07-04 19:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.09ms     4.4 MB/sec    1.00      9.2±0.11ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     8.1 MB/sec    1.00      2.1±0.02ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    232.6±2.57µs    12.7 MB/sec    1.00    233.7±5.69µs    12.6 MB/sec
formatter/pydantic/types.py                1.02      4.5±0.03ms     5.6 MB/sec    1.00      4.4±0.05ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.19ms     2.6 MB/sec    1.00     15.8±0.19ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.01      4.0±0.05ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    510.5±6.37µs     5.8 MB/sec    1.00    512.9±5.02µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.7 MB/sec    1.01      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.07ms     5.2 MB/sec    1.00      7.9±0.07ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1740.1±24.85µs     9.6 MB/sec    1.00  1729.7±20.72µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.03    200.2±1.60µs    14.7 MB/sec    1.00    194.7±3.34µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.6±0.03ms     5.3 MB/sec    1.00      7.6±0.03ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1595.7±14.97µs    10.4 MB/sec    1.01   1604.0±7.35µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    171.0±1.69µs    17.3 MB/sec    1.01    173.2±5.60µs    17.0 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.07ms     3.2 MB/sec    1.00     12.6±0.05ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.02ms     5.2 MB/sec    1.00      3.2±0.01ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    344.4±3.06µs     8.6 MB/sec    1.00    344.1±2.61µs     8.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.02ms     4.6 MB/sec    1.00      5.5±0.03ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.1 MB/sec    1.01      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1343.9±7.56µs    12.4 MB/sec    1.00   1344.5±7.79µs    12.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    146.7±1.15µs    20.1 MB/sec    1.00    146.7±1.27µs    20.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.8 MB/sec    1.00      2.9±0.01ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
