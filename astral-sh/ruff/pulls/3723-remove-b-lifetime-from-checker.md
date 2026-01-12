```yaml
number: 3723
title: "Remove `'b` lifetime from `Checker`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/lifetime
created_at: 2023-03-24T21:35:52Z
updated_at: 2023-03-24T22:05:13Z
url: https://github.com/astral-sh/ruff/pull/3723
synced_at: 2026-01-12T04:39:45Z
```

# Remove `'b` lifetime from `Checker`

---

_Pull request opened by @charliermarsh on 2023-03-24 21:35_

_No description provided._

---

_Marked ready for review by @charliermarsh on 2023-03-24 21:35_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-24 21:35_

---

_@MichaReiser approved on 2023-03-24 21:37_

---

_Comment by @MichaReiser on 2023-03-24 21:37_

Thanks 🙏 

---

_Merged by @charliermarsh on 2023-03-24 21:42_

---

_Closed by @charliermarsh on 2023-03-24 21:42_

---

_Branch deleted on 2023-03-24 21:42_

---

_Comment by @github-actions[bot] on 2023-03-24 21:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.02ms     2.9 MB/sec    1.01     14.3±0.08ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.2±2.21µs     6.8 MB/sec    1.00    434.9±1.59µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.0 MB/sec    1.00      6.3±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.03ms     5.3 MB/sec    1.01      7.8±0.04ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1687.6±4.51µs     9.9 MB/sec    1.00   1693.0±3.08µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.2±0.50µs    16.7 MB/sec    1.01    178.2±0.49µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.01      3.6±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.2±0.10ms     2.7 MB/sec    1.01     15.2±0.13ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.01      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    458.7±7.70µs     6.4 MB/sec    1.00   456.4±14.44µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.08ms     3.8 MB/sec    1.01      6.8±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.05ms     5.0 MB/sec    1.01      8.2±0.03ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1769.4±16.06µs     9.4 MB/sec    1.00  1764.4±15.32µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.9±1.92µs    16.0 MB/sec    1.02   187.0±12.74µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.7 MB/sec    1.01      3.9±0.05ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
