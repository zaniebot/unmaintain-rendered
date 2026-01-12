```yaml
number: 5165
title: "Format `continue` statement"
type: pull_request
state: merged
author: cnpryer
labels: []
assignees: []
merged: true
base: main
head: stmt-continue
created_at: 2023-06-17T17:00:36Z
updated_at: 2023-06-18T11:46:07Z
url: https://github.com/astral-sh/ruff/pull/5165
synced_at: 2026-01-12T15:55:17Z
```

# Format `continue` statement

---

_@cnpryer_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Format `continue` statement.

## Test Plan

`continue` is used already in some tests, but if a new test is needed I could add it.


---

_Comment by @github-actions[bot] on 2023-06-17 17:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.9±0.08ms     5.9 MB/sec    1.01      6.9±0.01ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1401.3±1.86µs    11.9 MB/sec    1.01  1415.4±10.61µs    11.8 MB/sec
formatter/numpy/globals.py                 1.00    136.4±0.36µs    21.6 MB/sec    1.02    138.5±0.38µs    21.3 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.1 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.01     14.4±0.04ms     2.8 MB/sec    1.00     14.2±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.8 MB/sec    1.00      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.8±1.42µs     8.1 MB/sec    1.00    364.0±1.60µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.02ms     4.2 MB/sec    1.00      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.02ms     5.6 MB/sec    1.00      7.3±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1544.3±3.68µs    10.8 MB/sec    1.00   1527.5±5.98µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.3±0.22µs    17.9 MB/sec    1.00    165.5±2.74µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.00ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.02ms     5.2 MB/sec    1.01      7.9±0.02ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1581.6±7.34µs    10.5 MB/sec    1.01  1603.7±10.29µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    155.3±1.77µs    19.0 MB/sec    1.01    157.5±2.83µs    18.7 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.04ms     8.0 MB/sec    1.01      3.2±0.03ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.07ms     2.6 MB/sec    1.00     15.9±0.06ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.02ms     4.0 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    429.2±4.12µs     6.9 MB/sec    1.00    425.4±5.64µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.03ms     3.6 MB/sec    1.00      7.0±0.03ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.02ms     4.9 MB/sec    1.00      8.3±0.03ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1749.4±12.98µs     9.5 MB/sec    1.00  1738.3±11.39µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.2±1.70µs    15.8 MB/sec    1.00    186.6±1.36µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.8 MB/sec    1.00      3.8±0.01ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__comments2_py.snap`:360 on 2023-06-17 17:11_

The ruff and black output both spit out:

```py
while True:
    if False:
        continue

        # and round and round we go
    # and round and round we go
```

cc: https://discord.com/channels/1039017663004942429/1073384953741574217/1119620838161928342

I wanna wrap my head around this, but I originally read this as "the output excludes the comment"

---

_@cnpryer reviewed on 2023-06-17 17:11_

---

_@cnpryer reviewed on 2023-06-17 20:34_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__comments2_py.snap`:360 on 2023-06-17 20:34_

Makes sense. "This is no longer in the diff" not "the output excludes this".

---

_Marked ready for review by @cnpryer on 2023-06-17 20:35_

---

_@charliermarsh approved on 2023-06-17 20:46_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/snapshots/ruff_python_formatter__tests__black_test__comments2_py.snap`:360 on 2023-06-17 21:13_

yeah the diff of diffs is confusing unfortunately

---

_@konstin reviewed on 2023-06-17 21:13_

---

_@konstin reviewed on 2023-06-18 11:18_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/stmt_continue.rs`:10 on 2023-06-18 11:18_

```suggestion
    fn fmt_fields(&self, _item: &StmtContinue, f: &mut PyFormatter) -> FormatResult<()> {
```

---

_@konstin approved on 2023-06-18 11:19_

---

_Merged by @konstin on 2023-06-18 11:25_

---

_Closed by @konstin on 2023-06-18 11:25_

---

_Branch deleted on 2023-06-18 11:29_

---

_@cnpryer reviewed on 2023-06-18 11:29_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/statement/stmt_continue.rs`:10 on 2023-06-18 11:29_

Oof

---
