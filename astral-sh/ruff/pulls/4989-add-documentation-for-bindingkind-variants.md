```yaml
number: 4989
title: "Add documentation for `BindingKind` variants"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/binding-docs
created_at: 2023-06-09T18:14:52Z
updated_at: 2023-06-09T18:49:51Z
url: https://github.com/astral-sh/ruff/pull/4989
synced_at: 2026-01-12T03:43:29Z
```

# Add documentation for `BindingKind` variants

---

_Pull request opened by @charliermarsh on 2023-06-09 18:14_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-06-09 18:14_

---

_Merged by @charliermarsh on 2023-06-09 18:32_

---

_Closed by @charliermarsh on 2023-06-09 18:32_

---

_Branch deleted on 2023-06-09 18:32_

---

_Comment by @github-actions[bot] on 2023-06-09 18:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      7.0±0.01ms     5.8 MB/sec    1.00      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.02   1418.8±4.21µs    11.7 MB/sec    1.00   1397.2±3.44µs    11.9 MB/sec
formatter/numpy/globals.py                 1.01    140.6±0.25µs    21.0 MB/sec    1.00    139.2±0.19µs    21.2 MB/sec
formatter/pydantic/types.py                1.01      2.8±0.01ms     9.0 MB/sec    1.00      2.8±0.02ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.01     15.0±0.05ms     2.7 MB/sec    1.00     14.8±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01    371.6±0.79µs     7.9 MB/sec    1.00    369.7±0.81µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.01ms     5.5 MB/sec    1.00      7.3±0.05ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1559.6±26.99µs    10.7 MB/sec    1.00   1540.5±3.91µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    167.0±0.22µs    17.7 MB/sec    1.00    165.4±0.52µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.00ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.0±0.12ms     5.1 MB/sec    1.00      7.9±0.13ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1605.7±27.68µs    10.4 MB/sec    1.01  1625.1±27.79µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    152.4±3.07µs    19.4 MB/sec    1.01    153.4±3.62µs    19.2 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     7.9 MB/sec    1.02      3.3±0.09ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     17.8±0.30ms     2.3 MB/sec    1.01     18.1±0.36ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.5±0.09ms     3.7 MB/sec    1.00      4.4±0.08ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02   521.9±17.49µs     5.7 MB/sec    1.00    510.4±7.22µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.5±0.14ms     3.4 MB/sec    1.00      7.4±0.11ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.10ms     4.8 MB/sec    1.00      8.5±0.10ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1800.7±18.67µs     9.2 MB/sec    1.00  1806.8±25.05µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.7±4.53µs    14.3 MB/sec    1.01    206.9±5.02µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.6 MB/sec    1.01      3.9±0.04ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
