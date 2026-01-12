```yaml
number: 3563
title: "ci(check_ecosystem): add scikit-build-core"
type: pull_request
state: merged
author: henryiii
labels: []
assignees: []
merged: true
base: main
head: patch-3
created_at: 2023-03-16T21:30:28Z
updated_at: 2023-03-16T23:46:45Z
url: https://github.com/astral-sh/ruff/pull/3563
synced_at: 2026-01-12T04:39:45Z
```

# ci(check_ecosystem): add scikit-build-core

---

_Pull request opened by @henryiii on 2023-03-16 21:30_

This one is a very modern and clean codebase with a lot of checking. I'm not biased at all, obviously. :)

---

_Comment by @github-actions[bot] on 2023-03-16 21:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      8.5±0.28ms     4.8 MB/sec    1.03      8.8±0.23ms     4.6 MB/sec
linter/numpy/ctypeslib.py    1.01      2.9±0.09ms   117.5 MB/sec    1.00      2.8±0.10ms   119.1 MB/sec
linter/numpy/globals.py      1.01  1489.8±63.30µs   119.7 MB/sec    1.00  1470.7±59.37µs   121.2 MB/sec
linter/pydantic/types.py     1.00      4.1±0.13ms     6.3 MB/sec    1.02      4.1±0.10ms     6.2 MB/sec
```

#### Windows
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      7.5±0.05ms     5.5 MB/sec    1.00      7.5±0.04ms     5.4 MB/sec
linter/numpy/ctypeslib.py    1.02  1942.6±65.89µs   178.8 MB/sec    1.00  1910.1±52.81µs   181.8 MB/sec
linter/numpy/globals.py      1.03   968.4±46.97µs   184.1 MB/sec    1.00   941.7±31.33µs   189.3 MB/sec
linter/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.01      3.4±0.02ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-16 23:46_

---

_Closed by @charliermarsh on 2023-03-16 23:46_

---

_Comment by @charliermarsh on 2023-03-16 23:46_

Thanks :)

---
