```yaml
number: 4145
title: "Respect parent-scoping rules for `NamedExpr` assignments"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/named-expr-in-comprehension
created_at: 2023-04-28T23:09:57Z
updated_at: 2023-04-29T23:08:06Z
url: https://github.com/astral-sh/ruff/pull/4145
synced_at: 2026-01-12T04:28:19Z
```

# Respect parent-scoping rules for `NamedExpr` assignments

---

_Pull request opened by @charliermarsh on 2023-04-28 23:09_

## Summary

Per [PEP 572](https://peps.python.org/pep-0572/#scope-of-the-target), a named expression assignment within a generator or comprehension actually binds to the parent scope. This is useful as it enables patterns like:

```py
# Compute partial sums in a list comprehension
total = 0
partial_sums = [total := total + v for v in values]
print("Total:", total)
```

Previously, we bound the named expression within the generator or comprehension, which led to us flagging `key` as undefined here:

```py
if any((key := x) for x in ["ok"]):
    print(key)
```

Closes #3997.


---

_Comment by @github-actions[bot] on 2023-04-28 23:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.03     14.3±0.97ms     2.8 MB/sec     1.00     14.0±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec     1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    419.4±1.06µs     7.0 MB/sec     1.00    418.6±1.14µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec     1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.01ms     5.9 MB/sec     1.00      6.9±0.03ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1478.9±2.42µs    11.3 MB/sec     1.00   1480.5±2.86µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    166.8±0.35µs    17.7 MB/sec     1.00    166.0±0.21µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec     1.00      3.1±0.02ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.5±0.08ms     7.4 MB/sec     1.00      5.5±0.01ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.15  1220.1±288.13µs    13.6 MB/sec    1.00   1065.2±2.45µs    15.6 MB/sec
parser/numpy/globals.py                    1.14   123.7±49.00µs    23.9 MB/sec     1.00    108.6±0.15µs    27.2 MB/sec
parser/pydantic/types.py                   1.15      2.7±0.72ms     9.6 MB/sec     1.00      2.3±0.04ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.6±0.25ms     2.4 MB/sec    1.00     16.5±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.00      4.1±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   493.1±13.65µs     6.0 MB/sec    1.00   490.9±14.77µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.7 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.09ms     4.9 MB/sec    1.00      8.3±0.08ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1777.5±18.24µs     9.4 MB/sec    1.00  1775.8±21.25µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.9±5.61µs    14.8 MB/sec    1.00    198.3±4.32µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.04ms     6.8 MB/sec    1.00      3.7±0.04ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.8±0.07ms     6.0 MB/sec    1.00      6.8±0.06ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1293.6±13.72µs    12.9 MB/sec    1.01  1301.9±18.06µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    132.2±2.45µs    22.3 MB/sec    1.01    133.7±2.87µs    22.1 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.04ms     8.8 MB/sec    1.00      2.9±0.03ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4277 on 2023-04-29 15:59_

Nit: The documentation mentions that the comprehension binds to the parent scope but the `find` binds to the first non generator scope. Can we either update the documentation or the code.

Could this crash if we support linting individual expressions?

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4272 on 2023-04-29 16:00_

Nit: Would it make sense to return the `scope` instead
```suggestion
        let scope = if binding.kind.is_named_expr_assignment() {
```

It would avoid retrieving the id from the ancestor scope only to look it up again.

---

_@MichaReiser approved on 2023-04-29 16:01_

---

_@charliermarsh reviewed on 2023-04-29 18:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4272 on 2023-04-29 18:29_

Yeah, it has to be `mut` though. I'll try to get it working.

---

_Merged by @charliermarsh on 2023-04-29 22:45_

---

_Closed by @charliermarsh on 2023-04-29 22:45_

---

_Branch deleted on 2023-04-29 22:45_

---
