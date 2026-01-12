```yaml
number: 3738
title: "Rename setter methods on `Diagnostic`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/with_fix
created_at: 2023-03-26T04:00:11Z
updated_at: 2023-03-26T14:28:31Z
url: https://github.com/astral-sh/ruff/pull/3738
synced_at: 2026-01-12T04:39:45Z
```

# Rename setter methods on `Diagnostic`

---

_Pull request opened by @charliermarsh on 2023-03-26 04:00_

_No description provided._

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-26 04:00_

---

_Review comment by @charliermarsh on `crates/ruff_diagnostics/src/diagnostic.rs`:46 on 2023-03-26 04:02_

Alternatively, we could use `with_fix` and continue to return `self`, but we don't use that behavior anywhere anyway, so I made it a simple setter.

---

_@charliermarsh reviewed on 2023-03-26 04:02_

---

_Comment by @github-actions[bot] on 2023-03-26 04:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.17ms     2.4 MB/sec    1.02     17.2±0.07ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.10ms     3.8 MB/sec    1.03      4.5±0.05ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    592.0±1.72µs     5.0 MB/sec    1.01    599.7±1.39µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.13ms     3.4 MB/sec    1.01      7.6±0.05ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.07ms     4.5 MB/sec    1.03      9.2±0.31ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1963.3±8.36µs     8.5 MB/sec    1.01   1976.5±4.03µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.02    226.4±5.44µs    13.0 MB/sec    1.00    222.7±0.55µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.04ms     6.2 MB/sec    1.01      4.2±0.02ms     6.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.10ms     2.7 MB/sec    1.00     15.2±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.01ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    455.1±3.49µs     6.5 MB/sec    1.00    451.7±3.80µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.05ms     3.8 MB/sec    1.00      6.7±0.03ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     5.0 MB/sec    1.00      8.2±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1767.6±6.84µs     9.4 MB/sec    1.00  1758.4±13.38µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.9±1.49µs    16.0 MB/sec    1.02    187.0±5.94µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.05ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-03-26 13:20_

---

_Merged by @charliermarsh on 2023-03-26 14:28_

---

_Closed by @charliermarsh on 2023-03-26 14:28_

---

_Branch deleted on 2023-03-26 14:28_

---
