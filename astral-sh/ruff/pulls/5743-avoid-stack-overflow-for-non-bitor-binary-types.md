```yaml
number: 5743
title: Avoid stack overflow for non-BitOr binary types
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/stack
created_at: 2023-07-13T17:21:33Z
updated_at: 2023-07-13T18:23:41Z
url: https://github.com/astral-sh/ruff/pull/5743
synced_at: 2026-01-12T15:55:19Z
```

# Avoid stack overflow for non-BitOr binary types

---

_@charliermarsh_

## Summary

Closes #5742.


---

_@charliermarsh reviewed on 2023-07-13 17:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/typing.rs`:42 on 2023-07-13 17:22_

I think we can remove this entirely and instead do this recursion via `contains_none` and `contains_any`.

---

_@charliermarsh reviewed on 2023-07-13 17:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/typing.rs`:138 on 2023-07-13 17:22_

This was the key issue: every BinOp was getting turned into `PEP604Union`, and when we went to analyze that expression in `PEP604UnionIterator`, we'd see that it wasn't a `BitOr`, and return that expression immediately, then analyze it again here, etc.

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-07-13 17:22_

---

_Comment by @github-actions[bot] on 2023-07-13 17:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.1±0.04ms     5.0 MB/sec    1.00      7.9±0.03ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.01   1863.8±3.97µs     8.9 MB/sec    1.00   1843.0±3.39µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    207.6±0.29µs    14.2 MB/sec    1.00    207.1±0.48µs    14.2 MB/sec
formatter/pydantic/types.py                1.03      4.0±0.01ms     6.3 MB/sec    1.00      3.9±0.01ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.01ms     3.0 MB/sec    1.00     13.4±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    445.6±0.51µs     6.6 MB/sec    1.00    445.0±0.65µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.05      6.3±0.08ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.1 MB/sec    1.01      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1445.1±2.74µs    11.5 MB/sec    1.01   1453.4±2.81µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.9±0.55µs    17.8 MB/sec    1.00    166.4±8.56µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.11     12.6±0.46ms     3.2 MB/sec    1.00     11.4±0.39ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.7±0.12ms     6.1 MB/sec    1.00      2.6±0.21ms     6.4 MB/sec
formatter/numpy/globals.py                 1.03   320.1±23.48µs     9.2 MB/sec    1.00   310.0±16.15µs     9.5 MB/sec
formatter/pydantic/types.py                1.06      6.0±0.23ms     4.2 MB/sec    1.00      5.7±0.26ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     18.7±0.68ms     2.2 MB/sec    1.05     19.7±0.60ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.19ms     3.4 MB/sec    1.04      5.2±0.18ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   596.7±22.71µs     4.9 MB/sec    1.09   649.9±33.02µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.34ms     2.9 MB/sec    1.01      8.8±0.33ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.27ms     4.2 MB/sec    1.03     10.0±0.29ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.06ms     7.9 MB/sec    1.00      2.1±0.08ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    247.8±9.91µs    11.9 MB/sec    1.02   252.4±11.86µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.31ms     5.7 MB/sec    1.00      4.5±0.16ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@dhruvmanila reviewed on 2023-07-13 17:45_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/typing.rs`:138 on 2023-07-13 17:45_

Oh, nice catch! I wonder what's the use case of any other operator in this context (will look at the linked project to satisfy my curiosity).

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/typing.rs`:109 on 2023-07-13 17:50_

I wonder if we should just short-circuit if the `op` isn't `BitOr` and return `Unknown`? 

Just thinking out loud that otherwise this will try to `resolve_call_path` and  return `Unknown` type. Either ways, `contains_none` will be `true` and `contains_any` will be `false`.

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/ruff/typing.rs`:42 on 2023-07-13 17:53_

Yeah.

---

_@dhruvmanila approved on 2023-07-13 17:54_

---

_@charliermarsh reviewed on 2023-07-13 18:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/typing.rs`:109 on 2023-07-13 18:23_

I think it's fine because `resolve_call_path` will exit "immediately" when it detects the expression type.

---

_Merged by @charliermarsh on 2023-07-13 18:23_

---

_Closed by @charliermarsh on 2023-07-13 18:23_

---

_Branch deleted on 2023-07-13 18:23_

---
