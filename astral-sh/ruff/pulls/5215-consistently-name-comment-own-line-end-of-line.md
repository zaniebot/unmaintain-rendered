```yaml
number: 5215
title: "Consistently name comment own line/end-of-line `line_position()`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: comment_line_position
created_at: 2023-06-20T16:02:47Z
updated_at: 2023-06-21T09:04:58Z
url: https://github.com/astral-sh/ruff/pull/5215
synced_at: 2026-01-12T03:43:30Z
```

# Consistently name comment own line/end-of-line `line_position()`

---

_Pull request opened by @konstin on 2023-06-20 16:02_

## Summary

Previously, `DecoratedComment` used `text_position()` and `SourceComment` used `position()`. This PR unifies this to `line_position` everywhere.

## Test Plan

This is a rename refactoring.

---

_Review requested from @MichaReiser by @konstin on 2023-06-20 16:02_

---

_@MichaReiser approved on 2023-06-20 16:12_

---

_Comment by @github-actions[bot] on 2023-06-20 16:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      8.6±0.33ms     4.8 MB/sec     1.20     10.3±0.46ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1783.9±87.63µs     9.3 MB/sec     1.16      2.1±0.13ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00   177.8±12.92µs    16.6 MB/sec     1.11    197.4±9.18µs    14.9 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.22ms     7.1 MB/sec     1.16      4.1±0.17ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     18.6±0.47ms     2.2 MB/sec     1.06     19.7±0.95ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.5±0.24ms     3.7 MB/sec     1.00      4.4±0.19ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   590.1±47.11µs     5.0 MB/sec     1.00   581.8±20.98µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.65ms     3.1 MB/sec     1.01      8.3±0.38ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.39ms     4.5 MB/sec     1.00      9.1±0.37ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1971.9±146.60µs     8.4 MB/sec    1.00  1930.8±65.78µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   228.6±13.26µs    12.9 MB/sec     1.00   229.1±10.65µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.23ms     6.2 MB/sec     1.00      4.1±0.17ms     6.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.1±0.22ms     4.5 MB/sec    1.15     10.5±0.21ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1871.5±51.25µs     8.9 MB/sec    1.12      2.1±0.07ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00   186.5±10.44µs    15.8 MB/sec    1.11   206.5±13.18µs    14.3 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.09ms     6.9 MB/sec    1.15      4.2±0.13ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     18.5±0.37ms     2.2 MB/sec    1.00     18.3±0.27ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.11ms     3.5 MB/sec    1.04      4.9±0.11ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   595.6±17.36µs     5.0 MB/sec    1.00   594.2±20.42µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.18ms     3.1 MB/sec    1.02      8.3±0.19ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.6±0.17ms     4.3 MB/sec    1.00      9.6±0.24ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.06ms     8.1 MB/sec    1.00      2.0±0.04ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    234.4±9.81µs    12.6 MB/sec    1.01    236.8±8.60µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.10ms     5.9 MB/sec    1.01      4.3±0.11ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `internal` added by @konstin on 2023-06-21 09:04_

---

_Label `formatter` added by @konstin on 2023-06-21 09:04_

---

_Merged by @konstin on 2023-06-21 09:04_

---

_Closed by @konstin on 2023-06-21 09:04_

---

_Branch deleted on 2023-06-21 09:04_

---
