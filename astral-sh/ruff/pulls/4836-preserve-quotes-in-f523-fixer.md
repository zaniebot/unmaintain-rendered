```yaml
number: 4836
title: Preserve quotes in F523 fixer
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/quotes
created_at: 2023-06-03T19:43:38Z
updated_at: 2023-06-03T20:15:36Z
url: https://github.com/astral-sh/ruff/pull/4836
synced_at: 2026-01-12T03:50:04Z
```

# Preserve quotes in F523 fixer

---

_Pull request opened by @charliermarsh on 2023-06-03 19:43_

Closes #4823.


---

_Label `bug` added by @charliermarsh on 2023-06-03 19:43_

---

_Merged by @charliermarsh on 2023-06-03 19:53_

---

_Closed by @charliermarsh on 2023-06-03 19:53_

---

_Branch deleted on 2023-06-03 19:53_

---

_Comment by @github-actions[bot] on 2023-06-03 19:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.06ms     2.9 MB/sec    1.06     15.0±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.07ms     4.7 MB/sec    1.03      3.7±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.1±1.31µs     6.8 MB/sec    1.00    430.6±2.68µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.04ms     4.1 MB/sec    1.01      6.3±0.03ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.03ms     5.7 MB/sec    1.00      7.1±0.06ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1516.2±5.55µs    11.0 MB/sec    1.01   1533.9±5.89µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.0±0.29µs    17.3 MB/sec    1.01    172.7±1.37µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.01      3.2±0.02ms     8.0 MB/sec
parser/large/dataset.py                    1.00      5.2±0.15ms     7.8 MB/sec    1.02      5.3±0.01ms     7.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1013.4±0.53µs    16.4 MB/sec    1.01   1021.9±3.24µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    105.2±1.70µs    28.0 MB/sec    1.01    106.2±1.17µs    27.8 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.01ms    11.4 MB/sec    1.01      2.2±0.00ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.16ms     2.5 MB/sec    1.00     16.5±0.14ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     3.9 MB/sec    1.00      4.2±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.8±9.29µs     5.9 MB/sec    1.01   503.7±13.92µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.6 MB/sec    1.00      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     5.0 MB/sec    1.01      8.2±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1745.4±32.02µs     9.5 MB/sec    1.01  1759.7±19.87µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.2±4.19µs    14.7 MB/sec    1.02    205.9±7.19µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.9 MB/sec    1.01      3.7±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.4±0.07ms     6.3 MB/sec    1.00      6.4±0.09ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1209.5±24.85µs    13.8 MB/sec    1.00  1214.6±27.76µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    124.6±2.37µs    23.7 MB/sec    1.00    124.9±3.52µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.05ms     9.3 MB/sec    1.00      2.7±0.05ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
