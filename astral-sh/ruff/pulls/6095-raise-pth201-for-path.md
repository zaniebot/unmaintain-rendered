```yaml
number: 6095
title: "Raise `PTH201` for `Path(\"\")`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: PTH201-empty-string
created_at: 2023-07-26T13:02:48Z
updated_at: 2023-07-26T13:36:30Z
url: https://github.com/astral-sh/ruff/pull/6095
synced_at: 2026-01-12T03:30:22Z
```

# Raise `PTH201` for `Path("")`

---

_Pull request opened by @harupy on 2023-07-26 13:02_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix #6085 

## Test Plan

<!-- How was it tested? -->

A new test case

---

_Renamed from "Raise PTH201 for Path("")" to "Raise `PTH201` for `Path("")`" by @harupy on 2023-07-26 13:03_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:76 on 2023-07-26 13:03_

Maybe `if matches!(value, "" | ".") { let mut diagnostic ... }` ?

---

_@charliermarsh reviewed on 2023-07-26 13:03_

---

_@harupy reviewed on 2023-07-26 13:04_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:76 on 2023-07-26 13:04_

```rs
    if value.is_empty() || value == "." {
        let mut diagnostic = Diagnostic::new(PathConstructorCurrentDirectory, *range);
        if checker.patch(diagnostic.kind.rule()) {
            diagnostic.set_fix(Fix::automatic(Edit::range_deletion(*range)));
        }
        checker.diagnostics.push(diagnostic);
    }
```

Is this easier to read?

---

_@harupy reviewed on 2023-07-26 13:05_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:76 on 2023-07-26 13:05_

Looks good!

---

_Comment by @github-actions[bot] on 2023-07-26 13:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.6±0.07ms     4.7 MB/sec    1.00      8.5±0.06ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01  1714.0±39.79µs     9.7 MB/sec    1.00  1699.1±27.67µs     9.8 MB/sec
formatter/numpy/globals.py                 1.00    189.1±5.56µs    15.6 MB/sec    1.01    191.9±5.90µs    15.4 MB/sec
formatter/pydantic/types.py                1.01      3.7±0.09ms     7.0 MB/sec    1.00      3.6±0.05ms     7.0 MB/sec
linter/all-rules/large/dataset.py          1.00     11.8±0.10ms     3.4 MB/sec    1.00     11.9±0.11ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.01ms     5.5 MB/sec    1.00      3.0±0.01ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    396.8±0.76µs     7.4 MB/sec    1.01    399.9±0.82µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.05ms     4.8 MB/sec    1.00      5.4±0.07ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.8±0.03ms     7.0 MB/sec    1.00      5.8±0.03ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1218.3±3.31µs    13.7 MB/sec    1.01   1229.3±6.89µs    13.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    133.3±1.59µs    22.1 MB/sec    1.00    133.3±0.41µs    22.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.02ms    10.0 MB/sec    1.01      2.6±0.01ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.14ms     4.0 MB/sec    1.00     10.1±0.10ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1926.9±28.81µs     8.6 MB/sec    1.00  1915.3±24.21µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    210.0±4.49µs    14.0 MB/sec    1.03   216.0±12.69µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.05ms     6.1 MB/sec    1.01      4.2±0.08ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.18ms     2.9 MB/sec    1.00     14.2±0.16ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.04ms     4.5 MB/sec    1.01      3.8±0.05ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    443.7±5.50µs     6.6 MB/sec    1.00    444.0±6.00µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.07ms     4.0 MB/sec    1.00      6.4±0.07ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.11ms     5.6 MB/sec    1.00      7.2±0.08ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1488.3±22.38µs    11.2 MB/sec    1.00  1474.8±18.74µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    166.8±3.80µs    17.7 MB/sec    1.00    165.2±3.70µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.05ms     8.0 MB/sec    1.00      3.2±0.04ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-07-26 13:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:76 on 2023-07-26 13:22_

I find it a bit easier to read since it encodes that exact patterns we care about.

---

_Merged by @charliermarsh on 2023-07-26 13:22_

---

_Closed by @charliermarsh on 2023-07-26 13:22_

---

_Branch deleted on 2023-07-26 13:24_

---

_Review comment by @harupy on `crates/ruff/src/rules/flake8_use_pathlib/rules/path_constructor_current_directory.rs`:76 on 2023-07-26 13:24_

Agree, thanks for the suggestion!

---

_@harupy reviewed on 2023-07-26 13:24_

---
