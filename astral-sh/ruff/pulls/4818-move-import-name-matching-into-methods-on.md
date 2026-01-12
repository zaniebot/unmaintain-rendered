```yaml
number: 4818
title: "Move import-name matching into methods on `BindingKind`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/qualified-name-2
created_at: 2023-06-02T20:16:53Z
updated_at: 2023-06-03T19:01:29Z
url: https://github.com/astral-sh/ruff/pull/4818
synced_at: 2026-01-12T03:50:03Z
```

# Move import-name matching into methods on `BindingKind`

---

_Pull request opened by @charliermarsh on 2023-06-02 20:16_

## Summary

We have this pattern in a few places whereby we match against the import `BindingKind` variants and extract the "full_name" field. This PR just formalizes it behind some API. In doing so, I also simplified the implicitly-imported-module check a bit.


---

_@charliermarsh reviewed on 2023-06-02 20:17_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_type_checking/strict.py`:52 on 2023-06-02 20:17_

(These comments were just slightly misleading. The actual tests and results haven't changed.)

---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-02 20:23_

---

_Comment by @github-actions[bot] on 2023-06-02 20:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.02ms     2.9 MB/sec    1.00     14.0±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    429.3±1.31µs     6.9 MB/sec    1.00    426.0±0.63µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.4 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1514.4±4.20µs    11.0 MB/sec    1.00   1496.6±2.59µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    172.9±0.57µs    17.1 MB/sec    1.00    169.5±0.24µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.8 MB/sec    1.00      5.2±0.02ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1013.6±0.46µs    16.4 MB/sec    1.00   1013.4±0.88µs    16.4 MB/sec
parser/numpy/globals.py                    1.00    104.8±0.29µs    28.2 MB/sec    1.01    105.4±0.41µs    28.0 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.01ms    11.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     17.4±0.80ms     2.3 MB/sec    1.00     16.4±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.07ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    504.3±7.47µs     5.9 MB/sec    1.00    496.3±6.51µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.14ms     3.6 MB/sec    1.00      7.0±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.08ms     4.9 MB/sec    1.00      8.1±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1759.4±29.16µs     9.5 MB/sec    1.00  1733.6±15.98µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    201.5±5.03µs    14.6 MB/sec    1.00    200.2±4.01µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     7.0 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.4±0.06ms     6.3 MB/sec    1.01      6.5±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1202.6±13.45µs    13.8 MB/sec    1.01  1216.0±20.27µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    123.4±2.10µs    23.9 MB/sec    1.01    124.7±1.80µs    23.7 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.4 MB/sec    1.01      2.7±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-02 20:59_

(Need to look into the ecosystem CI change, not intended.)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:185 on 2023-06-03 14:08_

Lol. This is all just gone? Nice!

---

_@MichaReiser approved on 2023-06-03 14:09_

---

_Merged by @charliermarsh on 2023-06-03 19:01_

---

_Closed by @charliermarsh on 2023-06-03 19:01_

---

_Branch deleted on 2023-06-03 19:01_

---
