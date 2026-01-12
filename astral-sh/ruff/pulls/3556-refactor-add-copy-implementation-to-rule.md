```yaml
number: 3556
title: "refactor: Add Copy implementation to Rule"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: rule-copy
created_at: 2023-03-16T09:06:59Z
updated_at: 2023-03-16T16:50:20Z
url: https://github.com/astral-sh/ruff/pull/3556
synced_at: 2026-01-12T04:39:45Z
```

# refactor: Add Copy implementation to Rule

---

_Pull request opened by @MichaReiser on 2023-03-16 09:06_

This PR adds a `Copy` implementation to `Rule` because `Rule` is only two bytes large (less than a poiner). 

I don't expect this to improve performance, but it allows us to remove some lifetime annotations.


---

_Renamed from "Add Copy implementation to Rule" to "refactor: Add Copy implementation to Rule" by @MichaReiser on 2023-03-16 09:10_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-16 09:10_

---

_Comment by @github-actions[bot] on 2023-03-16 09:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      8.2±0.01ms     5.0 MB/sec    1.00      8.2±0.02ms     4.9 MB/sec
linter/numpy/ctypeslib.py    1.00      2.2±0.01ms   151.3 MB/sec    1.00      2.2±0.01ms   151.4 MB/sec
linter/numpy/globals.py      1.00   1151.3±5.40µs   154.8 MB/sec    1.00   1151.4±8.17µs   154.8 MB/sec
linter/pydantic/types.py     1.00      3.9±0.00ms     6.6 MB/sec    1.00      3.9±0.01ms     6.6 MB/sec
```

#### Windows
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.01     10.6±0.28ms     3.9 MB/sec    1.00     10.5±0.28ms     3.9 MB/sec
linter/numpy/ctypeslib.py    1.03      3.5±0.03ms    96.9 MB/sec    1.00      3.4±0.07ms   100.3 MB/sec
linter/numpy/globals.py      1.03  1812.8±25.26µs    98.3 MB/sec    1.00  1765.0±60.59µs   101.0 MB/sec
linter/pydantic/types.py     1.03      5.1±0.31ms     5.0 MB/sec    1.00      5.0±0.11ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @MichaReiser on 2023-03-16 10:07_

---

_@charliermarsh approved on 2023-03-16 15:17_

---

_Merged by @MichaReiser on 2023-03-16 16:50_

---

_Closed by @MichaReiser on 2023-03-16 16:50_

---

_Branch deleted on 2023-03-16 16:50_

---
