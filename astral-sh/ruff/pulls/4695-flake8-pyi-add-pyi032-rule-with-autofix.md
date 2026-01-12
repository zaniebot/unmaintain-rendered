```yaml
number: 4695
title: "[`flake8-pyi`] Add `PYI032` rule with autofix"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/PYI032
created_at: 2023-05-28T17:14:18Z
updated_at: 2023-07-06T17:35:50Z
url: https://github.com/astral-sh/ruff/pull/4695
synced_at: 2026-01-12T15:55:16Z
```

# [`flake8-pyi`] Add `PYI032` rule with autofix

---

_@qdegraaf_

## Summary

Adds Y032 rule from `flake8-pyi` (https://github.com/PyCQA/flake8-pyi) to the Ruff plugin. Original rule definition: 

> The second argument of an __eq__ or __ne__ method should usually be annotated with object rather than Any.

## Test Plan

Fixtures for `.py` and `.pyi` added and added both cases to existing `flake8-pyi` unit tests 

## Issue link

Refers: https://github.com/charliermarsh/ruff/issues/848 


---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_pyi/rules/any_eq_ne_annotation.rs`:76 on 2023-05-28 17:15_

I tried combining this logic with the the checker logic from lines 64-66 in a more efficient and elegant manner. Did not succeed in finding a better way, though I believe there are many ways to Rome here, so welcome any advice on making this patch more compact.

---

_@qdegraaf reviewed on 2023-05-28 17:15_

---

_Comment by @github-actions[bot] on 2023-05-28 17:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.3±0.84ms     2.1 MB/sec    1.01     19.5±0.52ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.14ms     3.7 MB/sec    1.01      4.6±0.22ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   555.5±19.52µs     5.3 MB/sec    1.02   566.4±29.31µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.29ms     3.2 MB/sec    1.00      8.0±0.28ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.05      9.0±0.27ms     4.5 MB/sec    1.00      8.6±0.32ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09      2.0±0.08ms     8.3 MB/sec    1.00  1854.2±53.90µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.04   238.5±26.45µs    12.4 MB/sec    1.00   229.6±12.19µs    12.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.15ms     6.3 MB/sec    1.02      4.1±0.17ms     6.2 MB/sec
parser/large/dataset.py                    1.00      6.8±0.15ms     6.0 MB/sec    1.01      6.9±0.14ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1355.3±52.23µs    12.3 MB/sec    1.02  1381.3±43.99µs    12.1 MB/sec
parser/numpy/globals.py                    1.00    138.4±6.23µs    21.3 MB/sec    1.00    137.9±5.28µs    21.4 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.07ms     8.6 MB/sec    1.01      3.0±0.09ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.4±0.97ms  2039.3 KB/sec    1.02     20.8±1.06ms  2007.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.27ms     3.3 MB/sec    1.04      5.3±0.43ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   603.7±35.11µs     4.9 MB/sec    1.04   628.5±34.21µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.44ms     3.0 MB/sec    1.02      8.6±0.39ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.46ms     4.0 MB/sec    1.02     10.3±0.43ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.16ms     7.7 MB/sec    1.02      2.2±0.17ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   243.8±13.78µs    12.1 MB/sec    1.09   264.5±19.74µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.34ms     5.7 MB/sec    1.02      4.6±0.23ms     5.5 MB/sec
parser/large/dataset.py                    1.00      7.7±0.29ms     5.3 MB/sec    1.24      9.5±0.44ms     4.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1494.7±96.66µs    11.1 MB/sec    1.18  1768.5±84.06µs     9.4 MB/sec
parser/numpy/globals.py                    1.00   159.6±20.05µs    18.5 MB/sec    1.08   172.4±12.80µs    17.1 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.16ms     7.6 MB/sec    1.18      4.0±0.35ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-05-28 22:32_

---

_Renamed from "Add PYI032 rule with autofix" to "[`flake8-pyi`] Add `PYI032` rule with autofix" by @charliermarsh on 2023-05-28 22:32_

---

_@charliermarsh reviewed on 2023-05-28 22:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/any_eq_ne_annotation.rs`:76 on 2023-05-28 22:33_

Hmm -- is there any issue with just using the range of the annotation? Why do we need to match on the "kind"?

---

_Comment by @charliermarsh on 2023-05-28 22:33_

Thanks!

---

_Merged by @charliermarsh on 2023-05-28 22:41_

---

_Closed by @charliermarsh on 2023-05-28 22:41_

---

_@qdegraaf reviewed on 2023-05-29 10:13_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_pyi/rules/any_eq_ne_annotation.rs`:76 on 2023-05-29 10:13_

Ah I see what I missed now. Tried that first, but could not get it to work. But I forgot to import: `use ruff_python_ast::prelude::Ranged;`

Thanks for edits!

---

_Branch deleted on 2023-07-06 17:35_

---
