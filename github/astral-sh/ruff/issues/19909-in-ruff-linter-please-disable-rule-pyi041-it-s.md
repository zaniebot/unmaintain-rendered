---
number: 19909
title: "In Ruff linter, please disable rule PYI041: it's for type checking, linting it is a logical fallacy"
type: issue
state: closed
author: hunterhogan
labels: []
assignees: []
created_at: 2025-08-14T07:51:06Z
updated_at: 2025-08-15T02:59:21Z
url: https://github.com/astral-sh/ruff/issues/19909
synced_at: 2026-01-07T13:12:16-06:00
---

# In Ruff linter, please disable rule PYI041: it's for type checking, linting it is a logical fallacy

---

_Issue opened by @hunterhogan on 2025-08-14 07:51_

### Summary

# [PEP 484](https://peps.python.org/pep-0484/) § [The numeric tower](https://peps.python.org/pep-0484/#the-numeric-tower) is great.

As far as I can tell, @AlexWaygood thoroughly and clearly explained why the current rules for type checkers are goldilocks. (See, _e.g._, [#9810](https://github.com/astral-sh/ruff/issues/9810).)

## Type checkers ought to implement `int` ⊂ `float` ⊂ `complex`.

# A linter is not a type checker

## Fallacy of the converse

Given: Type checkers ought to treat `float` as `float | int`.

Non-sequitur: It does not follow that `float | int` is a tautology.

## Destroying information

> There are lots of classes in Python that you might not realise are subclasses of each other.

 _Originally posted by @AlexWaygood in [#9810](https://github.com/astral-sh/ruff/issues/9810#issuecomment-2577341967)_

A human user of a Python function with the annotation `float` would have less information than if the annotation were `float | int`. This fact is related to `float | int` not being tautologous.

## I shouldn't need an example with which everyone agrees

I sincerely hope that the above is enough to demonstrate that PEP 484 `int` ⊂ `float` ⊂ `complex` is a fantastic rule for type checkers and that [flake8-pyi](https://github.com/PyCQA/flake8-pyi) [Y041](https://github.com/PyCQA/flake8-pyi/blob/main/ERRORCODES.md#Y041) and [Ruff](https://docs.astral.sh/ruff/) [redundant-numeric-union (PYI041)](https://docs.astral.sh/ruff/rules/redundant-numeric-union/#redundant-numeric-union-pyi041) ought not to be linting rules.

<details><summary>But if I must provide a concrete example, read this.</summary>
<p>

## A real-world function

In [`hunterMakesPy`](https://pypi.org/project/hunterMakesPy/), [`defineConcurrencyLimit`](https://github.com/hunterhogan/hunterMakesPy/blob/81d94fc4da8c0f5ed2540c87aab51afa671bace9/hunterMakesPy/parseParameters.py) is a drop-in function that enables functions in other packages to honor user-requested CPU limits.

The hypothetical target user of `defineConcurrencyLimit` is developing a Python app with optional concurrency, so those users are relatively sophisticated. Nevertheless, one of these signatures is unequivocally superior to the other.

```python
def defineConcurrencyLimit(*, limit: bool | float | int | None, cpuTotal: int = multiprocessing.cpu_count()) -> int:

def defineConcurrencyLimit(*, limit: bool | float | None, cpuTotal: int = multiprocessing.cpu_count()) -> int:
```

But remember, most people will not see the signature above. At the moment, I use `defineConcurrencyLimit` in two public packages. In [`mapFolding`](https://pypi.org/project/mapFolding/), `defineConcurrencyLimit` silently handles the user-requested CPU limit in the function [`countFolds`](https://github.com/hunterhogan/mapFolding/blob/aea82fb82f2904bd23ae59ce521ccdf9350473ae/mapFolding/basecamp.py).

The target user of `countFolds` is a math enthusiast, and the option for limiting CPU usage is just one of many options for the function. The difference between `float` and `int` is important: the signature should signal there is a difference; the user shouldn't need to dig deep into the docstring to see the fact that `float | int` is absolutely not tautologous.

### The signature for most users

```python
def countFolds(listDimensions: Sequence[int] | None = None
                , pathLikeWriteFoldsTotal: PathLike[str] | PurePath | None = None
                , computationDivisions: int | str | None = None
                , CPUlimit: bool | float | int | None = None
                , mapShape: tuple[int, ...] | None = None
                , oeisID: str | None = None
                , oeis_n: int | None = None
                , flow: str | None = None
                ) -> int:

```

### The difference between `float` and `int` for the user

#### Parameters

- `CPUlimit`: bool | float | int | None = None
    If relevant, whether and how to limit the number of processors `countFolds` will use. `CPUlimit` is an irrelevant setting
    unless the computation is divided into more than one task with the `computationDivisions` parameter.
    - `False`, `None`, or `0`: No limits on processor usage; uses all available processors. All other values will
    potentially limit processor usage.
    - `True`: Yes, limit the processor usage; limits to 1 processor.
    - `int >= 1`: The maximum number of available processors to use.
    - `0 < float < 1`: The maximum number of processors to use expressed as a fraction of available processors.
    - `-1 < float < 0`: The number of processors to *not* use expressed as a fraction of available processors.
    - `int <= -1`: The number of available processors to *not* use.
    - If the value of `CPUlimit` is a `float` greater than 1 or less than -1, `countFolds` truncates the value to an `int`
    with the same sign as the `float`.

## Do you feel this example code is ambiguous or difficult to understand without more information?

If so, then don't have a linting rule that removes information and increases ambiguity.

</p>
</details> 



---

_Comment by @MichaReiser on 2025-08-14 09:01_

I don't think I follow the reasoning here. The reason this is a lint is exactly because `float | int` isn't a typing error, but specifying both is unnecessary. That's what `PYI041` checks. Whether you agree with this is a different question but that's why Ruff gives you the tools to disable the rule if you don't (it's not even enabled by default)

---

_Comment by @AlexWaygood on 2025-08-14 10:26_

@MichaReiser is correct. A type checker's primary purpose is to catch correctness issues in your code; it should generally refrain from emitting diagnostics about questions of style. This rule is purely stylistic in its purpose, however, so it would be improper for a type checker to complain about this issue. A type checker doesn't care whether you use `float` or `int | float` in a type annotation: it will treat the two type annotations exactly the same way. Which one you prefer is a question of style, and many people like to enforce a consistent style across their codebase -- that's what this rule is designed to help you with.

This rule is not enabled by default (only if you have `ALL`, `PYI` or `PYI041` selected in your configuration), and it can be easily disabled even if you have `ALL` or `PYI` selected -- for example, you can have `extend-select = ["PYI"]` and `ignore = ["PYI041"]`, for example. The rule is opinionated and subjective -- if you disagree with the style it enforces, I suggest you disable it in your project :-)

---

_Closed by @AlexWaygood on 2025-08-14 10:26_

---

_Comment by @hunterhogan on 2025-08-15 02:59_

The following is not an attempt to alter the outcome of this "Issue." It is strictly in case this "Issue" is analyzed in the future.

The statement "Creating PYI041 due to PEP 484 is a logical fallacy" is a claim of fact. AKA a constative. 

The constative was not addressed.



> I don't think I follow the reasoning here. The reason this is a lint is exactly because `float | int` isn't a typing error, but specifying both is unnecessary. That's what `PYI041` checks. Whether you agree with this is a different question but that's why Ruff gives you the tools to disable the rule if you don't (it's not even enabled by default)
https://github.com/astral-sh/ruff/issues/19909#issuecomment-3187606979


> [@MichaReiser](https://github.com/MichaReiser) is correct. A type checker's primary purpose is to catch correctness issues in your code; it should generally refrain from emitting diagnostics about questions of style. This rule is purely stylistic in its purpose, however, so it would be improper for a type checker to complain about this issue. A type checker doesn't care whether you use `float` or `int | float` in a type annotation: it will treat the two type annotations exactly the same way. Which one you prefer is a question of style, and many people like to enforce a consistent style across their codebase -- that's what this rule is designed to help you with.
> 
> This rule is not enabled by default (only if you have `ALL`, `PYI` or `PYI041` selected in your configuration), and it can be easily disabled even if you have `ALL` or `PYI` selected -- for example, you can have `extend-select = ["PYI"]` and `ignore = ["PYI041"]`, for example. The rule is opinionated and subjective -- if you disagree with the style it enforces, I suggest you disable it in your project :-)
https://github.com/astral-sh/ruff/issues/19909#issuecomment-3187943002


---
