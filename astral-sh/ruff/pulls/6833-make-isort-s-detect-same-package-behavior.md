```yaml
number: 6833
title: "Make isort's `detect-same-package` behavior configurable"
type: pull_request
state: merged
author: charliermarsh
labels:
  - isort
  - configuration
assignees: []
merged: true
base: main
head: charlie/same-module
created_at: 2023-08-24T01:34:07Z
updated_at: 2023-08-24T18:09:28Z
url: https://github.com/astral-sh/ruff/pull/6833
synced_at: 2026-01-12T15:55:22Z
```

# Make isort's `detect-same-package` behavior configurable

---

_@charliermarsh_

## Summary

Our first-party import detection uses a heuristic that doesn't exist in isort: if an import appears to be from within the same package as the containing file, we mark it as first-party. For example, if you have a directory `./foo/__init__.py`, and you import `from foo import bar` in `./foo/baz.py`, we'll mark that as first-party. (See: https://github.com/astral-sh/ruff/pull/1266.)

This is often unnecessary, and arguably should be removed (though it does have some important use-cases that are otherwise unserved -- I believe Dagster uses it to ensure that all packages mark imports from within the same package as first-party, but not imports _across_ different first-party packages)... but it does exist, and it does help in cases in which the `src` field is not properly configured.

This PR adds an option to turn off this behavior:

```toml
[tool.ruff.isort]
detect-same-package = false
```

This is being introduced to help codebases migrating over from isort that may want more consistent behavior with their current sorting.

## Test Plan

`cargo test`


---

_Renamed from "Make isort's detect-same-package behavior configurable" to "Make isort's `detect-same-package` behavior configurable" by @charliermarsh on 2023-08-24 01:34_

---

_Label `isort` added by @charliermarsh on 2023-08-24 01:35_

---

_Label `configuration` added by @charliermarsh on 2023-08-24 01:35_

---

_Comment by @github-actions[bot] on 2023-08-24 01:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      3.6±0.05ms    11.2 MB/sec    1.00      3.5±0.02ms    11.5 MB/sec
formatter/numpy/ctypeslib.py               1.03    726.9±9.95µs    22.9 MB/sec    1.00    706.8±8.50µs    23.6 MB/sec
formatter/numpy/globals.py                 1.03     76.3±0.39µs    38.7 MB/sec    1.00     74.3±0.34µs    39.7 MB/sec
formatter/pydantic/types.py                1.03  1467.4±16.81µs    17.4 MB/sec    1.00   1425.6±9.77µs    17.9 MB/sec
linter/all-rules/large/dataset.py          1.00     10.5±0.03ms     3.9 MB/sec    1.00     10.4±0.04ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.00ms     5.8 MB/sec    1.00      2.9±0.03ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    318.3±0.90µs     9.3 MB/sec    1.00    315.9±1.91µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.02ms     4.7 MB/sec    1.00      5.4±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.01      5.6±0.01ms     7.3 MB/sec    1.00      5.5±0.02ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1190.4±3.42µs    14.0 MB/sec    1.00   1181.8±3.56µs    14.1 MB/sec
linter/default-rules/numpy/globals.py      1.03    125.2±0.24µs    23.6 MB/sec    1.00    122.0±1.35µs    24.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.5±0.02ms    10.1 MB/sec    1.00      2.5±0.01ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.6±0.20ms     8.8 MB/sec    1.12      5.2±0.18ms     7.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   936.4±22.68µs    17.8 MB/sec    1.08  1015.2±36.08µs    16.4 MB/sec
formatter/numpy/globals.py                 1.00     95.1±3.45µs    31.0 MB/sec    1.07    101.8±3.61µs    29.0 MB/sec
formatter/pydantic/types.py                1.00  1892.9±49.00µs    13.5 MB/sec    1.08      2.0±0.05ms    12.5 MB/sec
linter/all-rules/large/dataset.py          1.00     14.9±0.70ms     2.7 MB/sec    1.04     15.6±0.22ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.15ms     4.2 MB/sec    1.11      4.3±0.15ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   487.1±17.83µs     6.1 MB/sec    1.11   540.2±18.09µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.23ms     3.5 MB/sec    1.15      8.3±0.18ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.19ms     4.9 MB/sec    1.06      8.7±0.19ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1699.7±64.56µs     9.8 MB/sec    1.11  1892.3±51.84µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.1±7.67µs    15.0 MB/sec    1.12    219.0±7.20µs    13.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.13ms     7.1 MB/sec    1.11      4.0±0.07ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-08-24 18:09_

---

_Closed by @charliermarsh on 2023-08-24 18:09_

---

_Branch deleted on 2023-08-24 18:09_

---
