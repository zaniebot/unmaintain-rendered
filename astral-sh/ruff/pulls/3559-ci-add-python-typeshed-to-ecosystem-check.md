```yaml
number: 3559
title: "ci: add `python/typeshed` to ecosystem check"
type: pull_request
state: merged
author: XuehaiPan
labels: []
assignees: []
merged: true
base: main
head: ecosystem-typeshed
created_at: 2023-03-16T14:15:10Z
updated_at: 2023-03-17T06:42:34Z
url: https://github.com/astral-sh/ruff/pull/3559
synced_at: 2026-01-12T04:39:45Z
```

# ci: add `python/typeshed` to ecosystem check

---

_Pull request opened by @XuehaiPan on 2023-03-16 14:15_

Add [`python/typeshed`](https://github.com/python/typeshed) to ecosystem check with the rule `PYI`.

---

_Comment by @github-actions[bot] on 2023-03-16 14:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      8.3±0.01ms     4.9 MB/sec    1.00      8.3±0.03ms     4.9 MB/sec
linter/numpy/ctypeslib.py    1.00      2.2±0.01ms   151.8 MB/sec    1.00      2.2±0.01ms   151.7 MB/sec
linter/numpy/globals.py      1.00   1147.3±5.37µs   155.4 MB/sec    1.00   1146.8±4.74µs   155.4 MB/sec
linter/pydantic/types.py     1.00      3.9±0.01ms     6.6 MB/sec    1.01      3.9±0.02ms     6.6 MB/sec
```

#### Windows
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      8.7±0.08ms     4.6 MB/sec    1.00      8.8±0.02ms     4.6 MB/sec
linter/numpy/ctypeslib.py    1.00      2.3±0.01ms   148.9 MB/sec    1.01      2.3±0.02ms   148.0 MB/sec
linter/numpy/globals.py      1.00   1167.4±6.72µs   152.7 MB/sec    1.00  1169.8±21.51µs   152.4 MB/sec
linter/pydantic/types.py     1.00      4.1±0.02ms     6.3 MB/sec    1.01      4.1±0.05ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-03-16 14:55_

---

_Merged by @charliermarsh on 2023-03-16 18:19_

---

_Closed by @charliermarsh on 2023-03-16 18:19_

---

_Branch deleted on 2023-03-17 06:42_

---
