```yaml
number: 3564
title: "Fix autofix conflict between `D209` and `D400`"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: fix-d209-d400-conflict
created_at: 2023-03-16T22:15:48Z
updated_at: 2023-03-17T06:26:04Z
url: https://github.com/astral-sh/ruff/pull/3564
synced_at: 2026-01-12T15:55:13Z
```

# Fix autofix conflict between `D209` and `D400`

---

_@JonathanPlasse_

- Closes #3538
- The snapshot of the first commit was generated with #3557. Without #3557, there is no change to the snapshot, so I opened a PR for this.

---

_Comment by @github-actions[bot] on 2023-03-16 22:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      8.2±0.02ms     4.9 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
linter/numpy/ctypeslib.py    1.00      2.2±0.01ms   152.7 MB/sec    1.01      2.2±0.01ms   151.6 MB/sec
linter/numpy/globals.py      1.00   1138.1±7.06µs   156.6 MB/sec    1.01  1152.3±14.55µs   154.7 MB/sec
linter/pydantic/types.py     1.00      3.8±0.01ms     6.6 MB/sec    1.00      3.8±0.00ms     6.6 MB/sec
```

#### Windows
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      8.2±0.07ms     4.9 MB/sec    1.01      8.3±0.11ms     4.9 MB/sec
linter/numpy/ctypeslib.py    1.00      2.0±0.01ms   166.0 MB/sec    1.00      2.0±0.01ms   165.6 MB/sec
linter/numpy/globals.py      1.00  1042.0±11.90µs   171.1 MB/sec    1.01  1049.1±17.38µs   169.9 MB/sec
linter/pydantic/types.py     1.00      3.7±0.05ms     6.9 MB/sec    1.01      3.7±0.06ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Fix D209 D400 conflict" to "Fix autofix conflict between `D209` and `D400`" by @charliermarsh on 2023-03-17 02:31_

---

_Merged by @charliermarsh on 2023-03-17 02:36_

---

_Closed by @charliermarsh on 2023-03-17 02:36_

---

_Branch deleted on 2023-03-17 06:26_

---
