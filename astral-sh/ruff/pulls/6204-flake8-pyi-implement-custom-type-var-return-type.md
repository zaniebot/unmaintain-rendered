```yaml
number: 6204
title: "[`flake8-pyi`] Implement `custom_type_var_return_type` (`PYI019`)"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/PYI019
created_at: 2023-07-31T17:18:37Z
updated_at: 2023-08-03T11:20:32Z
url: https://github.com/astral-sh/ruff/pull/6204
synced_at: 2026-01-12T15:55:20Z
```

# [`flake8-pyi`] Implement `custom_type_var_return_type` (`PYI019`)

---

_@qdegraaf_

## Summary

Implements `Y019` from [flake8-pyi](https://github.com/PyCQA/flake8-pyi). 

The rule checks if

-  instance methods that return `self` 
-  class methods that return an instance of `cls`
- `__new__` methods

Return a custom `TypeVar` instead of `typing.Self` and raises a violation if this is the case. The rule also covers [PEP-695](https://peps.python.org/pep-0695/) syntax as introduced in upstream in https://github.com/PyCQA/flake8-pyi/pull/402  

## Test Plan

Added fixtures with test cases from upstream implementation (plus additional one for an excluded edge case, mentioned in upstream implementation)

## Issue link:

Refers: https://github.com/astral-sh/ruff/issues/848

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_pyi/rules/custom_typevar_return.rs`:145 on 2023-07-31 17:22_

Upstream implementation mentions the exception as follows:

```python
# Don't error if the first argument is annotated
# with `builtins.type[T]` or `typing.Type[T]`
# These are edge cases, and it's hard to give good error messages for them.
if not _is_name(first_arg_annotation.value, "type"):
    return
```
Source: https://github.com/PyCQA/flake8-pyi/blob/2a86db8271dc500247a8a69419536240334731cf/pyi.py#L1843C1-L1847 

Unsure if this was a typo or I am missing something. But it seems to me that `builtins.type[T]` is not the edge case but the thing we want to check. 

I've implemented what I think they meant. But just mentioning it here as I might be confused or missing something.

---

_@qdegraaf reviewed on 2023-07-31 17:22_

---

_@qdegraaf reviewed on 2023-07-31 17:24_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_pyi/rules/custom_typevar_return.rs`:19 on 2023-07-31 17:24_

Scanned through PRs relating to Y019 in upstream repo but found no further explanation as to why this is problematic precisely.

I assumed, similar to `NonSelfReturnType` (or `PYI034`) it's hard for type checkers to infer the type when subclassed. 

Input for a more complete explanation is welcomed.

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI019.pyi`:4 on 2023-07-31 17:34_

Should there be coverage for PEP 695 declarations like `type _S = ...`? or is that not relevant?

---

_@zanieb reviewed on 2023-07-31 17:34_

---

_Comment by @github-actions[bot] on 2023-07-31 17:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.9±0.05ms     4.1 MB/sec    1.00      9.8±0.05ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.00  1945.8±15.00µs     8.6 MB/sec    1.00  1939.5±10.08µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    216.9±3.77µs    13.6 MB/sec    1.00    216.1±0.99µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.02ms     6.1 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.10ms     3.1 MB/sec    1.00     13.1±0.10ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     5.0 MB/sec    1.00      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    453.8±0.88µs     6.5 MB/sec    1.00    454.5±1.16µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.00      5.9±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.00      6.8±0.03ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1432.0±4.15µs    11.6 MB/sec    1.00   1426.3±7.56µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    157.2±1.53µs    18.8 MB/sec    1.00    156.0±1.06µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     12.3±0.52ms     3.3 MB/sec    1.00     12.2±0.30ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.08ms     7.1 MB/sec    1.03      2.4±0.14ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   262.5±12.21µs    11.2 MB/sec    1.02   267.6±18.42µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      5.1±0.18ms     5.0 MB/sec    1.03      5.3±0.21ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     16.4±0.51ms     2.5 MB/sec    1.01     16.6±0.39ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.16ms     3.8 MB/sec    1.01      4.4±0.13ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.03   549.7±72.49µs     5.4 MB/sec    1.00   534.5±23.76µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.7±0.28ms     3.3 MB/sec    1.00      7.5±0.24ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.19ms     4.5 MB/sec    1.00      9.0±0.29ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1878.1±73.24µs     8.9 MB/sec    1.00  1856.0±57.93µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   210.5±13.40µs    14.0 MB/sec    1.00    211.0±9.84µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.11ms     6.5 MB/sec    1.01      4.0±0.17ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@AlexWaygood reviewed on 2023-07-31 19:59_

---

_Review comment by @AlexWaygood on `crates/ruff/src/rules/flake8_pyi/rules/custom_typevar_return.rs`:145 on 2023-07-31 19:59_

I think what I was trying to say in that comment was that we emit Y019 for this:

```python
def __new__(cls: type[S]) -> S: ...
```

But not for any of these:

```python
def __new__(cls: builtins.type[S]) -> S: ...
def __new__(cls: Type[S]) -> S: ...
def __new__(cls: typing.Type[S]) -> S: ...
```

Whereas the behaviour most consistent with how flake8-pyi usually works would be to emit the same error for all of them.

I can't promise that this is the optimal  behaviour, though, just what seemed most reasonable to me at the time :D

---

_@qdegraaf reviewed on 2023-07-31 20:15_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_pyi/rules/custom_typevar_return.rs`:145 on 2023-07-31 20:15_

Hey Alex,

Thanks for chipping in! Then the confusion is on me, and not the comment 😅 .

Can you explain why `builtins.type[S]` is more problematic than just `type[S]`? Or `typing.Type` for that matter? Is it just a message formatting thing or are there functional differences? 

---

_@AlexWaygood reviewed on 2023-07-31 20:22_

---

_Review comment by @AlexWaygood on `crates/ruff/src/rules/flake8_pyi/rules/custom_typevar_return.rs`:145 on 2023-07-31 20:22_

Yeah I think, as the comment says, it just felt complicated to get a good error message in that kind of situation without excessively complicating the code. For `Type` and `typing.Type`, I think I might also have been worried about having two error messages telling the user to do contradictory things (one flake8-pyi code tells you to use `type` instead of `Type`; the other tells you to use `Self`, what do you do?!)

But I can't really remember the exact reasoning for making the decision (it was a while ago!) and it's entirely possible we could/should revisit it at flake8-pyi!

---

_@qdegraaf reviewed on 2023-07-31 20:24_

---

_Review comment by @qdegraaf on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI019.pyi`:4 on 2023-07-31 20:24_

The full heuristic, as derived from upsteam, right now is:

```python
fn is_likely_private_typevar(
    checker: &Checker,
    tvar_name: &str,
    type_params: &[TypeParam],
) -> bool {
    if tvar_name.starts_with('_') {
        return true;
    }
    if checker.settings.target_version < PythonVersion::Py312 {
        return false;
    }

    for param in type_params {
        if let TypeParam::TypeVar(ast::TypeParamTypeVar { name, .. }) = param {
            return name == tvar_name;
        }
    }
    false
}
```

Which should catch 

```python
type _S = ...
type T = ...

class Foo:
    def foo[T](self: T) -> T: ...
    def bar(self: _S) -> _S: ...
```

But will miss any TypeVar or type not prefixed by a "_" and not used with PEP-695 style type parameters regardless of Python version. 

I tried letting go of upstream's implementation and replacing the heuristic with a check on the binding to see if it was assigned to a TypeVar or type but could not get it to work (and it was not pretty). If this is possible and doable in a performant manner with some input, I would be open to replacing the heuristic with that. 

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/flake8_pyi/rules/custom_typevar_return.rs`:145 on 2023-07-31 20:33_

Clear! I'll leave it like this for now. Thanks! 

---

_@qdegraaf reviewed on 2023-07-31 20:33_

---

_@AlexWaygood reviewed on 2023-07-31 20:36_

---

_Review comment by @AlexWaygood on `crates/ruff/src/rules/flake8_pyi/rules/custom_typevar_return.rs`:19 on 2023-07-31 20:36_

This rule (unlike Y034) isn't to do with correctness; it's mainly just to enforce a consistent style and not have to have needles TypeVar definitions cluttering up stub files

---

_@zanieb reviewed on 2023-08-01 14:33_

---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI019.pyi`:4 on 2023-08-01 14:33_

cc @charliermarsh who will probably have a better suggestion than me

---

_@charliermarsh reviewed on 2023-08-03 00:32_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI019.pyi`:4 on 2023-08-03 00:32_

I think it's okay to use this heuristic for now.

---

_Label `rule` added by @charliermarsh on 2023-08-03 00:33_

---

_Renamed from "[`flake8-pyi`] Implement `PYI019`" to "[`flake8-pyi`] Implement `custom_type_var_return_type` (`PYI019`)" by @charliermarsh on 2023-08-03 00:34_

---

_Merged by @charliermarsh on 2023-08-03 00:42_

---

_Closed by @charliermarsh on 2023-08-03 00:42_

---

_Branch deleted on 2023-08-03 11:20_

---
