```yaml
number: 6378
title: "[`flake8-tidy-imports`] Add `TID253`"
type: pull_request
state: merged
author: durumu
labels:
  - rule
assignees: []
merged: true
base: main
head: durumu/bannedmodulelevel
created_at: 2023-08-06T21:42:08Z
updated_at: 2023-08-12T19:09:44Z
url: https://github.com/astral-sh/ruff/pull/6378
synced_at: 2026-01-12T02:52:04Z
```

# [`flake8-tidy-imports`] Add `TID253`

---

_Pull request opened by @durumu on 2023-08-06 21:42_

## Summary

Add a new rule `TID253` (`banned-module-level-imports`), to ban a user-specified list of imports from appearing at module level. This rule doesn't exist in `flake8-tidy-imports`, so it's unique to Ruff. The implementation is pretty similar to `TID251`.

Briefly discussed [here](https://github.com/astral-sh/ruff/discussions/6370).

## Test Plan

Added a new test case, checking that inline imports are allowed and that non-inline imports from the banned list are disallowed.


---

_Comment by @github-actions[bot] on 2023-08-06 21:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.05ms     4.9 MB/sec    1.02      8.4±0.22ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1636.2±13.97µs    10.2 MB/sec    1.01  1647.7±29.88µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    186.4±3.82µs    15.8 MB/sec    1.01    187.5±5.40µs    15.7 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.08ms     7.4 MB/sec    1.01      3.5±0.09ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.01     10.3±0.06ms     3.9 MB/sec    1.00     10.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     6.0 MB/sec    1.00      2.8±0.02ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    390.6±0.58µs     7.6 MB/sec    1.00    392.1±0.64µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.08ms     4.8 MB/sec    1.00      5.3±0.03ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.01ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1195.9±5.08µs    13.9 MB/sec    1.00   1201.8±6.51µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.1±1.48µs    21.1 MB/sec    1.00    140.1±0.32µs    21.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.4±0.01ms    10.4 MB/sec    1.01      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.1±0.36ms     3.4 MB/sec    1.00     12.2±0.32ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.07ms     7.2 MB/sec    1.02      2.4±0.08ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   259.4±12.62µs    11.4 MB/sec    1.00   260.6±12.72µs    11.3 MB/sec
formatter/pydantic/types.py                1.00      5.0±0.13ms     5.1 MB/sec    1.01      5.0±0.15ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.1±0.35ms     2.7 MB/sec    1.02     15.4±0.32ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.09ms     3.9 MB/sec    1.00      4.2±0.12ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   537.6±14.16µs     5.5 MB/sec    1.00   536.1±32.20µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.19ms     3.2 MB/sec    1.02      8.1±0.21ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.16ms     4.9 MB/sec    1.00      8.2±0.22ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1774.1±30.39µs     9.4 MB/sec    1.00  1758.6±65.03µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.5±6.54µs    13.8 MB/sec    1.00    215.0±9.99µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.07ms     6.8 MB/sec    1.00      3.8±0.09ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_tidy_imports/rules/banned_module_level_imports.rs`:38 on 2023-08-11 10:41_

this needs an example

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_tidy_imports/rules/banned_module_level_imports.rs`:42 on 2023-08-11 10:42_

nit: you can avoid the generic and pass the `TextRange` directly instead

---

_@konstin approved on 2023-08-11 10:45_

---

_Review requested from @charliermarsh by @konstin on 2023-08-11 10:45_

---

_Review requested from @dhruvmanila by @durumu on 2023-08-12 00:38_

---

_Comment by @durumu on 2023-08-12 00:55_

Sorry for the ping @dhruvmanila, I don't think you need to review this one -- I accidentally included some extra commits when I pushed 😅 (first github PR!)

---

_@durumu reviewed on 2023-08-12 00:55_

---

_Review comment by @durumu on `crates/ruff/src/rules/flake8_tidy_imports/rules/banned_module_level_imports.rs`:38 on 2023-08-12 00:55_

Added!

---

_@durumu reviewed on 2023-08-12 00:56_

---

_Review comment by @durumu on `crates/ruff/src/rules/flake8_tidy_imports/rules/banned_module_level_imports.rs`:42 on 2023-08-12 00:56_

Good idea, thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-12 03:45_

---

_Label `rule` added by @charliermarsh on 2023-08-12 03:45_

---

_Comment by @charliermarsh on 2023-08-12 04:18_

Generally looks good, will try to get to this soon.

---

_@charliermarsh reviewed on 2023-08-12 04:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_tidy_imports/rules/banned_module_level_imports.rs`:89 on 2023-08-12 04:20_

I think more robust here would be to use `checker.semantic().at_top_level()`, which will base its decision on scoping and parent statements. It's possible to have statements that aren't at the start of the line, but are at the top level, like:
```python
x = 1; import tensorflow
```

---

_@durumu reviewed on 2023-08-12 12:53_

---

_Review comment by @durumu on `crates/ruff/src/rules/flake8_tidy_imports/rules/banned_module_level_imports.rs`:89 on 2023-08-12 12:53_

Thanks! I shamelessly copied this from the `module_import_not_at_top_of_file` rule (`E402`), so I think that will need to be fixed too.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_tidy_imports/rules/banned_module_level_imports.rs`:89 on 2023-08-12 18:41_

Thanks for pointing that out, good call! Fixed in https://github.com/astral-sh/ruff/pull/6526/files.

---

_@charliermarsh reviewed on 2023-08-12 18:41_

---

_Merged by @charliermarsh on 2023-08-12 18:45_

---

_Closed by @charliermarsh on 2023-08-12 18:45_

---
