```yaml
number: 6324
title: "Flag `comparison-with-itself` on builtin calls"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/builtins
created_at: 2023-08-04T01:39:27Z
updated_at: 2023-08-04T14:11:08Z
url: https://github.com/astral-sh/ruff/pull/6324
synced_at: 2026-01-12T02:52:04Z
```

# Flag `comparison-with-itself` on builtin calls

---

_Pull request opened by @charliermarsh on 2023-08-04 01:39_

## Summary

Extends `comparison-with-itself` to cover simple function calls on known-pure functions, like `id`. For example, we now flag `id(x) == id(x)`.

Closes https://github.com/astral-sh/ruff/issues/6276.

## Test Plan

`cargo test`


---

_Label `rule` added by @charliermarsh on 2023-08-04 01:39_

---

_Comment by @github-actions[bot] on 2023-08-04 01:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.28      9.9±0.57ms     4.1 MB/sec    1.00      7.7±0.36ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.21  1926.7±63.39µs     8.6 MB/sec    1.00  1590.2±94.96µs    10.5 MB/sec
formatter/numpy/globals.py                 1.14   224.3±10.95µs    13.2 MB/sec    1.00   195.9±14.53µs    15.1 MB/sec
formatter/pydantic/types.py                1.21      4.2±0.14ms     6.1 MB/sec    1.00      3.5±0.22ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.12     14.0±0.86ms     2.9 MB/sec    1.00     12.6±0.77ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.13      3.4±0.11ms     4.9 MB/sec    1.00      3.0±0.20ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.20   488.0±16.36µs     6.0 MB/sec    1.00   406.2±17.15µs     7.3 MB/sec
linter/all-rules/pydantic/types.py         1.15      6.4±0.26ms     4.0 MB/sec    1.00      5.6±0.33ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.18      7.1±0.34ms     5.7 MB/sec    1.00      6.0±0.40ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.12  1404.4±38.55µs    11.9 MB/sec    1.00  1258.9±46.61µs    13.2 MB/sec
linter/default-rules/numpy/globals.py      1.10    171.7±5.65µs    17.2 MB/sec    1.00    156.6±7.36µs    18.8 MB/sec
linter/default-rules/pydantic/types.py     1.20      3.1±0.17ms     8.2 MB/sec    1.00      2.6±0.10ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.11ms     4.2 MB/sec    1.00      9.7±0.10ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1881.4±24.89µs     8.9 MB/sec    1.00  1885.1±24.57µs     8.8 MB/sec
formatter/numpy/globals.py                 1.02    212.8±5.27µs    13.9 MB/sec    1.00    209.0±6.96µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.05ms     6.3 MB/sec    1.02      4.2±0.06ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.7±0.18ms     3.0 MB/sec    1.00     13.7±0.17ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.7 MB/sec    1.00      3.5±0.06ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.4±5.88µs     6.9 MB/sec    1.01    433.2±8.77µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.09ms     4.1 MB/sec    1.00      6.2±0.14ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.08ms     5.9 MB/sec    1.00      6.8±0.08ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1396.9±14.96µs    11.9 MB/sec    1.01  1411.2±19.99µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.2±2.65µs    18.6 MB/sec    1.02    161.5±2.59µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.04ms     8.3 MB/sec    1.00      3.0±0.03ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-04 02:13_

I guess `hash` shouldn't be included here. Perhaps others too? I need to audit the list.

---

_Review requested from @zanieb by @charliermarsh on 2023-08-04 02:13_

---

_Comment by @charliermarsh on 2023-08-04 02:18_

On second thought, isn't Python dynamic enough that you could add randomness to almost any of these builtins (e.g., `len`)? I guess `id` is valid, but are there even others?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/comparison_with_itself.rs`:59 on 2023-08-04 05:46_

Nit
```suggestion
            (Expr::Name(left_name), Expr::Name(right_name)) if left_name.id == right_name.id => {
```

---

_@MichaReiser reviewed on 2023-08-04 05:47_

---

_Merged by @charliermarsh on 2023-08-04 13:51_

---

_Closed by @charliermarsh on 2023-08-04 13:51_

---

_Branch deleted on 2023-08-04 13:51_

---
