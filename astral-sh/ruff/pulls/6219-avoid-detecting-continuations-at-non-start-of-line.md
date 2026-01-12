```yaml
number: 6219
title: Avoid detecting continuations at non-start-of-line
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/index
created_at: 2023-08-01T03:41:53Z
updated_at: 2023-08-01T05:35:21Z
url: https://github.com/astral-sh/ruff/pull/6219
synced_at: 2026-01-12T15:55:20Z
```

# Avoid detecting continuations at non-start-of-line

---

_@charliermarsh_

## Summary

Previously, given:

```python
a = \
  5;
```

When detecting continuations starting at the offset of the `;`, we'd flag the previous line as a continuation. We should only flag a continuation if there isn't leading content prior to the offset.

Closes https://github.com/astral-sh/ruff/issues/6214



---

_Label `bug` added by @charliermarsh on 2023-08-01 03:41_

---

_Comment by @github-actions[bot] on 2023-08-01 03:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.11ms     4.1 MB/sec    1.00      9.9±0.11ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1962.6±16.56µs     8.5 MB/sec    1.00  1969.8±15.24µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    213.4±2.70µs    13.8 MB/sec    1.01    214.6±3.61µs    13.8 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.04ms     6.0 MB/sec    1.00      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.02     13.3±0.10ms     3.0 MB/sec    1.00     13.1±0.13ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.03ms     5.0 MB/sec    1.00      3.3±0.03ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    458.3±3.55µs     6.4 MB/sec    1.00    457.2±6.02µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.0±0.08ms     4.2 MB/sec    1.00      5.9±0.06ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.06ms     5.7 MB/sec    1.00      7.1±0.05ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1497.2±15.34µs    11.1 MB/sec    1.00  1490.5±13.52µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    164.8±1.17µs    17.9 MB/sec    1.00    163.2±3.69µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.1 MB/sec    1.00      3.1±0.05ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     12.0±0.38ms     3.4 MB/sec    1.00     11.9±0.40ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.3±0.08ms     7.2 MB/sec    1.00      2.3±0.08ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00   248.8±16.09µs    11.9 MB/sec    1.07   266.3±25.45µs    11.1 MB/sec
formatter/pydantic/types.py                1.02      5.2±0.22ms     4.9 MB/sec    1.00      5.1±0.21ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.00     16.0±0.49ms     2.5 MB/sec    1.01     16.1±0.49ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.18ms     4.0 MB/sec    1.01      4.2±0.13ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02   516.1±30.97µs     5.7 MB/sec    1.00   506.4±20.33µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.23ms     3.5 MB/sec    1.00      7.2±0.25ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      9.0±0.29ms     4.5 MB/sec    1.00      8.8±0.25ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1786.9±75.78µs     9.3 MB/sec    1.03  1839.3±87.64µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.03   204.2±11.78µs    14.5 MB/sec    1.00   198.7±11.41µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.18ms     6.6 MB/sec    1.00      3.9±0.12ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-01 04:20_

---

_Closed by @charliermarsh on 2023-08-01 04:20_

---

_Branch deleted on 2023-08-01 04:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_index/src/indexer.rs`:239 on 2023-08-01 05:34_

Nit: this is not the end of the previous line (its the full end because it includes the newline) but the start of the current lint

Should we create a helper for this on Locator. I think it's the second time in 7 days that you wrote this exact code

---

_@MichaReiser reviewed on 2023-08-01 05:35_

---
