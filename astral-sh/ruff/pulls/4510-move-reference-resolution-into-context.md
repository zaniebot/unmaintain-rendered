```yaml
number: 4510
title: Move reference-resolution into Context
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/resolve-reference
created_at: 2023-05-18T20:48:59Z
updated_at: 2023-05-20T03:13:35Z
url: https://github.com/astral-sh/ruff/pull/4510
synced_at: 2026-01-12T15:55:15Z
```

# Move reference-resolution into Context

---

_@charliermarsh_

## Summary

This is a non-behavior-changing PR to move the "Given a symbol, resolve it to a binding" logic out of `Checker` and into `Context`.

---

_Comment by @github-actions[bot] on 2023-05-18 20:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.6±0.32ms     3.0 MB/sec    1.09     14.9±0.50ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.19ms     4.7 MB/sec    1.05      3.8±0.11ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   451.2±19.86µs     6.5 MB/sec    1.01   456.0±20.79µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.25ms     4.1 MB/sec    1.02      6.4±0.22ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.30ms     6.0 MB/sec    1.00      6.7±0.24ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1411.5±52.51µs    11.8 MB/sec    1.01  1431.9±54.17µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    157.3±5.62µs    18.8 MB/sec    1.06    166.3±9.44µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.11ms     8.7 MB/sec    1.02      3.0±0.09ms     8.5 MB/sec
parser/large/dataset.py                    1.00      5.3±0.15ms     7.6 MB/sec    1.02      5.5±0.30ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1017.8±31.12µs    16.4 MB/sec    1.10  1119.8±62.46µs    14.9 MB/sec
parser/numpy/globals.py                    1.00    106.9±5.46µs    27.6 MB/sec    1.06    113.3±6.27µs    26.1 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.07ms    11.5 MB/sec    1.01      2.2±0.09ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.3±0.64ms     2.0 MB/sec    1.02     20.7±0.60ms  2010.8 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.3±0.25ms     3.2 MB/sec    1.00      5.2±0.17ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   608.6±25.80µs     4.8 MB/sec    1.02   621.7±37.38µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.7±0.34ms     2.9 MB/sec    1.00      8.6±0.28ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.26ms     4.1 MB/sec    1.02     10.2±0.31ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.8 MB/sec    1.01      2.2±0.15ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   239.9±10.93µs    12.3 MB/sec    1.02   244.6±20.23µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.18ms     5.6 MB/sec    1.01      4.6±0.17ms     5.5 MB/sec
parser/large/dataset.py                    1.01      8.3±0.27ms     4.9 MB/sec    1.00      8.2±0.19ms     4.9 MB/sec
parser/numpy/ctypeslib.py                  1.01  1600.6±57.61µs    10.4 MB/sec    1.00  1583.0±71.05µs    10.5 MB/sec
parser/numpy/globals.py                    1.00    163.4±8.19µs    18.1 MB/sec    1.00    163.2±7.64µs    18.1 MB/sec
parser/pydantic/types.py                   1.00      3.5±0.29ms     7.3 MB/sec    1.00      3.5±0.13ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-18 21:48_

---

_Review request for @MichaReiser removed by @charliermarsh on 2023-05-18 21:48_

---

_Review requested from @konstin by @charliermarsh on 2023-05-18 21:48_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-18 21:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/context.rs`:147 on 2023-05-19 07:02_

Nit: It feels a bit dangerous that the `resolve` behavior depends on where in the document we are. Meaning, it could return incorrect results if some rule does its own traversal and then calls `resolve`

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/context.rs`:763 on 2023-05-19 07:03_

What's the reason that we need to store the `ScopeId` and `BindingId`. Is there no way for us to do a `BindingId` -> `Binding` lookup?

---

_@MichaReiser approved on 2023-05-19 07:03_

---

_Review comment by @konstin on `crates/ruff/src/checkers/ast/mod.rs`:4840 on 2023-05-19 08:35_

I realize that this code is unchanged in the PR, but why is `__path__` gated to `__init__.py` files?

---

_Review comment by @konstin on `crates/ruff_python_semantic/src/context.rs`:126 on 2023-05-19 08:46_

i'd use `self.scopes.ancestor_ids(self.scope_id).enumerate()` instead, this also avoid `continue` and `first_iter` having bad interactions

---

_@konstin approved on 2023-05-19 08:47_

---

_@charliermarsh reviewed on 2023-05-19 14:45_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4840 on 2023-05-19 14:45_

I think it's only set for modules that are packages?

---

_@charliermarsh reviewed on 2023-05-19 14:46_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/context.rs`:147 on 2023-05-19 14:46_

I don't like this. I also don't like that this method is mutative.

---

_@charliermarsh reviewed on 2023-05-19 15:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/context.rs`:763 on 2023-05-19 15:05_

It's because we have this weird behavior at the level above (which should be moved into this method, I think) whereby we check if there are other names in the scope that need to be marked as "used". This is mainly for submodule imports, e.g., if you have:

```py
import pyarrow as pa
import pyarrow.csv
print(pa.csv.read_csv("test.csv"))
```

Then we end up returning `pa` as the resolved reference, but we actually want to mark `pyarrow.csv` as used too.


---

_@konstin reviewed on 2023-05-19 16:11_

---

_Review comment by @konstin on `crates/ruff/src/checkers/ast/mod.rs`:4840 on 2023-05-19 16:11_

sorry i mixed this up with `__file__`

---

_Merged by @charliermarsh on 2023-05-20 02:47_

---

_Closed by @charliermarsh on 2023-05-20 02:47_

---

_Branch deleted on 2023-05-20 02:47_

---
