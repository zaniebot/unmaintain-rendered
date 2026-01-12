```yaml
number: 5181
title: Run shadowed-variable analyses in deferred handlers
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/shadowed
created_at: 2023-06-19T14:27:51Z
updated_at: 2023-06-29T00:29:41Z
url: https://github.com/astral-sh/ruff/pull/5181
synced_at: 2026-01-12T15:55:17Z
```

# Run shadowed-variable analyses in deferred handlers

---

_@charliermarsh_

## Summary

This PR extracts a bunch of complex logic from `add_binding`, instead running the the shadowing rules in the deferred handler, thereby decoupling the binding phase (during which we build up the semantic model) from the analysis phase, and generally making `add_binding` much more focused.

This was made possible by improving the semantic model to better handle deletions -- previously, we'd "lose track" of bindings if they were deleted, which made this kind of refactor impossible.

## Test Plan

We have good automated coverage for this, but I want to benchmark it separately.


---

_@charliermarsh reviewed on 2023-06-19 14:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__del_shadowed_global_import_in_local_scope.snap`:26 on 2023-06-19 14:28_

This was a false negative.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__del_shadowed_import_shadow_in_local_scope.snap`:26 on 2023-06-19 14:28_

I think it's fine not to flag this.

---

_@charliermarsh reviewed on 2023-06-19 14:28_

---

_Comment by @github-actions[bot] on 2023-06-19 14:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.02ms     5.3 MB/sec    1.02      7.9±0.01ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.00   1689.5±7.15µs     9.9 MB/sec    1.02   1716.6±2.78µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    188.3±0.51µs    15.7 MB/sec    1.01    190.6±0.33µs    15.5 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.01ms     7.0 MB/sec    1.01      3.7±0.01ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.01     13.0±0.05ms     3.1 MB/sec    1.00     13.0±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.00ms     5.1 MB/sec    1.04      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.0±1.46µs     7.0 MB/sec    1.03    433.3±1.04µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.01ms     4.5 MB/sec    1.00      5.7±0.01ms     4.5 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1432.8±3.88µs    11.6 MB/sec    1.00   1434.3±1.10µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.0±0.15µs    18.3 MB/sec    1.01    162.8±3.67µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.5±0.59ms     3.2 MB/sec    1.00     12.5±0.59ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.6±0.14ms     6.5 MB/sec    1.00      2.4±0.12ms     6.9 MB/sec
formatter/numpy/globals.py                 1.03   292.0±32.72µs    10.1 MB/sec    1.00   282.7±16.83µs    10.4 MB/sec
formatter/pydantic/types.py                1.04      5.5±0.26ms     4.6 MB/sec    1.00      5.3±0.24ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     19.9±0.88ms     2.0 MB/sec    1.04     20.8±0.90ms  2002.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.25ms     3.2 MB/sec    1.02      5.3±0.39ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   634.1±38.43µs     4.7 MB/sec    1.03   652.1±36.79µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.05      9.1±0.56ms     2.8 MB/sec    1.00      8.7±0.47ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.51ms     4.0 MB/sec    1.01     10.2±0.42ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.10ms     7.7 MB/sec    1.04      2.3±0.08ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   263.9±15.49µs    11.2 MB/sec    1.04   274.4±14.52µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.20ms     5.4 MB/sec    1.01      4.8±0.26ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-19 15:32_

---

_Review requested from @konstin by @charliermarsh on 2023-06-19 15:32_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__del_shadowed_import_shadow_in_local_scope.snap`:26 on 2023-06-20 07:17_

Why did this change? Is it because `f` assigned `os` on line 5?

Should we update the comment in the test input file?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:1078 on 2023-06-20 07:19_

Unrelated to this PR. Why do we need a global shadowed_bindings and a scoipe specfici shadowed binding tracking?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:1075 on 2023-06-20 07:20_

Are both paths (checking the scope specific and global shadowed bindings) check necessary after we've found the first shadowed binding?

---

_@MichaReiser approved on 2023-06-20 07:21_

---

_@charliermarsh reviewed on 2023-06-20 16:12_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:1078 on 2023-06-20 16:12_

Unfortunately, Flake8 has different rules around _when_ it flags for shadowed-while-unused depending on whether the shadowing is within-scope or across scopes.

Namely, when flagging shadowed-while-unused across scopes, it only flags when the shadowed variable is an import like:

```python
import os

def f():
  os = 1
```

But not:

```python
os = 1

def f():
  os = 1
```

However, when flagging shadowed-while-unused within scopes, it does flag:

```python
def f():
  os = 1
  os = 1
```

---

_Review comment by @konstin on `crates/ruff_python_semantic/src/model.rs`:1067 on 2023-06-21 08:19_

this setting first to false is kinda confusing to read

---

_@konstin approved on 2023-06-21 08:20_

---

_@charliermarsh reviewed on 2023-06-29 00:01_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__del_shadowed_import_shadow_in_local_scope.snap`:26 on 2023-06-29 00:01_

I did!

---

_@charliermarsh reviewed on 2023-06-29 00:01_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__del_shadowed_import_shadow_in_local_scope.snap`:26 on 2023-06-29 00:01_

It's because we `del os` in the containing scope (not visible in this diff).

---

_Merged by @charliermarsh on 2023-06-29 00:08_

---

_Closed by @charliermarsh on 2023-06-29 00:08_

---

_Branch deleted on 2023-06-29 00:08_

---
