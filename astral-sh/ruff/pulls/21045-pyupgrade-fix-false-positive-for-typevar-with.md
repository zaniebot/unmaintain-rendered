```yaml
number: 21045
title: "[`pyupgrade`] Fix false positive for `TypeVar` with default on Python <3.13 (`UP046`,`UP047`)"
type: pull_request
state: merged
author: prakhar1144
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-20929
created_at: 2025-10-23T13:22:31Z
updated_at: 2025-10-30T16:59:07Z
url: https://github.com/astral-sh/ruff/pull/21045
synced_at: 2026-01-12T15:57:15Z
```

# [`pyupgrade`] Fix false positive for `TypeVar` with default on Python <3.13 (`UP046`,`UP047`)

---

_@prakhar1144_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Type default for Type parameter was added in Python 3.13 (PEP 696).

`typing_extensions.TypeVar` backports the default argument to earlier versions.

`UP046` & `UP047` were getting triggered when `typing_extensions.TypeVar` with `default` argument was used on python version < 3.13

It shouldn't be triggered for python version < 3.13

This commit fixes the bug by adding a python version check before triggering them.

Fixes #20929.

## Test Plan

### Manual testing 1

As the issue author pointed out in https://github.com/astral-sh/ruff/issues/20929#issuecomment-3413194511, ran the following on `main` branch:
> % cargo run -p ruff -- check ../efax/ --target-version py312 --no-cache

<details><summary>Output</summary>

```zsh
   Compiling ruff_linter v0.14.1 (/Users/prakhar/ruff/crates/ruff_linter)
   Compiling ruff v0.14.1 (/Users/prakhar/ruff/crates/ruff)
   Compiling ruff_graph v0.1.0 (/Users/prakhar/ruff/crates/ruff_graph)
   Compiling ruff_workspace v0.0.0 (/Users/prakhar/ruff/crates/ruff_workspace)
   Compiling ruff_server v0.2.2 (/Users/prakhar/ruff/crates/ruff_server)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 6.72s
     Running `target/debug/ruff check ../efax/ --target-version py312 --no-cache`
UP046 Generic class `ExpectationParametrization` uses `Generic` subclass instead of type parameters
  --> /Users/prakhar/efax/efax/_src/expectation_parametrization.py:17:48
   |
17 | class ExpectationParametrization(Distribution, Generic[NP]):
   |                                                ^^^^^^^^^^^
18 |     """The expectation parametrization of an exponential family distribution.
   |
help: Use type parameters

UP046 Generic class `ExpToNat` uses `Generic` subclass instead of type parameters
  --> /Users/prakhar/efax/efax/_src/mixins/exp_to_nat/exp_to_nat.py:27:68
   |
26 | @dataclass
27 | class ExpToNat(ExpectationParametrization[NP], SimpleDistribution, Generic[NP]):
   |                                                                    ^^^^^^^^^^^
28 |     """This mixin implements the conversion from expectation to natural parameters.
   |
help: Use type parameters

UP046 Generic class `HasEntropyEP` uses `Generic` subclass instead of type parameters
  --> /Users/prakhar/efax/efax/_src/mixins/has_entropy.py:25:20
   |
23 |                    HasEntropy,
24 |                    JaxAbstractClass,
25 |                    Generic[NP]):
   |                    ^^^^^^^^^^^
26 |     @abstract_jit
27 |     @abstractmethod
   |
help: Use type parameters

UP046 Generic class `HasEntropyNP` uses `Generic` subclass instead of type parameters
  --> /Users/prakhar/efax/efax/_src/mixins/has_entropy.py:64:20
   |
62 | class HasEntropyNP(NaturalParametrization[EP],
63 |                    HasEntropy,
64 |                    Generic[EP]):
   |                    ^^^^^^^^^^^
65 |     @jit
66 |     @final
   |
help: Use type parameters

UP046 Generic class `NaturalParametrization` uses `Generic` subclass instead of type parameters
  --> /Users/prakhar/efax/efax/_src/natural_parametrization.py:43:30
   |
41 | class NaturalParametrization(Distribution,
42 |                              JaxAbstractClass,
43 |                              Generic[EP, Domain]):
   |                              ^^^^^^^^^^^^^^^^^^^
44 |     """The natural parametrization of an exponential family distribution.
   |
help: Use type parameters

UP046 Generic class `Structure` uses `Generic` subclass instead of type parameters
  --> /Users/prakhar/efax/efax/_src/structure/structure.py:31:17
   |
30 | @dataclass
31 | class Structure(Generic[P]):
   |                 ^^^^^^^^^^
32 |     """This class generalizes the notion of type for Distribution objects.
   |
help: Use type parameters

UP046 Generic class `DistributionInfo` uses `Generic` subclass instead of type parameters
  --> /Users/prakhar/efax/tests/distribution_info.py:20:24
   |
20 | class DistributionInfo(Generic[NP, EP, Domain]):
   |                        ^^^^^^^^^^^^^^^^^^^^^^^
21 |     def __init__(self, dimensions: int = 1, safety: float = 0.0) -> None:
22 |         super().__init__()
   |
help: Use type parameters

Found 7 errors.
No fixes available (7 hidden fixes can be enabled with the `--unsafe-fixes` option).
```
</details> 

