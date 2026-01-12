```yaml
number: 5330
title: "Add documentation missing docstring rules (`D1XX`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: missing-docstring
created_at: 2023-06-23T11:55:19Z
updated_at: 2023-07-10T09:55:07Z
url: https://github.com/astral-sh/ruff/pull/5330
synced_at: 2026-01-12T03:36:54Z
```

# Add documentation missing docstring rules (`D1XX`)

---

_Pull request opened by @tjkuson on 2023-06-23 11:55_

## Summary

Add documentation to the `D1XX` rules that flag missing docstrings. 

The examples are quite long and docstrings practices vary a lot between projects, so I thought it would be best that the documentation for these rules be their own PR separate to the other `pydocstyle` rules.

Related to #2646.

## Test Plan

`python scripts/check_docs_formatted.py`


---

_Comment by @github-actions[bot] on 2023-06-23 12:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      6.7±0.02ms     6.1 MB/sec    1.00      6.6±0.03ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1546.7±2.17µs    10.8 MB/sec    1.00   1543.6±2.39µs    10.8 MB/sec
formatter/numpy/globals.py                 1.02    187.7±0.33µs    15.7 MB/sec    1.00    184.6±2.53µs    16.0 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.00ms     7.4 MB/sec    1.00      3.4±0.00ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.02     13.3±0.07ms     3.1 MB/sec    1.00     13.0±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.6±0.71µs     6.9 MB/sec    1.00    425.4±0.62µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.04ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.2 MB/sec    1.00      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1433.9±2.97µs    11.6 MB/sec    1.01   1443.0±2.06µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.8±0.33µs    18.2 MB/sec    1.00    162.2±1.13µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.13      9.7±0.36ms     4.2 MB/sec    1.00      8.5±0.33ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.10      2.2±0.19ms     7.7 MB/sec    1.00  1973.9±91.37µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00   243.7±15.99µs    12.1 MB/sec    1.06   257.9±47.33µs    11.4 MB/sec
formatter/pydantic/types.py                1.03      4.6±0.23ms     5.6 MB/sec    1.00      4.4±0.21ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.02     17.0±0.52ms     2.4 MB/sec    1.00     16.7±0.54ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.6±0.18ms     3.7 MB/sec    1.00      4.5±0.18ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.03   564.6±27.72µs     5.2 MB/sec    1.00   549.2±29.34µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.6±0.35ms     3.3 MB/sec    1.00      7.6±0.27ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      8.9±0.25ms     4.6 MB/sec    1.00      8.7±0.25ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1910.6±85.03µs     8.7 MB/sec    1.00  1890.1±94.96µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.01   222.3±11.72µs    13.3 MB/sec    1.00   220.8±12.59µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.0±0.17ms     6.3 MB/sec    1.00      3.9±0.16ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @tjkuson on 2023-06-23 16:09_

Added references to the [NumPy](https://numpydoc.readthedocs.io/en/latest/format.html#style-guide) and [Google](https://google.github.io/styleguide/pyguide.html#s3.8-comments-and-docstrings) style guides. These third-party style guides available as [`convention`](https://beta.ruff.rs/docs/settings/#pydocstyle-convention) options, so I felt it would be a good idea to use them in the examples.

---

_Label `documentation` added by @charliermarsh on 2023-06-26 14:38_

---

_Merged by @charliermarsh on 2023-06-26 14:44_

---

_Closed by @charliermarsh on 2023-06-26 14:44_

---

_Branch deleted on 2023-07-10 09:55_

---
