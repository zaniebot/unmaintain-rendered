```yaml
number: 5021
title: "[flake8-pyi] Implement PYI044"
type: pull_request
state: merged
author: Thomasdezeeuw
labels: []
assignees: []
merged: true
base: main
head: PYI044
created_at: 2023-06-12T09:20:03Z
updated_at: 2023-06-12T11:20:18Z
url: https://github.com/astral-sh/ruff/pull/5021
synced_at: 2026-01-12T03:43:29Z
```

# [flake8-pyi] Implement PYI044

---

_Pull request opened by @Thomasdezeeuw on 2023-06-12 09:20_

## Summary

This implements PYI044. This rule checks if `from __future__` is used in stub files as it has no effect in stub files, since type checkers automatically treat stubs as having those semantics.

Updates https://github.com/astral-sh/ruff/issues/848

## Test Plan

Added a test case and snapshots.

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/import_from_future.rs`:14 on 2023-06-12 09:27_

```suggestion
        format!("`from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics")
```

---

_@konstin reviewed on 2023-06-12 09:30_

This lint is specifically about `from __future__ import annotations`, so this should not line other future imports  (https://github.com/PyCQA/flake8-pyi/blob/8fe8f0a49f66ac91e60f8939c01fefd2fd02812c/pyi.py#L989-L991) 

---

_Comment by @github-actions[bot] on 2023-06-12 09:31_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+6, -0, 0 error(s))

<details><summary>airflow (+4, -0)</summary>
<p>

```diff
+ airflow/compat/functools.pyi:21:1: PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
+ airflow/decorators/__init__.pyi:21:1: PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
+ airflow/migrations/db_types.pyi:19:1: PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
+ airflow/utils/context.pyi:27:1: PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
```

</p>
</details>
<details><summary>bokeh (+2, -0)</summary>
<p>

```diff
+ src/typings/selenium/webdriver/common/action_chains.pyi:1:1: PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
+ src/typings/selenium/webdriver/remote/webelement.pyi:1:1: PYI044 `from __future__ import annotations` has no effect in stub files, since type checkers automatically treat stubs as having those semantics
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI044 | 6 | 6 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.2±0.01ms     6.5 MB/sec    1.00      6.3±0.01ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1299.9±2.25µs    12.8 MB/sec    1.01   1307.8±3.59µs    12.7 MB/sec
formatter/numpy/globals.py                 1.00    125.2±2.07µs    23.6 MB/sec    1.00    125.6±1.44µs    23.5 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.03ms     9.9 MB/sec    1.01      2.6±0.03ms     9.9 MB/sec
linter/all-rules/large/dataset.py          1.01     13.7±0.08ms     3.0 MB/sec    1.00     13.6±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    417.4±0.71µs     7.1 MB/sec    1.00    413.8±1.49µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.01ms     4.4 MB/sec    1.00      5.7±0.01ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1464.3±1.70µs    11.4 MB/sec    1.00  1445.0±10.49µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    163.3±0.22µs    18.1 MB/sec    1.00    162.0±0.96µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.5±0.28ms     4.3 MB/sec     1.01      9.6±0.37ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1917.7±119.28µs     8.7 MB/sec    1.02  1961.7±100.68µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    181.2±9.15µs    16.3 MB/sec     1.03   187.1±11.69µs    15.8 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.21ms     6.6 MB/sec     1.01      3.9±0.18ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     20.2±0.45ms     2.0 MB/sec     1.03     20.9±0.70ms  1996.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.20ms     3.3 MB/sec     1.04      5.3±0.33ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   594.8±20.61µs     5.0 MB/sec     1.04   619.1±28.91µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.27ms     3.0 MB/sec     1.04      8.9±0.37ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.41ms     4.0 MB/sec     1.02     10.5±0.30ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.08ms     7.7 MB/sec     1.03      2.2±0.10ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    239.9±8.25µs    12.3 MB/sec     1.04   249.4±16.76µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.13ms     5.6 MB/sec     1.05      4.8±0.25ms     5.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @Thomasdezeeuw on 2023-06-12 09:37_

> This lint is specifically about `from __future__ import annotations`, so this should not line other future imports (https://github.com/PyCQA/flake8-pyi/blob/8fe8f0a49f66ac91e60f8939c01fefd2fd02812c/pyi.py#L989-L991)

Ah, then I misunderstood the lint. I though `from __future__ import $anything` should trigger it. But it's specifically `import annotations`, will fix.

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/import_from_future.rs`:9 on 2023-06-12 09:50_

I think i'd call this rule `FutureAnnotationsInStub` which would fit the [other namein PYI](https://beta.ruff.rs/docs/rules/#flake8-pyi-pyi) better

---

_@konstin approved on 2023-06-12 09:50_

---

_@konstin approved on 2023-06-12 11:12_

---

_Merged by @Thomasdezeeuw on 2023-06-12 11:20_

---

_Closed by @Thomasdezeeuw on 2023-06-12 11:20_

---

_Branch deleted on 2023-06-12 11:20_

---
