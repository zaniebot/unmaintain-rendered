```yaml
number: 4258
title: Respect insertion location when importing symbols
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/importer
created_at: 2023-05-06T20:15:35Z
updated_at: 2023-05-07T02:58:32Z
url: https://github.com/astral-sh/ruff/pull/4258
synced_at: 2026-01-12T03:56:39Z
```

# Respect insertion location when importing symbols

---

_Pull request opened by @charliermarsh on 2023-05-06 20:15_

## Summary

Previously, `Importer` would only track the most recently-seen top-level import statement, and would add any new imports _after_ that statement. However, because we visit function bodies and other scopes in a deferred manner, then if there are top-level imports _below_ a given function, we'll visit those imports, then visit the function body, then insert the new import _after_ the function.

This causes two problems: (1) it breaks the invariant, enforced in the fixer, that edits are _ordered_, since the import insertion can come _after_ the edit that makes use of the symbol; and (2) in theory, this could lead to broken code, since a function could be called _before_ we reach the import statement (since that statement comes later in the module).

The fix here is to track all import statements, and return the last import statement _before_ a given edit location.

Closes #4208.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-06 20:15_

---

_Review requested from @konstin by @charliermarsh on 2023-05-06 20:15_

---

_@charliermarsh reviewed on 2023-05-06 20:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer.rs`:22 on 2023-05-06 20:16_

I opted to get rid of this and just do a full search. The number of imports is typically not huge, this operation is relatively rare, and we'd now have to track `FxHashMap<&'a str, Vec<&'a Stmt>>` anyway.


---

_@charliermarsh reviewed on 2023-05-06 20:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer.rs`:79 on 2023-05-06 20:16_

Hard to reason about whether this preliminary search is worth it.

---

_Comment by @github-actions[bot] on 2023-05-06 20:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.6±0.02ms     2.8 MB/sec    1.00     14.6±0.03ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.7±1.01µs     8.2 MB/sec    1.01    364.3±1.21µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.00      7.4±0.01ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1542.2±3.48µs    10.8 MB/sec    1.00   1543.3±3.26µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.0±0.20µs    17.9 MB/sec    1.01    166.7±0.38µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.9±0.00ms     6.9 MB/sec    1.00      5.9±0.00ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1153.4±8.53µs    14.4 MB/sec    1.00   1151.3±2.58µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    117.5±0.23µs    25.1 MB/sec    1.00    117.7±1.09µs    25.1 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.01ms    10.2 MB/sec    1.00      2.5±0.00ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.25ms     2.7 MB/sec    1.01     15.0±0.40ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.8±0.14ms     4.4 MB/sec    1.00      3.7±0.08ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.01   445.5±17.96µs     6.6 MB/sec    1.00   443.2±13.55µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.18ms     4.1 MB/sec    1.01      6.3±0.61ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.02      7.6±0.17ms     5.4 MB/sec    1.00      7.4±0.14ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1595.8±34.98µs    10.4 MB/sec    1.00  1597.6±48.59µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.9±5.65µs    16.5 MB/sec    1.01    181.5±5.82µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.08ms     7.6 MB/sec    1.00      3.3±0.07ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.9±0.14ms     6.9 MB/sec    1.02      6.0±0.13ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1107.6±26.73µs    15.0 MB/sec    1.04  1146.6±34.91µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    113.5±3.13µs    26.0 MB/sec    1.03    116.7±3.81µs    25.3 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.07ms    10.3 MB/sec    1.04      2.6±0.11ms     9.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-06 20:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer.rs`:79 on 2023-05-06 20:29_

Probably not?

---

_@konstin reviewed on 2023-05-06 21:10_

---

_Review comment by @konstin on `crates/ruff/src/importer.rs`:79 on 2023-05-06 21:10_

If we assume that in most file all imports are at the start of the file, just `if stmt.start() > at { return false; }` in the find should be faster.

---

_Review comment by @konstin on `crates/ruff/src/rules/isort/rules/add_required_imports.rs`:125 on 2023-05-06 21:14_

Shouldn't this use the location of the first non-import statement, or do they need to be inserted even before the other imports?

---

_@konstin approved on 2023-05-06 21:15_

---

_@charliermarsh reviewed on 2023-05-07 02:27_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/rules/add_required_imports.rs`:125 on 2023-05-07 02:27_

They should be inserted at the top of the file -- since it has to work with `__future__` imports.

---

_Merged by @charliermarsh on 2023-05-07 02:32_

---

_Closed by @charliermarsh on 2023-05-07 02:32_

---

_Branch deleted on 2023-05-07 02:32_

---
