```yaml
number: 4735
title: Rename top-of-file to start-of-file
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/top
created_at: 2023-05-30T21:45:24Z
updated_at: 2023-05-30T22:15:57Z
url: https://github.com/astral-sh/ruff/pull/4735
synced_at: 2026-01-12T03:50:03Z
```

# Rename top-of-file to start-of-file

---

_Pull request opened by @charliermarsh on 2023-05-30 21:45_

## Summary

Just felt inconsistent with some other names.


---

_Merged by @charliermarsh on 2023-05-30 21:53_

---

_Closed by @charliermarsh on 2023-05-30 21:53_

---

_Branch deleted on 2023-05-30 21:53_

---

_Comment by @github-actions[bot] on 2023-05-30 21:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.11     18.4±0.94ms     2.2 MB/sec     1.00     16.6±0.77ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.15ms     4.0 MB/sec     1.06      4.4±0.25ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.07   540.0±27.33µs     5.5 MB/sec     1.00   506.6±43.58µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.3±0.34ms     3.5 MB/sec     1.00      7.0±0.44ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.11      8.7±0.49ms     4.7 MB/sec     1.00      7.8±0.25ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1783.4±106.22µs     9.3 MB/sec    1.00  1753.9±66.02µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   216.5±15.13µs    13.6 MB/sec     1.00   216.6±14.01µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.18ms     6.9 MB/sec     1.00      3.6±0.13ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.3±0.54ms     6.5 MB/sec     1.07      6.7±0.44ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1211.8±70.42µs    13.7 MB/sec     1.00  1215.6±51.34µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    126.8±6.10µs    23.3 MB/sec     1.01   128.4±10.04µs    23.0 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.13ms     9.4 MB/sec     1.03      2.8±0.16ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     21.2±0.76ms  1963.5 KB/sec    1.00     21.0±0.68ms  1979.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.4±0.19ms     3.1 MB/sec    1.01      5.4±0.20ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   627.3±23.72µs     4.7 MB/sec    1.01   633.6±30.45µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.31ms     2.8 MB/sec    1.01      9.0±0.30ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.4±0.36ms     3.9 MB/sec    1.00     10.4±0.30ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.5 MB/sec    1.00      2.2±0.10ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   259.4±12.52µs    11.4 MB/sec    1.02   263.9±10.94µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.16ms     5.4 MB/sec    1.02      4.8±0.22ms     5.3 MB/sec
parser/large/dataset.py                    1.00      8.3±0.18ms     4.9 MB/sec    1.41     11.6±0.38ms     3.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1605.7±53.69µs    10.4 MB/sec    1.32      2.1±0.08ms     7.8 MB/sec
parser/numpy/globals.py                    1.00    161.8±6.95µs    18.2 MB/sec    1.27   205.6±12.19µs    14.4 MB/sec
parser/pydantic/types.py                   1.00      3.6±0.09ms     7.1 MB/sec    1.34      4.8±0.21ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
