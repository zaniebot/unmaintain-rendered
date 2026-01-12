```yaml
number: 3856
title: "Change \"indexes\" to \"indices\" in various contexts"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/indices
created_at: 2023-04-02T23:02:20Z
updated_at: 2023-04-02T23:26:24Z
url: https://github.com/astral-sh/ruff/pull/3856
synced_at: 2026-01-12T04:28:19Z
```

# Change "indexes" to "indices" in various contexts

---

_Pull request opened by @charliermarsh on 2023-04-02 23:02_

Both Rust and Python seem to use `indices` in public-facing API, so let's mirror that.

---

_Marked ready for review by @charliermarsh on 2023-04-02 23:02_

---

_Renamed from "Change indexes to indices in various contexts" to "Change "indexes" to "indices" in various contexts" by @charliermarsh on 2023-04-02 23:02_

---

_Merged by @charliermarsh on 2023-04-02 23:08_

---

_Closed by @charliermarsh on 2023-04-02 23:08_

---

_Branch deleted on 2023-04-02 23:08_

---

_Comment by @github-actions[bot] on 2023-04-02 23:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.1±0.07ms     2.7 MB/sec    1.00     14.9±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    411.4±1.02µs     7.2 MB/sec    1.01    417.1±1.46µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.5±0.02ms     3.9 MB/sec    1.00      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.02      8.0±0.06ms     5.1 MB/sec    1.00      7.9±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1766.1±1.65µs     9.4 MB/sec    1.00   1743.5±3.70µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    184.2±0.56µs    16.0 MB/sec    1.01    185.2±0.56µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     7.0 MB/sec    1.00      3.6±0.00ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     21.1±0.72ms  1975.8 KB/sec    1.00     20.8±0.60ms  2000.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.4±0.23ms     3.1 MB/sec    1.00      5.3±0.23ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   640.1±27.51µs     4.6 MB/sec    1.00   632.3±38.42µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.9±0.38ms     2.9 MB/sec    1.00      8.8±0.34ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.6±0.62ms     3.8 MB/sec    1.00     10.4±0.30ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.07ms     7.4 MB/sec    1.02      2.3±0.10ms     7.3 MB/sec
linter/default-rules/numpy/globals.py      1.03   273.5±17.83µs    10.8 MB/sec    1.00   265.8±14.90µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.9±0.24ms     5.3 MB/sec    1.00      4.8±0.19ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
