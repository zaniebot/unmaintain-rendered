```yaml
number: 4787
title: Complete the Pyflakes documention
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: f7
created_at: 2023-06-01T15:12:48Z
updated_at: 2023-07-10T09:55:32Z
url: https://github.com/astral-sh/ruff/pull/4787
synced_at: 2026-01-12T15:55:16Z
```

# Complete the Pyflakes documention

---

_@tjkuson_

## Summary

Completes the documentation for the Pyflakes rules.

Related to #2646.

## Test Plan

`python3 scripts/generate_mkdocs.py && python3 scripts/check_docs_formatted.py`

---

_Marked ready for review by @tjkuson on 2023-06-01 15:12_

---

_Comment by @github-actions[bot] on 2023-06-01 15:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.08ms     2.9 MB/sec    1.00     14.2±0.10ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.7±0.80µs     7.0 MB/sec    1.00    422.9±0.48µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.4 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.01      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1496.9±1.74µs    11.1 MB/sec    1.01   1512.1±6.48µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.7±0.51µs    17.4 MB/sec    1.01    171.4±1.35µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.01      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.8 MB/sec    1.03      5.3±0.01ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1018.1±0.57µs    16.4 MB/sec    1.02   1040.7±1.69µs    16.0 MB/sec
parser/numpy/globals.py                    1.00    105.7±0.32µs    27.9 MB/sec    1.02    107.7±0.21µs    27.4 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.01      2.3±0.00ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.20ms     2.4 MB/sec    1.00     16.6±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.01      4.2±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    499.0±7.20µs     5.9 MB/sec    1.00   497.9±11.70µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.6 MB/sec    1.00      7.0±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.08ms     5.0 MB/sec    1.00      8.2±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1747.1±18.04µs     9.5 MB/sec    1.01  1769.0±24.15µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.3±3.09µs    14.7 MB/sec    1.01    203.2±4.54µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.9 MB/sec    1.01      3.8±0.04ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.4±0.05ms     6.3 MB/sec    1.00      6.4±0.05ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.01  1221.5±13.38µs    13.6 MB/sec    1.00  1208.4±11.92µs    13.8 MB/sec
parser/numpy/globals.py                    1.01    125.6±2.13µs    23.5 MB/sec    1.00    123.9±2.00µs    23.8 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.03ms     9.2 MB/sec    1.00      2.7±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @MichaReiser on 2023-06-01 16:04_

---

_Label `documentation` added by @charliermarsh on 2023-06-01 20:14_

---

_Comment by @charliermarsh on 2023-06-01 20:14_

Thanks!

---

_Merged by @charliermarsh on 2023-06-01 20:25_

---

_Closed by @charliermarsh on 2023-06-01 20:25_

---

_Branch deleted on 2023-07-10 09:55_

---
