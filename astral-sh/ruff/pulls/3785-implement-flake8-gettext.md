```yaml
number: 3785
title: "Implement `flake8-gettext`"
type: pull_request
state: merged
author: leiserfg
labels:
  - plugin
assignees: []
merged: true
base: main
head: implement-flake8-gettext
created_at: 2023-03-28T21:06:44Z
updated_at: 2023-03-28T23:47:21Z
url: https://github.com/astral-sh/ruff/pull/3785
synced_at: 2026-01-12T15:55:13Z
```

# Implement `flake8-gettext`

---

_@leiserfg_

Adds rules regarding incorrect usage of gettext functions
Closes #3294.


---

_Comment by @github-actions[bot] on 2023-03-28 21:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.9±0.08ms     2.9 MB/sec    1.00     13.8±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    461.8±0.72µs     6.4 MB/sec    1.00    461.9±1.16µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.02ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1603.7±2.67µs    10.4 MB/sec    1.00   1606.4±5.73µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.7±0.23µs    16.4 MB/sec    1.00    180.5±1.15µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.8±0.07ms     2.6 MB/sec    1.00     15.6±0.07ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.04ms     4.0 MB/sec    1.00      4.1±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.8±4.63µs     6.7 MB/sec    1.00    438.2±4.36µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.03ms     3.7 MB/sec    1.00      6.8±0.02ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      8.3±0.03ms     4.9 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1796.9±23.45µs     9.3 MB/sec    1.00  1788.3±12.79µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    189.8±1.37µs    15.5 MB/sec    1.00    188.8±5.89µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.8±0.02ms     6.7 MB/sec    1.00      3.8±0.03ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `plugin` added by @charliermarsh on 2023-03-28 23:26_

---

_Renamed from "Implement flake8-gettext" to "Implement `flake8-gettext`" by @charliermarsh on 2023-03-28 23:26_

---

_Merged by @charliermarsh on 2023-03-28 23:32_

---

_Closed by @charliermarsh on 2023-03-28 23:32_

---