Running it after the changes:
```zsh
ruff % cargo run -p ruff -- check ../efax/ --target-version py312 --no-cache
   Compiling ruff_linter v0.14.1 (/Users/prakhar/ruff/crates/ruff_linter)
   Compiling ruff v0.14.1 (/Users/prakhar/ruff/crates/ruff)
   Compiling ruff_graph v0.1.0 (/Users/prakhar/ruff/crates/ruff_graph)
   Compiling ruff_workspace v0.0.0 (/Users/prakhar/ruff/crates/ruff_workspace)
   Compiling ruff_server v0.2.2 (/Users/prakhar/ruff/crates/ruff_server)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 7.86s
     Running `target/debug/ruff check ../efax/ --target-version py312 --no-cache`
All checks passed!
```

---

### Manual testing 2

Ran the check on the following script (mainly to verify `UP047`):
```py
from __future__ import annotations                                                                                                                                                    

from typing import Generic

from typing_extensions import TypeVar

T = TypeVar("T", default=int)


def generic_function(var: T) -> T:
    return var


Q = TypeVar("Q", default=str)


class GenericClass(Generic[Q]):
    var: Q
```

On `main` branch:
> ruff % cargo run -p ruff -- check ~/up046.py --target-version py312 --preview --no-cache

<details><summary>Output</summary>

```zsh
   Compiling ruff_linter v0.14.1 (/Users/prakhar/ruff/crates/ruff_linter)
   Compiling ruff v0.14.1 (/Users/prakhar/ruff/crates/ruff)
   Compiling ruff_graph v0.1.0 (/Users/prakhar/ruff/crates/ruff_graph)
   Compiling ruff_workspace v0.0.0 (/Users/prakhar/ruff/crates/ruff_workspace)
   Compiling ruff_server v0.2.2 (/Users/prakhar/ruff/crates/ruff_server)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 7.43s
     Running `target/debug/ruff check /Users/prakhar/up046.py --target-version py312 --preview --no-cache`
UP047 Generic function `generic_function` should use type parameters
  --> /Users/prakhar/up046.py:10:5
   |
10 | def generic_function(var: T) -> T:
   |     ^^^^^^^^^^^^^^^^^^^^^^^^
11 |     return var
   |
help: Use type parameters

UP046 Generic class `GenericClass` uses `Generic` subclass instead of type parameters
  --> /Users/prakhar/up046.py:17:20
   |
17 | class GenericClass(Generic[Q]):
   |                    ^^^^^^^^^^
18 |     var: Q
   |
help: Use type parameters

Found 2 errors.
No fixes available (2 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

</details> 

After the fix (this branch):
```zsh
ruff % cargo run -p ruff -- check ~/up046.py --target-version py312 --preview --no-cache
   Compiling ruff_linter v0.14.1 (/Users/prakhar/ruff/crates/ruff_linter)
   Compiling ruff v0.14.1 (/Users/prakhar/ruff/crates/ruff)
   Compiling ruff_graph v0.1.0 (/Users/prakhar/ruff/crates/ruff_graph)
   Compiling ruff_workspace v0.0.0 (/Users/prakhar/ruff/crates/ruff_workspace)
   Compiling ruff_server v0.2.2 (/Users/prakhar/ruff/crates/ruff_server)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 7.40s
     Running `target/debug/ruff check /Users/prakhar/up046.py --target-version py312 --preview --no-cache`
All checks passed!
```


---

_Review requested from @ntBre by @ntBre on 2025-10-23 18:16_

---

_Label `bug` added by @ntBre on 2025-10-23 18:16_

---

_Label `rule` added by @ntBre on 2025-10-23 18:16_

---

_Comment by @github-actions[bot] on 2025-10-23 18:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-10-23 20:20_

Thank you! The code change and new comments look great, but would you mind converting your manual tests into regression tests in the repo?

If you already have separate Python files, they would fit nicely as new test cases alongside files like [UP046_1.py](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/resources/test/fixtures/pyupgrade/UP046_1.py) and `test_case` entries here:

https://github.com/astral-sh/ruff/blob/83a3bc4ee94de552d5cec9a3146aff00dade6903/crates/ruff_linter/src/rules/pyupgrade/mod.rs#L114-L126

Although you may need a separate test function so that you can set the version like we do here:

https://github.com/astral-sh/ruff/blob/83a3bc4ee94de552d5cec9a3146aff00dade6903/crates/ruff_linter/src/rules/pyupgrade/mod.rs#L244

---

_Comment by @prakhar1144 on 2025-10-27 07:14_

Thanks for the review!

As an update - I got busy with festivals around; I plan to complete this in the next 2-3 days.

---

_Comment by @prakhar1144 on 2025-10-29 22:44_

@ntBre  I've added tests. Ready to review.

---

_@ntBre approved on 2025-10-30 16:53_

This is great, thank you!

---

_Renamed from "[`pyupgrade`] Fix false positive for TypeVar with default on Python<3.13 (`UP046`,`UP047`)" to "[`pyupgrade`] Fix false positive for `TypeVar` with default on Python <3.13 (`UP046`,`UP047`)" by @ntBre on 2025-10-30 16:53_

---

_Merged by @ntBre on 2025-10-30 16:59_

---

_Closed by @ntBre on 2025-10-30 16:59_

---
