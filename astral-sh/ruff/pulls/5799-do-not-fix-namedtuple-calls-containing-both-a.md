```yaml
number: 5799
title: "Do not fix `NamedTuple` calls containing both a list of fields and keywords"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: do-not-fix-incorrect-NamedTuple
created_at: 2023-07-16T05:35:02Z
updated_at: 2023-07-17T01:54:26Z
url: https://github.com/astral-sh/ruff/pull/5799
synced_at: 2026-01-12T03:30:21Z
```

# Do not fix `NamedTuple` calls containing both a list of fields and keywords

---

_Pull request opened by @harupy on 2023-07-16 05:35_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #5794

## Test Plan

<!-- How was it tested? -->

Existing tests

---

_Renamed from "Do not fix NamedTuple calls containing both a list of fields and keywords" to "Do not fix `NamedTuple` calls containing both a list of fields and keywords" by @harupy on 2023-07-16 05:41_

---

_@harupy reviewed on 2023-07-16 05:41_

---

_Review comment by @harupy on `crates/ruff/src/rules/pyupgrade/rules/convert_named_tuple_functional_to_class.rs`:208 on 2023-07-16 05:41_

Is this necessary?

---

_Comment by @github-actions[bot] on 2023-07-16 05:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.02ms     4.3 MB/sec    1.00      9.4±0.01ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1865.9±1.42µs     8.9 MB/sec    1.00  1866.8±15.96µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    209.9±0.36µs    14.1 MB/sec    1.00    210.0±0.37µs    14.1 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.05ms     6.3 MB/sec    1.00      4.0±0.03ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.05ms     3.1 MB/sec    1.04     13.5±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.02      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.4±1.21µs     6.7 MB/sec    1.03    451.9±0.95µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.02      6.0±0.05ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.01ms     6.3 MB/sec    1.03      6.7±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1388.9±1.92µs    12.0 MB/sec    1.05   1460.0±8.89µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.2±0.72µs    19.1 MB/sec    1.09    167.4±0.77µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.02      3.0±0.02ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.08ms     3.8 MB/sec    1.01     10.9±0.11ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.04ms     7.8 MB/sec    1.11      2.4±0.78ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00    243.3±3.64µs    12.1 MB/sec    1.18  288.3±166.76µs    10.2 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.05ms     5.5 MB/sec    1.23      5.7±2.77ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.11ms     2.7 MB/sec    1.02     15.6±0.14ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.03ms     4.1 MB/sec    1.03      4.2±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   491.5±13.03µs     6.0 MB/sec    1.03   505.5±10.28µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.7 MB/sec    1.01      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.04ms     5.2 MB/sec    1.02      8.0±0.10ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1655.5±20.41µs    10.1 MB/sec    1.04  1724.6±22.14µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.1±3.04µs    15.4 MB/sec    1.07    205.2±3.64µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.06ms     7.3 MB/sec    1.02      3.6±0.05ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @harupy on 2023-07-16 11:48_

Hmm, the current implementation doesn't support auto-fix for `NamedTuple` calls where fields are supplied as keywords arguments:

```python
MyType = typing.NamedTuple("MyType", a=int, b=tuple[str, ...])
```

This is fixed to:

```python
class MyType(typing.NamedTuple):
    pass
```

---

_@harupy reviewed on 2023-07-17 00:17_

---

_Review comment by @harupy on `crates/ruff/resources/test/fixtures/pyupgrade/UP014.py`:13 on 2023-07-17 00:17_

Removed `NamedTuple` calls that cause a runtime error.

---

_Comment by @harupy on 2023-07-17 01:30_

@charliermarsh thanks for the update!

---

_Merged by @charliermarsh on 2023-07-17 01:31_

---

_Closed by @charliermarsh on 2023-07-17 01:31_

---

_Branch deleted on 2023-07-17 01:32_

---
