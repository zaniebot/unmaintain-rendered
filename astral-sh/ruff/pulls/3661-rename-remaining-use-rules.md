```yaml
number: 3661
title: "Rename remaining `use-*` rules"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/remove-use
created_at: 2023-03-22T02:31:47Z
updated_at: 2023-03-22T15:36:04Z
url: https://github.com/astral-sh/ruff/pull/3661
synced_at: 2026-01-12T15:55:13Z
```

# Rename remaining `use-*` rules

---

_@charliermarsh_

Closes: #2902.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-22 02:31_

---

_Comment by @github-actions[bot] on 2023-03-22 03:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     13.5±0.05ms     3.0 MB/sec    1.00     13.5±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    486.6±5.78µs     6.1 MB/sec    1.00    484.6±4.43µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.03ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.02ms     5.6 MB/sec    1.00      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1611.3±5.78µs    10.3 MB/sec    1.00   1607.5±1.05µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.4±0.42µs    16.7 MB/sec    1.01    178.2±0.66µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.00      3.4±0.00ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     20.8±1.01ms  1998.7 KB/sec    1.00     20.2±0.77ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.37ms     3.2 MB/sec    1.02      5.3±0.20ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   630.1±29.16µs     4.7 MB/sec    1.06   667.2±22.24µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.37ms     3.1 MB/sec    1.07      8.8±0.46ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.3±0.49ms     3.9 MB/sec    1.00     10.2±0.38ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.20ms     7.3 MB/sec    1.01      2.3±0.22ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    266.8±9.23µs    11.1 MB/sec    1.01   269.8±15.90µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.18ms     5.5 MB/sec    1.06      4.9±0.16ms     5.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-03-22 07:57_

Looks good. We may need to do another pass in the future. I'm in process of reviewing each rule name and identify rules that can be merged (e.g. should `LoadBeforeGlobalDeclaration` be merged with `undeclared-variable`?

---

_Merged by @charliermarsh on 2023-03-22 15:36_

---

_Closed by @charliermarsh on 2023-03-22 15:36_

---

_Branch deleted on 2023-03-22 15:36_

---
