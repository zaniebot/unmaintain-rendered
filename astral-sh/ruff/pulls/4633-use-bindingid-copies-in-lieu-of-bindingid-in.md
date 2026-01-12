```yaml
number: 4633
title: "Use `BindingId` copies in lieu of `&BindingId` in semantic model methods"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/copy
created_at: 2023-05-24T14:54:44Z
updated_at: 2023-05-24T16:23:00Z
url: https://github.com/astral-sh/ruff/pull/4633
synced_at: 2026-01-12T15:55:16Z
```

# Use `BindingId` copies in lieu of `&BindingId` in semantic model methods

---

_@charliermarsh_

## Summary

Partly putting this up because I don't have good intuition around these tradeoffs. Given that `BindingId` is small and implements `Copy`, is it ever advantageous to pass around reference?

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-24 14:54_

---

_@charliermarsh reviewed on 2023-05-24 14:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:79 on 2023-05-24 14:55_

This is a bit tedious. (Should this just be `self.bindings.iter().copied()`? That would change to `&str`.)

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/scope.rs`:79 on 2023-05-24 14:58_

I think so. `&&str` gets automatically dereferenced by `Rust` to `&str` but using `copied()` is just simpler :)

---

_@MichaReiser approved on 2023-05-24 14:59_

The way I think about it (and I might be wrong) is that passing a value by reference means we pass and copy a pointer. A pointer is 8 bytes. Copying any data that is less or equal to 8 bytes should, thus, be as much work. 

---

_Merged by @charliermarsh on 2023-05-24 15:55_

---

_Closed by @charliermarsh on 2023-05-24 15:55_

---

_Branch deleted on 2023-05-24 15:55_

---

_Comment by @github-actions[bot] on 2023-05-24 16:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.18ms     2.9 MB/sec    1.00     14.2±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.4±0.66µs     7.0 MB/sec    1.00    425.7±1.18µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.04ms     4.3 MB/sec    1.00      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.03ms     5.9 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1513.9±24.05µs    11.0 MB/sec    1.00   1509.5±5.31µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.5±0.27µs    17.2 MB/sec    1.00    171.6±0.18µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.00ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1023.3±0.58µs    16.3 MB/sec    1.00   1023.0±1.10µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    104.8±0.14µs    28.2 MB/sec    1.00    105.2±0.24µs    28.1 MB/sec
parser/pydantic/types.py                   1.01      2.2±0.00ms    11.3 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.6±0.18ms     2.2 MB/sec    1.00     18.6±0.13ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.07ms     3.4 MB/sec    1.00      4.8±0.09ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   480.9±11.60µs     6.1 MB/sec    1.00    480.3±8.77µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.07ms     3.2 MB/sec    1.00      7.9±0.06ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.02      9.2±0.07ms     4.4 MB/sec    1.00      9.1±0.07ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1917.2±18.23µs     8.7 MB/sec    1.00  1904.3±24.38µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.4±2.39µs    14.4 MB/sec    1.01    206.6±5.30µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.04ms     6.2 MB/sec    1.00      4.1±0.06ms     6.2 MB/sec
parser/large/dataset.py                    1.17      8.1±0.03ms     5.0 MB/sec    1.00      7.0±0.04ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.14  1490.9±13.10µs    11.2 MB/sec    1.00  1302.4±13.78µs    12.8 MB/sec
parser/numpy/globals.py                    1.10    146.3±1.83µs    20.2 MB/sec    1.00    133.1±1.60µs    22.2 MB/sec
parser/pydantic/types.py                   1.15      3.4±0.02ms     7.6 MB/sec    1.00      2.9±0.02ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
