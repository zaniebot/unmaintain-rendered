```yaml
number: 19964
title: "[ty] Use specialized parameter type for overload filter"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dhruv/use-specialized-parameter-type
created_at: 2025-08-18T09:31:10Z
updated_at: 2025-08-20T04:09:07Z
url: https://github.com/astral-sh/ruff/pull/19964
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Use specialized parameter type for overload filter

---

_Pull request opened by @dhruvmanila on 2025-08-18 09:31_

## Summary

Closes: https://github.com/astral-sh/ty/issues/669

(This turned out to be simpler that I thought :))

## Test Plan

Update existing test cases.

### Ecosystem report

Most of them are basically because ty has now started inferring more precise types for the return type to an overloaded call and a lot of the types are defined using type aliases, here's some examples:

<details><summary>Details</summary>
<p>

> attrs (https://github.com/python-attrs/attrs)
> + tests/test_make.py:146:14: error[unresolved-attribute] Type `Literal[42]` has no attribute `default`
> - Found 555 diagnostics
> + Found 556 diagnostics

This is accurate now that we infer the type as `Literal[42]` instead of `Unknown` (Pyright infers it as `int`)

> optuna (https://github.com/optuna/optuna)
> + optuna/_gp/search_space.py:181:53: error[invalid-argument-type] Argument to function `_round_one_normalized_param` is incorrect: Expected `tuple[int | float, int | float]`, found `tuple[Unknown | ndarray[Unknown, <class 'float'>], Unknown | ndarray[Unknown, <class 'float'>]]`
> + optuna/_gp/search_space.py:181:83: error[invalid-argument-type] Argument to function `_round_one_normalized_param` is incorrect: Expected `int | float`, found `Unknown | ndarray[Unknown, <class 'float'>]`
> + tests/gp_tests/test_search_space.py:109:13: error[invalid-argument-type] Argument to function `_unnormalize_one_param` is incorrect: Expected `tuple[int | float, int | float]`, found `Unknown | ndarray[Unknown, <class 'float'>]`
> + tests/gp_tests/test_search_space.py:110:13: error[invalid-argument-type] Argument to function `_unnormalize_one_param` is incorrect: Expected `int | float`, found `Unknown | ndarray[Unknown, <class 'float'>]`
> - Found 559 diagnostics
> + Found 563 diagnostics

Same as above where ty is now inferring a more precise type like `Unknown | ndarray[tuple[int, int], <class 'float'>]` instead of just `Unknown` as before

> jinja (https://github.com/pallets/jinja)
> + src/jinja2/bccache.py:298:39: error[invalid-argument-type] Argument to bound method `write_bytecode` is incorrect: Expected `IO[bytes]`, found `_TemporaryFileWrapper[str]`
> - Found 186 diagnostics
> + Found 187 diagnostics

This requires support for type aliases to match the correct overload.

> hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
> + src/hydra_zen/wrapper/_implementations.py:945:16: error[invalid-return-type] Return type does not match returned value: expected `DataClass_ | type[@Todo(type[T] for protocols)] | ListConfig | DictConfig`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | dict[Any, Any] | (ListConfig & dict[Unknown, Unknown]) | (DictConfig & dict[Unknown, Unknown]) | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (ListConfig & list[Unknown]) | (DictConfig & list[Unknown])`
> + tests/annotations/behaviors.py:60:28: error[call-non-callable] Object of type `Path` is not callable
> + tests/annotations/behaviors.py:64:21: error[call-non-callable] Object of type `Path` is not callable
> + tests/annotations/declarations.py:167:17: error[call-non-callable] Object of type `Path` is not callable
> + tests/annotations/declarations.py:524:17: error[unresolved-attribute] Type `<class 'int'>` has no attribute `_target_`
> - Found 561 diagnostics
> + Found 566 diagnostics

Same as above, this requires support for type aliases to match the correct overload.

