```yaml
number: 4293
title: "[`flake8-pyi`] Implement `unannotated-assignment-in-stub` (`PY052`)"
type: pull_request
state: merged
author: sladyn98
labels:
  - rule
assignees: []
merged: true
base: main
head: implemenent-Y052
created_at: 2023-05-08T22:31:46Z
updated_at: 2023-05-16T02:28:24Z
url: https://github.com/astral-sh/ruff/pull/4293
synced_at: 2026-01-12T03:50:02Z
```

# [`flake8-pyi`] Implement `unannotated-assignment-in-stub` (`PY052`)

---

_Pull request opened by @sladyn98 on 2023-05-08 22:31_

This pull request introduces a new function, assignment_default_in_async_stub, to handle async stub cases in flake8_pyi rules. It covers overlapping cases from both `assignment_default_in_stub` and `annotated_assignment_default_in_stub` and calls them only when necessary.

* Implemented is_inside_class and is_inside_enum_class functions to identify if the given `Expr` is inside a class or an enum class, respectively.
* Added the `assignment_default_in_async_stub `function in simple_defaults.rs to handle cases where assignments have default values in async stubs. The function checks if the current context is inside a non-enum class and calls either `assignment_default_in_stub` or `annotated_assignment_default_in_stub` accordingly.
* Updated `ast/mod.rs` to call `assignment_default_in_async_stub` when the is_stub condition is true.
* Fixed type mismatch errors in mod.rs and updated the code to call the new function with the correct argument types.

This PR aims to improve the handling of async stubs in flake8_pyi rules and provide better diagnostics and error messages.




---

_Comment by @sladyn98 on 2023-05-08 22:32_

@MichaReiser Opened a new PR

---

_Comment by @github-actions[bot] on 2023-05-08 23:08_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.8±0.06ms     3.0 MB/sec    1.01     13.9±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.0±0.98µs     7.1 MB/sec    1.00    416.1±2.87µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.09ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1446.5±3.41µs    11.5 MB/sec    1.00   1441.9±3.94µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.5±0.23µs    18.6 MB/sec    1.00    158.6±0.64µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.00ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.4±0.00ms     7.5 MB/sec    1.01      5.4±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1045.5±0.96µs    15.9 MB/sec    1.01   1055.8±1.90µs    15.8 MB/sec
parser/numpy/globals.py                    1.00    106.1±0.14µs    27.8 MB/sec    1.00    106.3±0.23µs    27.7 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.01ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.22ms     2.5 MB/sec    1.01     16.6±0.26ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.07ms     3.9 MB/sec    1.00      4.2±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    496.7±6.72µs     5.9 MB/sec    1.00    493.9±6.84µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.09ms     3.7 MB/sec    1.02      7.0±0.14ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.08ms     5.0 MB/sec    1.01      8.2±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1715.0±21.58µs     9.7 MB/sec    1.02  1749.3±40.21µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.0±3.18µs    15.5 MB/sec    1.03    195.3±6.40µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.05ms     6.9 MB/sec    1.01      3.7±0.04ms     6.9 MB/sec
parser/large/dataset.py                    1.01      6.7±0.05ms     6.1 MB/sec    1.00      6.6±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1274.6±16.35µs    13.1 MB/sec    1.00  1268.3±21.67µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    131.4±2.35µs    22.5 MB/sec    1.00    130.9±2.46µs    22.5 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.03ms     8.9 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Converted to draft by @sladyn98 on 2023-05-10 17:28_

---

_Comment by @sladyn98 on 2023-05-10 17:29_

@charliermarsh could I get some feedback on this one, like do I need to add a test for this

---

_Comment by @charliermarsh on 2023-05-10 17:36_

@sladyn98 - Yup, will review this today.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-05-10 17:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:492 on 2023-05-10 18:16_

We need to define a new diagnostic type to represent Y052 itself: `UnannotatedAssignmentInStub`. Can you look at how `AssignmentDefaultInStub` is defined and define a new diagnostic for it?

---

_@charliermarsh reviewed on 2023-05-10 18:16_

---

_@charliermarsh reviewed on 2023-05-10 18:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:475 on 2023-05-10 18:16_

What's the relevance of "async" in these various names and contexts? I don't think Y052 has any relationship to async, but trying to understand.

---

_@charliermarsh reviewed on 2023-05-10 18:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:500 on 2023-05-10 18:17_

Rather than add a new method here, can we instead add the enum-related logic within `assignment_default_in_stub`? We're now checking `is_valid_default_value_without_annotation` multiple times, because it also gets called within `assignment_default_in_stub`. And we're avoiding a variety of conditions that need to be checked.

---

_@sladyn98 reviewed on 2023-05-11 19:26_

---

_Review comment by @sladyn98 on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:492 on 2023-05-11 19:26_

Yeah I added it to the `codes.rs` and `registry.rs`

---

_@sladyn98 reviewed on 2023-05-11 19:29_

---

_Review comment by @sladyn98 on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:475 on 2023-05-11 19:29_

Oh I guess I got the naming wrong, I agree it does not have anything to do with async here. 

---

_Review requested from @charliermarsh by @sladyn98 on 2023-05-11 20:05_

---

_@sladyn98 reviewed on 2023-05-11 20:06_

---

_Review comment by @sladyn98 on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:500 on 2023-05-11 20:06_

Yeah this makes sense, I refactored it to now include the checks inside assignment_default_in_stub

---

_Marked ready for review by @charliermarsh on 2023-05-16 01:55_

---

_Renamed from "Implement PY052" to "[`flake8-pyi`] Implement `unannotated-assignment-in-stub` (`PY052`)" by @charliermarsh on 2023-05-16 01:56_

---

_Label `rule` added by @charliermarsh on 2023-05-16 01:58_

---

_Merged by @charliermarsh on 2023-05-16 02:06_

---

_Closed by @charliermarsh on 2023-05-16 02:06_

---
