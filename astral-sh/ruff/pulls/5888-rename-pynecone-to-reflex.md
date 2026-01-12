```yaml
number: 5888
title: "Rename `Pynecone` to `Reflex`"
type: pull_request
state: merged
author: odiseo0
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2023-07-19T16:37:38Z
updated_at: 2023-07-19T17:04:20Z
url: https://github.com/astral-sh/ruff/pull/5888
synced_at: 2026-01-12T03:30:22Z
```

# Rename `Pynecone` to `Reflex`

---

_Pull request opened by @odiseo0 on 2023-07-19 16:37_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

They just changed the name to `Reflex`

## Test Plan

Nothing


---

_@konstin approved on 2023-07-19 16:46_

---

_Merged by @konstin on 2023-07-19 16:46_

---

_Closed by @konstin on 2023-07-19 16:46_

---

_Comment by @github-actions[bot] on 2023-07-19 16:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.04ms     4.3 MB/sec    1.00      9.3±0.04ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   1873.0±2.19µs     8.9 MB/sec    1.00   1860.9±3.12µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    207.6±0.36µs    14.2 MB/sec    1.00    207.7±1.31µs    14.2 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.02ms     6.3 MB/sec    1.00      4.0±0.02ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.08ms     3.1 MB/sec    1.00     13.1±0.06ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.1±1.10µs     6.8 MB/sec    1.00    433.5±2.80µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.03ms     6.1 MB/sec    1.00      6.6±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1423.6±2.49µs    11.7 MB/sec    1.00   1411.2±2.72µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    157.6±0.28µs    18.7 MB/sec    1.00    156.7±0.33µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02     12.5±0.48ms     3.3 MB/sec    1.00     12.3±0.47ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.10ms     6.8 MB/sec    1.02      2.5±0.13ms     6.6 MB/sec
formatter/numpy/globals.py                 1.00   281.2±11.71µs    10.5 MB/sec    1.05   295.9±15.97µs    10.0 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.20ms     4.8 MB/sec    1.02      5.4±0.26ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.01     17.9±0.48ms     2.3 MB/sec    1.00     17.7±0.65ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.8±0.19ms     3.5 MB/sec    1.00      4.7±0.18ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   568.2±21.84µs     5.2 MB/sec    1.03   584.7±22.83µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.26ms     3.1 MB/sec    1.02      8.3±0.30ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.31ms     4.3 MB/sec    1.02      9.6±0.40ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1945.9±83.98µs     8.6 MB/sec    1.00  1945.2±68.00µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    235.4±9.71µs    12.5 MB/sec    1.01   238.3±12.96µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.2±0.16ms     6.1 MB/sec    1.00      4.2±0.18ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