> paasta (https://github.com/yelp/paasta)
> + paasta_tools/utils.py:4188:19: warning[redundant-cast] Value is already of type `list[str]`
> - Found 888 diagnostics
> + Found 889 diagnostics

This is correct.

> colour (https://github.com/colour-science/colour)
> + colour/plotting/diagrams.py:448:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
> + colour/plotting/diagrams.py:462:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
> + colour/plotting/models.py:419:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
> + colour/plotting/temperature.py:230:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
> + colour/plotting/temperature.py:474:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
> + colour/plotting/temperature.py:495:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
> + colour/plotting/temperature.py:513:13: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `int | float`, found `ndarray[@Todo(Support for `typing.TypeAlias`), dtype[Unknown]]`
> + colour/plotting/temperature.py:514:13: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `int | float`, found `ndarray[@Todo(Support for `typing.TypeAlias`), dtype[Unknown]]`
> - Found 480 diagnostics
> + Found 488 diagnostics

Most of them are correct except for the last two diagnostics which I'm not sure
what's happening, it's trying to index into an `np.ndarray` type (which is
inferred correctly) but I think it might be picking up an incorrect overload
for the `__getitem__` method.

Scipy's diagnostics also requires support for type alises to pick the correct overload.

</p>
</details> 

---

_Comment by @github-actions[bot] on 2025-08-18 09:33_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-18 09:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ tests/test_make.py:146:14: error[unresolved-attribute] Type `Literal[42]` has no attribute `default`
- Found 555 diagnostics
+ Found 556 diagnostics

optuna (https://github.com/optuna/optuna)
+ optuna/_gp/search_space.py:181:53: error[invalid-argument-type] Argument to function `_round_one_normalized_param` is incorrect: Expected `tuple[int | float, int | float]`, found `tuple[Unknown | ndarray[Unknown, <class 'float'>], Unknown | ndarray[Unknown, <class 'float'>]]`
+ optuna/_gp/search_space.py:181:83: error[invalid-argument-type] Argument to function `_round_one_normalized_param` is incorrect: Expected `int | float`, found `Unknown | ndarray[Unknown, <class 'float'>]`
+ tests/gp_tests/test_search_space.py:109:13: error[invalid-argument-type] Argument to function `_unnormalize_one_param` is incorrect: Expected `tuple[int | float, int | float]`, found `Unknown | ndarray[Unknown, <class 'float'>]`
+ tests/gp_tests/test_search_space.py:110:13: error[invalid-argument-type] Argument to function `_unnormalize_one_param` is incorrect: Expected `int | float`, found `Unknown | ndarray[Unknown, <class 'float'>]`
- Found 559 diagnostics
+ Found 563 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/bccache.py:298:39: error[invalid-argument-type] Argument to bound method `write_bytecode` is incorrect: Expected `IO[bytes]`, found `_TemporaryFileWrapper[str]`
- Found 186 diagnostics
+ Found 187 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/wrapper/_implementations.py:945:16: error[invalid-return-type] Return type does not match returned value: expected `DataClass_ | type[@Todo(type[T] for protocols)] | ListConfig | DictConfig`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | dict[Any, Any] | (ListConfig & dict[Unknown, Unknown]) | (DictConfig & dict[Unknown, Unknown]) | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (ListConfig & list[Unknown]) | (DictConfig & list[Unknown])`
+ tests/annotations/behaviors.py:60:28: error[call-non-callable] Object of type `Path` is not callable
+ tests/annotations/behaviors.py:64:21: error[call-non-callable] Object of type `Path` is not callable
+ tests/annotations/declarations.py:167:17: error[call-non-callable] Object of type `Path` is not callable
+ tests/annotations/declarations.py:524:17: error[unresolved-attribute] Type `<class 'int'>` has no attribute `_target_`
- Found 561 diagnostics
+ Found 566 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/utils.py:4188:19: warning[redundant-cast] Value is already of type `list[str]`
- Found 888 diagnostics
+ Found 889 diagnostics

colour (https://github.com/colour-science/colour)
+ colour/plotting/diagrams.py:448:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
+ colour/plotting/diagrams.py:462:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
+ colour/plotting/models.py:419:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
+ colour/plotting/temperature.py:230:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
+ colour/plotting/temperature.py:474:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
+ colour/plotting/temperature.py:495:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[@Todo(Support for `typing.TypeAlias`)]`, found `ndarray[tuple[int, int, int], dtype[Unknown]]`
+ colour/plotting/temperature.py:513:13: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `int | float`, found `ndarray[@Todo(Support for `typing.TypeAlias`), dtype[Unknown]]`
+ colour/plotting/temperature.py:514:13: error[invalid-argument-type] Argument to bound method `text` is incorrect: Expected `int | float`, found `ndarray[@Todo(Support for `typing.TypeAlias`), dtype[Unknown]]`
- Found 480 diagnostics
+ Found 488 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ tests/test_pipeline_files.py:364:5: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[str]`
+ tests/test_pipeline_files.py:365:5: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_files.py:367:5: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[str]`
+ tests/test_pipeline_files.py:368:5: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_images.py:312:5: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[str]`
+ tests/test_pipeline_images.py:313:5: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_images.py:315:5: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[str]`
+ tests/test_pipeline_images.py:316:5: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[dict[str, str]]`
- Found 1058 diagnostics
+ Found 1066 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:553:21: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `(Unknown & ~None) | Iterable[Unknown | type[FrameDeferred]] | Unknown` is possibly unbound
+ static_frame/core/bus.py:553:21: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `(Unknown & ~None) | Iterable[Unknown | type[FrameDeferred]]` is possibly unbound
- static_frame/core/bus.py:571:40: warning[possibly-unbound-attribute] Attribute `copy` on type `(Unknown & ~None) | Iterable[Unknown | type[FrameDeferred]] | Unknown` is possibly unbound
+ static_frame/core/bus.py:571:40: warning[possibly-unbound-attribute] Attribute `copy` on type `(Unknown & ~None) | Iterable[Unknown | type[FrameDeferred]]` is possibly unbound
+ static_frame/core/join.py:102:42: error[invalid-argument-type] Argument to function `nonzero_1d` is incorrect: Expected `ndarray[Unknown, Unknown]`, found `bool[bool] | @Todo(unknown type subscript)`
- Found 1778 diagnostics
+ Found 1779 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/_loss/loss.py:370:12: warning[possibly-unbound-attribute] Attribute `ndim` on type `Unknown | None` is possibly unbound
+ sklearn/_loss/loss.py:370:12: warning[possibly-unbound-attribute] Attribute `ndim` on type `Unknown | None | (Unknown & ~None)` is possibly unbound
- sklearn/_loss/loss.py:370:38: warning[possibly-unbound-attribute] Attribute `shape` on type `Unknown | None` is possibly unbound
+ sklearn/_loss/loss.py:370:38: warning[possibly-unbound-attribute] Attribute `shape` on type `Unknown | None | (Unknown & ~None)` is possibly unbound
- sklearn/_loss/loss.py:371:27: warning[possibly-unbound-attribute] Attribute `squeeze` on type `Unknown | None` is possibly unbound
+ sklearn/_loss/loss.py:371:27: warning[possibly-unbound-attribute] Attribute `squeeze` on type `Unknown | None | (Unknown & ~None)` is possibly unbound

sympy (https://github.com/sympy/sympy)
+ sympy/matrices/matrixbase.py:3268:16: error[missing-argument] No argument provided for required parameter 2
- Found 12993 diagnostics
+ Found 12994 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/io/_idl.py:782:9: error[no-matching-overload] No overload of bound method `write` matches arguments
+ scipy/io/_idl.py:789:13: error[no-matching-overload] No overload of bound method `write` matches arguments
+ scipy/io/_idl.py:801:17: error[no-matching-overload] No overload of bound method `write` matches arguments
+ scipy/io/_idl.py:802:17: error[no-matching-overload] No overload of bound method `write` matches arguments
+ scipy/io/_idl.py:818:13: error[no-matching-overload] No overload of bound method `write` matches arguments
+ scipy/io/_idl.py:819:13: error[no-matching-overload] No overload of bound method `write` matches arguments
+ scipy/io/_idl.py:821:13: error[no-matching-overload] No overload of bound method `write` matches arguments
- scipy/linalg/tests/test_basic.py:1426:29: error[no-matching-overload] No overload of function `tri` matches arguments
- scipy/linalg/tests/test_basic.py:1438:29: error[no-matching-overload] No overload of function `tri` matches arguments
+ scipy/stats/_mstats_extras.py:88:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `flat` on type `ndarray[tuple[int, int], Unknown]` with custom `__set__` method
- Found 6394 diagnostics
+ Found 6400 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~22MB
+     memo metadata = ~21MB

```
</details>


---

_Label `ty` added by @dhruvmanila on 2025-08-18 10:03_

---

_Label `ecosystem-analyzer` added by @dhruvmanila on 2025-08-18 10:03_

---

_Label `ecosystem-analyzer` removed by @dhruvmanila on 2025-08-19 05:47_

---

_Label `ecosystem-analyzer` added by @dhruvmanila on 2025-08-19 05:48_

---

_Comment by @github-actions[bot] on 2025-08-19 05:57_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 14 | 0 | 0 |
| `invalid-assignment` | 9 | 0 | 0 |
| `no-matching-overload` | 7 | 2 | 0 |
| `possibly-unbound-attribute` | 0 | 0 | 4 |
| `call-non-callable` | 3 | 0 | 0 |
| `unresolved-attribute` | 2 | 0 | 0 |
| `invalid-return-type` | 1 | 0 | 0 |
| `missing-argument` | 1 | 0 | 0 |
| `possibly-unbound-implicit-call` | 0 | 0 | 1 |
| `redundant-cast` | 1 | 0 | 0 |
| **Total** | **38** | **2** | **5** |

**[Full report with detailed diff](https://dhruv-use-specialized-parame.ecosystem-663.pages.dev/diff)**


---

_Comment by @dhruvmanila on 2025-08-19 09:14_

> sympy (https://github.com/sympy/sympy)
> + [sympy/matrices/matrixbase.py:3268:16](https://github.com/sympy/sympy/blob/97e513139dee148567782c4b15fb56fa61e3c1ac/sympy/matrices/matrixbase.py#L3268): error[missing-argument] No argument provided for required parameter 2

This is interesting, it seems that a decorator that returns a callable on a method is removing the fact that the original function is an instance method:

```py
    def __rmatmul__(self, other: MatrixBase) -> MatrixBase:
        self, other, T = _unify_with_other(self, other)

        if T != "is_matrix":
            return NotImplemented

		# This is the revealed type with the `call_highest_priority` decorator
		# ty: Revealed type: `Unknown | ((Unknown, Unknown, /) -> Unknown)` [revealed-type]
		# Pyright: Type of "self.__rmul__" is "(MatrixBase | Expr | complex) -> MatrixBase"
        return reveal_type(self.__rmul__)(other)

		# And, this is without the decorator
		# ty: Revealed type: `Unknown | (bound method MatrixBase.__rmul__(other: MatrixBase | Unknown) -> MatrixBase)` [revealed-type]


    @call_highest_priority('__mul__')
    def __rmul__(self, other: MatrixBase | SExpr) -> MatrixBase:
        return self.rmultiply(other)
```

This wasn't an easy before this PR because the call to an overloaded function `_unify_with_other` wasn't working correctly and the type of `self`, `other` and `T` was `Unknown`.

---

_Marked ready for review by @dhruvmanila on 2025-08-19 10:01_

---

_Review requested from @carljm by @dhruvmanila on 2025-08-19 10:01_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-08-19 10:01_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-08-19 10:01_

---

_Review requested from @dcreager by @dhruvmanila on 2025-08-19 10:01_

---

_@carljm approved on 2025-08-19 22:32_

Nice!

---

_Merged by @dhruvmanila on 2025-08-20 04:09_

---

_Closed by @dhruvmanila on 2025-08-20 04:09_

---

_Branch deleted on 2025-08-20 04:09_

---
