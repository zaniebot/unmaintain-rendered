```yaml
number: 4584
title: "Fix `# isort: split` comment detection in nested blocks"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: charlie/split
created_at: 2023-05-22T15:51:00Z
updated_at: 2023-05-22T16:32:01Z
url: https://github.com/astral-sh/ruff/pull/4584
synced_at: 2026-01-12T03:50:03Z
```

# Fix `# isort: split` comment detection in nested blocks

---

_Pull request opened by @charliermarsh on 2023-05-22 15:51_

## Summary

Given:

```py
if True:
    import C
    import A

    # isort: split

    import D
    import B
```

Our `stmt.end() >= *split` check meant that we were triggering the split when we visited the outer `if True:`, instead of when visiting the inner `import D`.

Closes #4579.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-22 15:51_

---

_Renamed from "Fix # isort: split comment detection in nested blocks" to "Fix `# isort: split` comment detection in nested blocks" by @charliermarsh on 2023-05-22 15:51_

---

_Label `bug` added by @charliermarsh on 2023-05-22 15:51_

---

_Label `isort` added by @charliermarsh on 2023-05-22 15:51_

---

_@MichaReiser approved on 2023-05-22 16:04_

Thanks

---

_Comment by @github-actions[bot] on 2023-05-22 16:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.7±0.73ms     2.1 MB/sec    1.02     20.0±0.61ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.7±0.20ms     3.6 MB/sec    1.00      4.6±0.36ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   574.5±24.58µs     5.1 MB/sec    1.00   572.1±23.92µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.2±0.32ms     3.1 MB/sec    1.00      8.1±0.26ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.2±0.29ms     4.4 MB/sec    1.01      9.2±0.27ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1929.7±48.35µs     8.6 MB/sec    1.00  1935.4±61.28µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    225.7±9.56µs    13.1 MB/sec    1.01   227.5±16.77µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.1±0.16ms     6.2 MB/sec    1.00      4.1±0.17ms     6.2 MB/sec
parser/large/dataset.py                    1.03      7.2±0.22ms     5.7 MB/sec    1.00      6.9±0.24ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.01  1418.6±57.49µs    11.7 MB/sec    1.00  1410.8±73.31µs    11.8 MB/sec
parser/numpy/globals.py                    1.02   143.2±11.99µs    20.6 MB/sec    1.00    139.7±5.56µs    21.1 MB/sec
parser/pydantic/types.py                   1.00      3.1±0.11ms     8.3 MB/sec    1.01      3.1±0.08ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.02     21.5±0.72ms  1938.9 KB/sec     1.00     21.1±0.87ms  1971.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.5±0.29ms     3.0 MB/sec     1.00      5.3±0.27ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   647.0±39.03µs     4.6 MB/sec     1.00   644.9±98.86µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      9.0±0.39ms     2.8 MB/sec     1.00      9.0±0.50ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.02     10.3±0.39ms     3.9 MB/sec     1.00     10.1±0.33ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.07ms     7.9 MB/sec     1.03      2.2±0.11ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   250.5±11.94µs    11.8 MB/sec     1.02   254.5±10.03µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.28ms     5.6 MB/sec     1.02      4.6±0.20ms     5.5 MB/sec
parser/large/dataset.py                    1.00      8.6±0.40ms     4.7 MB/sec     1.00      8.6±0.55ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.05  1630.2±132.55µs    10.2 MB/sec    1.00  1546.3±41.73µs    10.8 MB/sec
parser/numpy/globals.py                    1.00    161.1±6.64µs    18.3 MB/sec     1.01    162.2±7.80µs    18.2 MB/sec
parser/pydantic/types.py                   1.00      3.6±0.13ms     7.2 MB/sec     1.01      3.6±0.36ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-22 16:32_

---

_Closed by @charliermarsh on 2023-05-22 16:32_

---

_Branch deleted on 2023-05-22 16:32_

---
