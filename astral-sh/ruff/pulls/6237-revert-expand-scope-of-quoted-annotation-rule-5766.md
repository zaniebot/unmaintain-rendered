```yaml
number: 6237
title: "Revert \"Expand scope of `quoted-annotation` rule (#5766)\""
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/unquote
created_at: 2023-08-01T12:46:08Z
updated_at: 2023-08-01T13:25:06Z
url: https://github.com/astral-sh/ruff/pull/6237
synced_at: 2026-01-12T02:58:30Z
```

# Revert "Expand scope of `quoted-annotation` rule (#5766)"

---

_Pull request opened by @charliermarsh on 2023-08-01 12:46_

This is causing some problems, so we'll just revert for now.

Closes https://github.com/astral-sh/ruff/issues/6189.

---

_@MichaReiser approved on 2023-08-01 12:58_

---

_Comment by @github-actions[bot] on 2023-08-01 13:00_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.3±0.05ms     4.9 MB/sec    1.01      8.4±0.05ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1622.5±21.84µs    10.3 MB/sec    1.01  1633.9±23.34µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    173.2±1.42µs    17.0 MB/sec    1.00    172.7±0.39µs    17.1 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.04ms     7.2 MB/sec    1.00      3.5±0.02ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     11.3±0.03ms     3.6 MB/sec    1.00     11.3±0.05ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.01ms     5.7 MB/sec    1.00      2.9±0.01ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    319.6±5.99µs     9.2 MB/sec    1.00    318.5±1.93µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.1±0.01ms     5.0 MB/sec    1.00      5.1±0.02ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.01ms     6.8 MB/sec    1.00      6.0±0.01ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1196.6±3.54µs    13.9 MB/sec    1.00  1196.0±10.64µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    121.2±0.22µs    24.3 MB/sec    1.00    121.0±2.58µs    24.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.01ms     9.7 MB/sec    1.00      2.6±0.01ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.07ms     4.1 MB/sec    1.00      9.8±0.07ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1883.6±15.44µs     8.8 MB/sec    1.00  1885.4±12.70µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    198.0±2.09µs    14.9 MB/sec    1.02    201.3±6.86µs    14.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.04ms     6.0 MB/sec    1.00      4.2±0.04ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.07ms     3.0 MB/sec    1.00     13.4±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.7 MB/sec    1.00      3.6±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.7±6.33µs     8.1 MB/sec    1.00    364.4±5.40µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.03ms     4.2 MB/sec    1.00      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.04ms     5.6 MB/sec    1.00      7.3±0.04ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1456.3±10.38µs    11.4 MB/sec    1.00   1456.0±9.32µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    146.2±0.97µs    20.2 MB/sec    1.00    146.7±2.17µs    20.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.02ms     7.9 MB/sec    1.00      3.2±0.03ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-01 13:03_

---

_Closed by @charliermarsh on 2023-08-01 13:03_

---

_Branch deleted on 2023-08-01 13:03_

---
