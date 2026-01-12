```yaml
number: 3602
title: "ci: fix check_ecosystem"
type: pull_request
state: merged
author: henryiii
labels: []
assignees: []
merged: true
base: main
head: patch-4
created_at: 2023-03-18T22:32:51Z
updated_at: 2023-03-18T23:03:12Z
url: https://github.com/astral-sh/ruff/pull/3602
synced_at: 2026-01-12T04:39:45Z
```

# ci: fix check_ecosystem

---

_Pull request opened by @henryiii on 2023-03-18 22:32_

Broken by #3590.

---

_Comment by @github-actions[bot] on 2023-03-18 22:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.13ms     2.9 MB/sec    1.01     14.4±0.14ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.05ms     4.5 MB/sec    1.00      3.7±0.04ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    506.4±1.60µs     5.8 MB/sec    1.00    507.8±1.13µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.06ms     4.1 MB/sec    1.01      6.3±0.04ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.06ms     5.3 MB/sec    1.02      7.9±0.04ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1695.2±4.19µs     9.8 MB/sec    1.01   1712.4±2.38µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.2±0.57µs    15.2 MB/sec    1.00    194.6±1.21µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.01ms     7.2 MB/sec    1.02      3.6±0.02ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.7±0.27ms     2.6 MB/sec    1.00     15.5±0.05ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     3.9 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    465.8±6.89µs     6.3 MB/sec    1.00    463.8±4.99µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.16ms     3.7 MB/sec    1.00      6.9±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.03ms     4.7 MB/sec    1.00      8.7±0.03ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1853.1±7.92µs     9.0 MB/sec    1.00  1849.3±35.53µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.2±4.29µs    15.4 MB/sec    1.01    192.3±5.03µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.02ms     6.4 MB/sec    1.00      4.0±0.10ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-03-18 23:03_

---

_Closed by @charliermarsh on 2023-03-18 23:03_

---

_Comment by @charliermarsh on 2023-03-18 23:03_

Thank you :)

---
