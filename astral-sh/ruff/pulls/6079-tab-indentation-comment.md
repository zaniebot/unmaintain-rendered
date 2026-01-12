```yaml
number: 6079
title: tab indentation comment
type: pull_request
state: merged
author: arembridge
labels:
  - documentation
assignees: []
merged: true
base: main
head: tab-indentations-rule
created_at: 2023-07-25T21:32:04Z
updated_at: 2023-07-25T23:36:38Z
url: https://github.com/astral-sh/ruff/pull/6079
synced_at: 2026-01-12T15:55:20Z
```

# tab indentation comment

---

_@arembridge_

## Summary

Updated doc comment for `tab_indentation.rs`.  Online docs also benefit from this update.

## Test Plan

Checked docs via [mkdocs](https://github.com/astral-sh/ruff/blob/389fe13c934fe73679474006412b1eded4a2cad0/CONTRIBUTING.md?plain=1#L267-L296)


---

_Comment by @github-actions[bot] on 2023-07-25 22:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.1±0.05ms     3.7 MB/sec    1.00     10.9±0.04ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.01ms     7.5 MB/sec    1.00      2.2±0.01ms     7.5 MB/sec
formatter/numpy/globals.py                 1.00    247.3±1.10µs    11.9 MB/sec    1.00    248.5±4.82µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.02ms     5.3 MB/sec    1.02      4.9±0.03ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.01     15.2±0.09ms     2.7 MB/sec    1.00     15.1±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.02ms     4.3 MB/sec    1.00      3.9±0.01ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    505.1±3.79µs     5.8 MB/sec    1.00    506.1±1.21µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.03ms     3.7 MB/sec    1.00      7.0±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.02      8.0±0.04ms     5.1 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1704.4±6.45µs     9.8 MB/sec    1.00   1690.3±2.46µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    189.5±0.77µs    15.6 MB/sec    1.00    187.6±0.50µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.9±0.14ms     3.7 MB/sec    1.00     10.8±0.10ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.05ms     7.8 MB/sec    1.00      2.1±0.04ms     7.8 MB/sec
formatter/numpy/globals.py                 1.00    241.8±7.62µs    12.2 MB/sec    1.01    243.5±7.92µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.07ms     5.4 MB/sec    1.00      4.7±0.09ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.04     15.6±0.23ms     2.6 MB/sec    1.00     15.0±0.15ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.08ms     4.1 MB/sec    1.00      4.0±0.04ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.02    493.4±9.84µs     6.0 MB/sec    1.00    483.1±8.56µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.09ms     3.6 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.03      8.2±0.19ms     5.0 MB/sec    1.00      8.0±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1679.6±18.33µs     9.9 MB/sec    1.00  1673.3±22.12µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.8±2.83µs    15.1 MB/sec    1.00    194.4±3.46µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.03ms     7.1 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-07-25 23:06_

---

_Comment by @charliermarsh on 2023-07-25 23:06_

Thx!

---

_Merged by @charliermarsh on 2023-07-25 23:14_

---

_Closed by @charliermarsh on 2023-07-25 23:14_

---
