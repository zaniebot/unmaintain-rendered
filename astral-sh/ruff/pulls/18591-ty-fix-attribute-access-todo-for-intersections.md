```yaml
number: 18591
title: "[ty] Fix attribute access TODO for intersections with negative contributions"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/intersection-todo
created_at: 2025-06-09T13:07:10Z
updated_at: 2025-06-17T21:48:54Z
url: https://github.com/astral-sh/ruff/pull/18591
synced_at: 2026-01-10T18:39:08Z
```

# [ty] Fix attribute access TODO for intersections with negative contributions

---

_Pull request opened by @AlexWaygood on 2025-06-09 13:07_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-06-09 13:09_

---

_Comment by @github-actions[bot] on 2025-06-09 13:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[invalid-return-type] mypy_primer/utils.py:114:12: Return type does not match returned value: expected `tuple[CompletedProcess[str], int | float]`, found `tuple[CompletedProcess[@Todo(map_with_boundness: intersections with negative contributions) | None], int | float]`
+ error[invalid-return-type] mypy_primer/utils.py:114:12: Return type does not match returned value: expected `tuple[CompletedProcess[str], int | float]`, found `tuple[CompletedProcess[@Todo(generic `typing.Awaitable` type) | None], int | float]`

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ error[unresolved-attribute] pytest_robotframework/__init__.py:230:63: Type `((...) -> Unknown) & ~_KeywordDecorator` has no attribute `__name__`
- Found 186 diagnostics
+ Found 187 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
+ warning[possibly-unbound-attribute] src/pegen/python_generator.py:115:20: Attribute `lower` on type `(Unknown & ~Literal["SOFT_KEYWORD"]) | (str & ~Literal["SOFT_KEYWORD"])` is possibly unbound
+ warning[possibly-unbound-attribute] src/pegen/python_generator.py:119:26: Attribute `lower` on type `(Unknown & ~Literal["SOFT_KEYWORD"]) | (str & ~Literal["SOFT_KEYWORD"])` is possibly unbound
- Found 53 diagnostics
+ Found 55 diagnostics

parso (https://github.com/davidhalter/parso)
- warning[possibly-unbound-attribute] parso/python/parser.py:120:50: Attribute `value` on type `@Todo(map_with_boundness: intersections with negative contributions) | None` is possibly unbound
+ warning[possibly-unbound-attribute] parso/python/parser.py:120:50: Attribute `value` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] parso/python/parser.py:121:26: Attribute `value` on type `@Todo(map_with_boundness: intersections with negative contributions) | None` is possibly unbound
+ warning[possibly-unbound-attribute] parso/python/parser.py:121:26: Attribute `value` on type `Unknown | None` is possibly unbound

beartype (https://github.com/beartype/beartype)
- warning[unused-ignore-comment] beartype/_util/kind/map/utilmapset.py:141:35: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] beartype/bite/collection/infercollectionitems.py:321:51: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] beartype/bite/collection/infercollectionitems.py:504:43: Unused blanket `type: ignore` directive
- Found 570 diagnostics
+ Found 567 diagnostics

python-chess (https://github.com/niklasf/python-chess)
+ error[non-subscriptable] chess/__init__.py:694:39: Cannot subscript object of type `str & ~Literal["0000"]` with no `__getitem__` method
+ error[non-subscriptable] chess/__init__.py:696:44: Cannot subscript object of type `str & ~Literal["0000"]` with no `__getitem__` method
+ error[non-subscriptable] chess/__init__.py:697:45: Cannot subscript object of type `str & ~Literal["0000"]` with no `__getitem__` method
+ error[non-subscriptable] chess/__init__.py:703:50: Cannot subscript object of type `str & ~Literal["0000"]` with no `__getitem__` method
+ error[non-subscriptable] chess/__init__.py:704:48: Cannot subscript object of type `str & ~Literal["0000"]` with no `__getitem__` method
+ error[non-subscriptable] chess/__init__.py:705:49: Cannot subscript object of type `str & ~Literal["0000"]` with no `__getitem__` method
+ error[unresolved-attribute] chess/__init__.py:1152:22: Type `str & ~Literal["~"]` has no attribute `lower`
+ error[not-iterable] chess/__init__.py:2689:21: Object of type `str & ~AlwaysFalsy & ~Literal["-"]` is not iterable
+ error[non-subscriptable] chess/__init__.py:2858:16: Cannot subscript object of type `str & ~AlwaysFalsy & ~Literal["-"]` with no `__getitem__` method
+ error[invalid-argument-type] chess/__init__.py:3051:36: Argument to function `piece_symbol` is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None`
+ error[invalid-argument-type] chess/__init__.py:3114:39: Argument to function `piece_symbol` is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None`
+ error[non-subscriptable] chess/engine.py:2550:18: Cannot subscript object of type `str & ~Literal["///"]` with no `__getitem__` method
+ error[non-subscriptable] chess/engine.py:2551:27: Cannot subscript object of type `str & ~Literal["///"]` with no `__getitem__` method
+ error[non-subscriptable] chess/engine.py:2552:28: Cannot subscript object of type `str & ~Literal["///"]` with no `__getitem__` method
+ warning[possibly-unbound-implicit-call] chess/pgn.py:1776:39: Method `__getitem__` of type `(Unknown & ~Literal["("] & ~Literal[")"]) | (str & ~Literal["("] & ~Literal[")"])` is possibly unbound
- Found 36 diagnostics
+ Found 51 diagnostics

nionutils (https://github.com/nion-software/nionutils)
+ warning[possibly-unbound-attribute] nion/utils/DateTime.py:30:49: Attribute `utcoffset` on type `@Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- Found 17 diagnostics
+ Found 18 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
+ error[unresolved-attribute] pyp.py:272:21: Type `Unknown & Expr & ~FunctionDef & ~AsyncFunctionDef & ~ClassDef & ~Import & ~ImportFrom & ~Assign & ~AnnAssign & ~If & ~Try` has no attribute `value`
+ error[unresolved-attribute] pyp.py:429:34: Type `stmt` has no attribute `body`
+ error[unresolved-attribute] pyp.py:432:27: Type `stmt` has no attribute `value`
+ error[unresolved-attribute] pyp.py:433:26: Type `stmt` has no attribute `value`
+ error[unresolved-attribute] pyp.py:439:75: Type `stmt` has no attribute `value`
- Found 10 diagnostics
+ Found 15 diagnostics

git-revise (https://github.com/mystor/git-revise)
+ error[unsupported-operator] gitrevise/utils.py:116:9: Operator `+=` is unsupported between objects of type `bytes & ~Literal[b""]` and `Literal[b"\n"]`
- Found 10 diagnostics
+ Found 11 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[unresolved-attribute] pyinstrument/renderers/speedscope.py:103:25: Type `Any & SpeedscopeProfile & ~SpeedscopeFile` has no attribute `name`
+ error[unsupported-operator] pyinstrument/vendor/decorator.py:258:13: Operator `+=` is unsupported between objects of type `LiteralString & ~Literal[""]` and `Literal[","]`
- Found 87 diagnostics
+ Found 89 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
+ error[unresolved-attribute] koda_validate/serialization/errors.py:64:49: Type `(Predicate[Any] & MaxKeys & ~MinKeys) | (PredicateAsync[Any] & MaxKeys & ~MinKeys)` has no attribute `size`
+ error[unresolved-attribute] koda_validate/serialization/errors.py:80:45: Type `(Predicate[Any] & MaxItems[Unknown] & ~MinKeys & ~MaxKeys & ~Choices[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~MultipleOf[Unknown] & ~EqualTo[Unknown] & ~MinItems[Unknown]) | (PredicateAsync[Any] & MaxItems[Unknown] & ~MinKeys & ~MaxKeys & ~Choices[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~MultipleOf[Unknown] & ~EqualTo[Unknown] & ~MinItems[Unknown])` has no attribute `item_count`
+ error[unresolved-attribute] koda_validate/serialization/errors.py:82:34: Type `(Predicate[Any] & ExactItemCount[Unknown] & ~MinKeys & ~MaxKeys & ~Choices[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~MultipleOf[Unknown] & ~EqualTo[Unknown] & ~MinItems[Unknown] & ~MaxItems[Unknown]) | (PredicateAsync[Any] & ExactItemCount[Unknown] & ~MinKeys & ~MaxKeys & ~Choices[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~MultipleOf[Unknown] & ~EqualTo[Unknown] & ~MinItems[Unknown] & ~MaxItems[Unknown])` has no attribute `item_count`
+ error[unresolved-attribute] koda_validate/serialization/errors.py:94:45: Type `(Predicate[Any] & MaxLength[Unknown] & ~MinKeys & ~MaxKeys & ~Choices[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~MultipleOf[Unknown] & ~EqualTo[Unknown] & ~MinItems[Unknown] & ~MaxItems[Unknown] & ~ExactItemCount[Unknown] & ~UniqueItems[Unknown] & ~RegexPredicate & ~EmailPredicate & ~NotBlank[Unknown] & ~MinLength[Unknown]) | (PredicateAsync[Any] & MaxLength[Unknown] & ~MinKeys & ~MaxKeys & ~Choices[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~MultipleOf[Unknown] & ~EqualTo[Unknown] & ~MinItems[Unknown] & ~MaxItems[Unknown] & ~ExactItemCount[Unknown] & ~UniqueItems[Unknown] & ~RegexPredicate & ~EmailPredicate & ~NotBlank[Unknown] & ~MinLength[Unknown])` has no attribute `length`
+ error[unresolved-attribute] koda_validate/serialization/errors.py:96:38: Type `(Predicate[Any] & ExactLength[Unknown] & ~MinKeys & ~MaxKeys & ~Choices[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~MultipleOf[Unknown] & ~EqualTo[Unknown] & ~MinItems[Unknown] & ~MaxItems[Unknown] & ~ExactItemCount[Unknown] & ~UniqueItems[Unknown] & ~RegexPredicate & ~EmailPredicate & ~NotBlank[Unknown] & ~MinLength[Unknown] & ~MaxLength[Unknown]) | (PredicateAsync[Any] & ExactLength[Unknown] & ~MinKeys & ~MaxKeys & ~Choices[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~MultipleOf[Unknown] & ~EqualTo[Unknown] & ~MinItems[Unknown] & ~MaxItems[Unknown] & ~ExactItemCount[Unknown] & ~UniqueItems[Unknown] & ~RegexPredicate & ~EmailPredicate & ~NotBlank[Unknown] & ~MinLength[Unknown] & ~MaxLength[Unknown])` has no attribute `length`
+ error[unresolved-attribute] koda_validate/serialization/json_schema.py:309:30: Type `(Predicate[Any] & MinLength[Unknown] & ~EmailPredicate & ~MaxLength[Unknown]) | (PredicateAsync[Any] & MinLength[Unknown] & ~EmailPredicate & ~MaxLength[Unknown])` has no attribute `length`
+ error[unresolved-attribute] koda_validate/serialization/json_schema.py:311:30: Type `(Predicate[Any] & ExactLength[Unknown] & ~EmailPredicate & ~MaxLength[Unknown] & ~MinLength[Unknown]) | (PredicateAsync[Any] & ExactLength[Unknown] & ~EmailPredicate & ~MaxLength[Unknown] & ~MinLength[Unknown])` has no attribute `length`
+ error[unresolved-attribute] koda_validate/serialization/json_schema.py:311:56: Type `(Predicate[Any] & ExactLength[Unknown] & ~EmailPredicate & ~MaxLength[Unknown] & ~MinLength[Unknown]) | (PredicateAsync[Any] & ExactLength[Unknown] & ~EmailPredicate & ~MaxLength[Unknown] & ~MinLength[Unknown])` has no attribute `length`
+ error[unresolved-attribute] koda_validate/serialization/json_schema.py:317:28: Type `(Predicate[Any] & RegexPredicate & ~EmailPredicate & ~MaxLength[Unknown] & ~MinLength[Unknown] & ~ExactLength[Unknown] & ~Choices[Unknown] & ~NotBlank[Unknown]) | (PredicateAsync[Any] & RegexPredicate & ~EmailPredicate & ~MaxLength[Unknown] & ~MinLength[Unknown] & ~ExactLength[Unknown] & ~Choices[Unknown] & ~NotBlank[Unknown])` has no attribute `pattern`
+ error[unresolved-attribute] koda_validate/serialization/json_schema.py:395:34: Type `(Predicate[Any] & MaxKeys & ~EmailPredicate & ~MaxLength[Unknown] & ~MinLength[Unknown] & ~ExactLength[Unknown] & ~Choices[Unknown] & ~NotBlank[Unknown] & ~RegexPredicate & ~StartsWith[Unknown] & ~EndsWith[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~EqualTo[Unknown] & ~MinKeys) | (PredicateAsync[Any] & MaxKeys & ~EmailPredicate & ~MaxLength[Unknown] & ~MinLength[Unknown] & ~ExactLength[Unknown] & ~Choices[Unknown] & ~NotBlank[Unknown] & ~RegexPredicate & ~StartsWith[Unknown] & ~EndsWith[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~EqualTo[Unknown] & ~MinKeys)` has no attribute `size`
+ error[unresolved-attribute] koda_validate/serialization/json_schema.py:400:29: Type `(Predicate[Any] & MaxItems[Unknown] & ~EmailPredicate & ~MaxLength[Unknown] & ~MinLength[Unknown] & ~ExactLength[Unknown] & ~Choices[Unknown] & ~NotBlank[Unknown] & ~RegexPredicate & ~StartsWith[Unknown] & ~EndsWith[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~EqualTo[Unknown] & ~MinKeys & ~MaxKeys & ~MinItems[Unknown]) | (PredicateAsync[Any] & MaxItems[Unknown] & ~EmailPredicate & ~MaxLength[Unknown] & ~MinLength[Unknown] & ~ExactLength[Unknown] & ~Choices[Unknown] & ~NotBlank[Unknown] & ~RegexPredicate & ~StartsWith[Unknown] & ~EndsWith[Unknown] & ~Min[Unknown] & ~Max[Unknown] & ~EqualTo[Unknown] & ~MinKeys & ~MaxKeys & ~MinItems[Unknown])` has no attribute `item_count`
- Found 45 diagnostics
+ Found 56 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
+ error[unsupported-operator] more_itertools/recipes.py:1217:13: Operator `*` is unsupported between objects of type `int & ~Literal[1]` and `int & ~Literal[1]`
- Found 51 diagnostics
+ Found 52 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ warning[possibly-unbound-attribute] aioredis/client.py:4712:26: Attribute `evalsha` on type `(Redis & ~Pipeline) | (Unknown & ~Pipeline)` is possibly unbound
+ warning[possibly-unbound-attribute] aioredis/client.py:4717:30: Attribute `script_load` on type `(Redis & ~Pipeline) | (Unknown & ~Pipeline)` is possibly unbound
+ warning[possibly-unbound-attribute] aioredis/client.py:4718:26: Attribute `evalsha` on type `(Redis & ~Pipeline) | (Unknown & ~Pipeline)` is possibly unbound
+ error[unresolved-attribute] aioredis/connection.py:1161:35: Type `Unknown & str & ~Literal[""]` has no attribute `upper`
- Found 25 diagnostics
+ Found 29 diagnostics

anyio (https://github.com/agronholm/anyio)
+ error[unresolved-attribute] src/anyio/_core/_sockets.py:212:46: Type `IPv4Address & ~IPv6Address` has no attribute `compressed`
+ error[unresolved-attribute] src/anyio/streams/tls.py:199:21: Type `SSLError & ~SSLEOFError` has no attribute `strerror`
+ error[unresolved-attribute] src/anyio/streams/tls.py:199:72: Type `SSLError & ~SSLEOFError` has no attribute `strerror`
+ warning[possibly-unbound-attribute] src/anyio/to_process.py:233:29: Attribute `exec_module` on type `Loader | None` is possibly unbound
- Found 109 diagnostics
+ Found 113 diagnostics

kornia (https://github.com/kornia/kornia)
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:63: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:63: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[Unknown, Unknown]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:69: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:69: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[Unknown, Unknown]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:77: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:77: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[Unknown, Unknown]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:84: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:84: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[Unknown, Unknown]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:120:61: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:120:61: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[Unknown, Unknown]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:120:67: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:120:67: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[Unknown, Unknown]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:227:67: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:227:67: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[Unknown, Unknown]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:227:73: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:227:73: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[Unknown, Unknown]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:56: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:56: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[Unknown, Unknown]`
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:64: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[invalid-argument-type] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:64: Argument to bound method `__init__` is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[Unknown, Unknown]`
+ warning[possibly-unbound-attribute] kornia/augmentation/container/augment.py:655:25: Attribute `dtype` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/augmentation/container/augment.py:656:23: Attribute `float` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ error[unsupported-operator] kornia/contrib/models/efficient_vit/utils/network.py:33:12: Operator `//` is unsupported between objects of type `(int & ~tuple[Unknown, ...]) | (tuple[int, ...] & ~tuple[Unknown, ...])` and `Literal[2]`
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:49:18: Attribute `to` on type `(int & ~float) | (Unknown & ~float)` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:52:15: Attribute `shape` on type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/enhance/adjust.py:53:18: Method `__getitem__` of type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:107:18: Attribute `to` on type `(int & ~float) | (Unknown & ~float)` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:110:15: Attribute `shape` on type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/enhance/adjust.py:111:18: Method `__getitem__` of type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:180:14: Attribute `to` on type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:290:13: Attribute `to` on type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:291:12: Attribute `to` on type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:364:18: Attribute `to` on type `(int & ~float) | (Unknown & ~float)` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:367:15: Attribute `shape` on type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/enhance/adjust.py:368:18: Method `__getitem__` of type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:419:18: Attribute `to` on type `(int & ~float) | (Unknown & ~float)` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:422:15: Attribute `shape` on type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/enhance/adjust.py:423:18: Method `__getitem__` of type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:492:18: Attribute `to` on type `(int & ~float) | (Unknown & ~float)` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:495:15: Attribute `shape` on type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/enhance/adjust.py:496:18: Method `__getitem__` of type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:545:18: Attribute `to` on type `(int & ~float) | (Unknown & ~float)` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:548:15: Attribute `shape` on type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-implicit-call] kornia/enhance/adjust.py:549:18: Method `__getitem__` of type `(int & ~float) | (Unknown & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:721:50: Attribute `shape` on type `(int & ~float) | (Unknown & ~None & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:722:61: Attribute `shape` on type `(int & ~float) | (Unknown & ~None & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/adjust.py:725:25: Attribute `to` on type `(int & ~float) | (Unknown & ~None & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:249:12: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:249:27: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:250:16: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:250:52: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:251:90: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:254:12: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:254:26: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:255:16: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:255:51: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/normalize.py:256:89: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/enhance/rescale.py:37:50: Attribute `ndim` on type `(int & ~float) | (Unknown & ~float)` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/gaussian.py:77:17: Attribute `to` on type `(tuple[int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:106:18: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:110:40: Attribute `device` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:110:60: Attribute `dtype` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:115:43: Attribute `device` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:115:63: Attribute `dtype` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:120:42: Attribute `pow` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:146:18: Attribute `shape` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:148:43: Attribute `device` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:148:63: Attribute `dtype` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:150:22: Attribute `abs` on type `(Unknown & ~float) | (int & ~float) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/filters/kernels.py:276:55: Attribute `exp` on type `@Todo(Type::Intersection.call()) | int` is possibly unbound
+ error[call-non-callable] kornia/filters/kernels.py:685:24: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:685:24: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:685:24: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:685:43: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:685:43: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:685:43: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:33: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:33: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:33: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:33: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:52: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:52: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:52: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:52: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:71: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:71: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:71: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ error[call-non-callable] kornia/filters/kernels.py:744:71: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (Unknown & ~(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]])) | Unknown` is not callable on object of type `(tuple[int | float, int | float, int | float] & ~tuple[Unknown, ...]) | (Unknown & ~tuple[Unknown, ...]) | Unknown`
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:223:16: Attribute `is_floating_point` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:225:80: Attribute `dtype` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:227:21: Attribute `float` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:229:16: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:230:21: Attribute `reshape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:232:22: Attribute `ndim` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:232:42: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:233:84: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:235:37: Attribute `ndim` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:707:43: Attribute `dim` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:707:63: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:710:33: Attribute `size` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:713:13: Attribute `view` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:713:24: Attribute `size` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:713:40: Attribute `size` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:713:59: Attribute `size` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/boxes.py:713:74: Attribute `size` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:51:16: Attribute `is_floating_point` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:53:80: Attribute `dtype` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:55:25: Attribute `float` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:57:16: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:60:25: Attribute `reshape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:62:22: Attribute `ndim` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:62:46: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:63:82: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:65:37: Attribute `ndim` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:203:43: Attribute `dim` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:203:63: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:206:33: Attribute `size` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:210:19: Attribute `view` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:210:30: Attribute `size` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:210:46: Attribute `size` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:210:65: Attribute `size` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:251:16: Attribute `is_floating_point` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:253:80: Attribute `dtype` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:255:25: Attribute `float` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:257:16: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:260:25: Attribute `reshape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:262:22: Attribute `ndim` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:262:46: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:263:82: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/geometry/keypoints.py:265:37: Attribute `ndim` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown]) | Unknown` is possibly unbound
+ error[not-iterable] kornia/geometry/transform/affwarp.py:705:36: Object of type `int & ~float` is not iterable
+ warning[possibly-unbound-attribute] kornia/models/depth_estimation/base.py:59:35: Attribute `cpu` on type `(Unknown & ~list[Unknown] & ~tuple[Unknown, ...]) | (list[Unknown] & ~list[Unknown] & ~tuple[Unknown, ...])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/models/depth_estimation/base.py:60:40: Attribute `device` on type `(Unknown & ~list[Unknown] & ~tuple[Unknown, ...]) | (list[Unknown] & ~list[Unknown] & ~tuple[Unknown, ...])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/models/depth_estimation/base.py:60:61: Attribute `dtype` on type `(Unknown & ~list[Unknown] & ~tuple[Unknown, ...]) | (list[Unknown] & ~list[Unknown] & ~tuple[Unknown, ...])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/models/segmentation/base.py:184:40: Attribute `size` on type `(Unknown & ~None & ~list[Unknown] & ~tuple[Unknown, ...]) | (list[Unknown] & ~list[Unknown] & ~tuple[Unknown, ...]) | (Unknown & ~list[Unknown] & ~tuple[Unknown, ...])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/models/utils.py:61:58: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
+ warning[possibly-unbound-attribute] kornia/models/utils.py:95:58: Attribute `shape` on type `(Unknown & ~list[Unknown]) | (list[Unknown] & ~list[Unknown])` is possibly unbound
- Found 949 diagnostics
+ Found 1066 diagnostics

starlette (https://github.com/encode/starlette)
+ error[unresolved-attribute] starlette/routing.py:753:16: Type `str & ~Literal["/"]` has no attribute `endswith`
+ error[invalid-argument-type] starlette/staticfiles.py:76:45: Argument to function `find_spec` is incorrect: Expected `str`, found `(str & ~tuple[Unknown, ...]) | (tuple[str, str] & ~tuple[Unknown, ...])`
- Found 175 diagnostics
+ Found 177 diagnostics

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
+ error[unresolved-attribute] mypy_protobuf/main.py:183:15: Type `str & ~Literal["typing_extensions"]` has no attribute `replace`
+ error[unresolved-attribute] mypy_protobuf/main.py:248:72: Type `str & ~Literal[""]` has no attribute `rstrip`
- Found 46 diagnostics
+ Found 48 diagnostics

aiortc (https://github.com/aiortc/aiortc)
+ error[unresolved-attribute] src/aiortc/rtcsctptransport.py:1268:35: Type `StreamAddOutgoingParam & ~StreamResetOutgoingParam` has no attribute `request_sequence`
+ error[unresolved-attribute] src/aiortc/rtcsctptransport.py:1270:43: Type `StreamAddOutgoingParam & ~StreamResetOutgoingParam` has no attribute `request_sequence`
+ error[unresolved-attribute] src/aiortc/rtcsctptransport.py:1276:21: Type `StreamResetResponseParam & ~StreamResetOutgoingParam & ~StreamAddOutgoingParam` has no attribute `response_sequence`
+ error[unresolved-attribute] src/aiortc/rtcsctptransport.py:1814:47: Type `str & ~Literal[""]` has no attribute `encode`
+ error[unresolved-attribute] src/aiortc/rtcsctptransport.py:1814:47: Type `str & ~Literal[""]` has no attribute `encode`
+ error[unresolved-attribute] src/aiortc/rtcsctptransport.py:1814:47: Type `str & ~Literal[""]` has no attribute `encode`
- Found 128 diagnostics
+ Found 134 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ error[non-subscriptable] dulwich/ignore.py:119:13: Cannot subscript object of type `bytes & ~Literal[b"*"]` with no `__getitem__` method
+ error[non-subscriptable] dulwich/ignore.py:127:34: Cannot subscript object of type `bytes & ~Literal[b"*"]` with no `__getitem__` method
+ error[non-subscriptable] dulwich/ignore.py:133:26: Cannot subscript object of type `bytes & ~Literal[b"*"]` with no `__getitem__` method
+ error[non-subscriptable] dulwich/ignore.py:135:26: Cannot subscript object of type `bytes & ~Literal[b"*"]` with no `__getitem__` method
+ error[non-subscriptable] dulwich/ignore.py:137:29: Cannot subscript object of type `bytes & ~Literal[b"*"]` with no `__getitem__` method
+ error[non-subscriptable] dulwich/ignore.py:142:25: Cannot subscript object of type `bytes & ~Literal[b"*"]` with no `__getitem__` method
+ error[unresolved-attribute] dulwich/ignore.py:228:20: Type `bytes & ~Literal[b"**"]` has no attribute `split`
+ error[unresolved-attribute] dulwich/objects.py:709:22: Type `bytes & ~Literal[b"\n"]` has no attribute `split`
+ error[unresolved-attribute] dulwich/objects.py:709:22: Type `bytes & ~Literal[b"\n"]` has no attribute `split`
+ error[unresolved-attribute] dulwich/objects.py:709:22: Type `bytes & ~Literal[b"\n"]` has no attribute `split`
+ error[unresolved-attribute] dulwich/pack.py:1928:31: Type `int` has no attribute `type_num`
- Found 141 diagnostics
+ Found 152 diagnostics

operator (https://github.com/canonical/operator)
+ error[unresolved-attribute] ops/model.py:3971:39: Type `str & ~Literal["icmp"]` has no attribute `partition`
+ error[unresolved-attribute] ops/model.py:3971:39: Type `str & ~Literal["icmp"]` has no attribute `partition`
+ error[unresolved-attribute] ops/model.py:3971:39: Type `str & ~Literal["icmp"]` has no attribute `partition`
+ error[unresolved-attribute] ops/model.py:3971:39: Type `str & ~Literal["icmp"]` has no attribute `partition`
- Found 111 diagnostics
+ Found 115 diagnostics

kopf (https://github.com/nolar/kopf)
- warning[possibly-unbound-attribute] kopf/_cogs/structs/dicts.py:241:25: Attribute `obj` on type `(_T & ~None) | Iterable[_T]` is possibly unbound
+ error[unresolved-attribute] kopf/_cogs/structs/dicts.py:241:25: Type `(_T & ~None) | Iterable[_T]` has no attribute `obj`
+ error[not-iterable] kopf/_cogs/structs/dicts.py:257:20: Object of type `(_T & Iterable[Unknown] & ~Mapping[Unknown, Unknown] & ~KubernetesModel) | (Iterable[_T] & ~KubernetesModel & ~Mapping[Unknown, Unknown])` may not be iterable
+ error[unresolved-attribute] kopf/_core/intents/registries.py:502:29: Type `ResourceCause & ~ChangingCause` has no attribute `body`
+ error[not-iterable] kopf/_core/reactor/subhandling.py:84:19: Object of type `Iterable[Unknown] & ~Mapping[Unknown, Unknown]` is not iterable
- Found 157 diagnostics
+ Found 160 diagnostics

trio (https://github.com/python-trio/trio)
- warning[unused-ignore-comment] src/trio/_core/_run.py:1972:66: Unused blanket `type: ignore` directive
- Found 1103 diagnostics
+ Found 1102 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- warning[unused-ignore-comment] src/graphql/error/graphql_error.py:147:72: Unused blanket `type: ignore` directive
+ error[unresolved-attribute] src/graphql/execution/collect_fields.py:261:42: Type `InlineFragmentNode & ~FieldNode` has no attribute `selection_set`
+ error[unresolved-attribute] src/graphql/execution/collect_fields.py:263:25: Type `FragmentSpreadNode & ~FieldNode & ~InlineFragmentNode` has no attribute `name`
+ error[unresolved-attribute] src/graphql/execution/execute.py:273:27: Type `FragmentDefinitionNode & ~OperationDefinitionNode` has no attribute `name`
- warning[unused-ignore-comment] src/graphql/execution/execute.py:1659:56: Unused blanket `type: ignore` directive
+ warning[possibly-unbound-attribute] src/graphql/execution/execute.py:2187:8: Attribute `is_awaitable` on type `(list[GraphQLError] & ~list[Unknown]) | (ExecutionContext & ~list[Unknown])` is possibly unbound
- warning[unused-ignore-comment] src/graphql/execution/execute.py:2200:62: Unused blanket `type: ignore` directive
+ warning[possibly-unbound-attribute] src/graphql/execution/execute.py:2193:20: Attribute `map_source_to_response` on type `Unknown | (list[GraphQLError] & ~list[Unknown]) | (ExecutionContext & ~list[Unknown])` is possibly unbound
- error[invalid-argument-type] src/graphql/execution/execute.py:2258:44: Argument to function `create_source_event_stream_impl` is incorrect: Expected `ExecutionContext`, found `(@Todo(map_with_boundness: intersections with negative contributions) & ~list[Unknown]) | (list[GraphQLError] & ~list[Unknown]) | (ExecutionContext & ~list[Unknown])`
+ error[invalid-argument-type] src/graphql/execution/execute.py:2258:44: Argument to function `create_source_event_stream_impl` is incorrect: Expected `ExecutionContext`, found `(list[GraphQLError] & ~list[Unknown]) | (ExecutionContext & ~list[Unknown])`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:114:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `id`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:115:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `path`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:116:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `label`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:171:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `id`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:172:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `errors`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:250:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `data`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:251:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `errors`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:252:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `extensions`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:344:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `data`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:345:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `errors`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:346:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `pending`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:347:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `has_next`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:348:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `extensions`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:443:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `data`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:444:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `id`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:445:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `sub_path`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:446:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `errors`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:447:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `extensions`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:535:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `items`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:536:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `id`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:537:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `sub_path`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:538:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `errors`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:539:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `extensions`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:642:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `has_next`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:643:33: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `pending`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:644:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `incremental`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:645:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `completed`
+ error[unresolved-attribute] src/graphql/execution/incremental_publisher.py:646:17: Type `~dict[Unknown, Unknown] & ~tuple[Unknown, ...]` has no attribute `extensions`
+ error[unsupported-operator] src/graphql/language/printer.py:93:46: Operator `+` is unsupported between objects of type `str & ~Literal["query"]` and `Literal[" "]`
+ error[unresolved-attribute] src/graphql/pyutils/cached_property.py:34:21: Type `~None` has no attribute `__dict__`
+ error[unresolved-attribute] src/graphql/type/definition.py:1712:30: Type `GraphQLType & ~AlwaysFalsy` has no attribute `of_type`
+ error[not-iterable] src/graphql/type/validate.py:616:26: Object of type `(InputValueDefinitionNode & ~AlwaysTruthy & ~AlwaysFalsy) | (tuple[ConstDirectiveNode, ...] & ~AlwaysFalsy)` may not be iterable
+ error[unresolved-attribute] src/graphql/utilities/extend_schema.py:222:48: Type `TypeExtensionNode & ~SchemaDefinitionNode & ~SchemaExtensionNode & ~DirectiveDefinitionNode & ~TypeDefinitionNode` has no attribute `name`
+ warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:258:13: Attribute `value` on type `StringValueNode | None` is possibly unbound
+ error[unresolved-attribute] src/graphql/utilities/extend_schema.py:554:67: Type `NonNullTypeNode & ~ListTypeNode` has no attribute `type`
+ warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:589:33: Attribute `value` on type `StringValueNode | None` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:609:29: Attribute `value` on type `StringValueNode | None` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:630:33: Attribute `value` on type `StringValueNode | None` is possibly unbound
+ warning[possibly-unbound-attribute] src/graphql/utilities/extend_schema.py:651:33: Attribute `value` on type `StringValueNode | None` is possibly unbound
+ error[unresolved-attribute] src/graphql/utilities/separate_operations.py:46:23: Type `FragmentDefinitionNode & ~OperationDefinitionNode` has no attribute `name`
+ error[unresolved-attribute] src/graphql/utilities/separate_operations.py:47:17: Type `FragmentDefinitionNode & ~OperationDefinitionNode` has no attribute `selection_set`
+ error[unresolved-attribute] src/graphql/utilities/type_from_ast.py:60:44: Type `NonNullTypeNode & ~ListTypeNode` has no attribute `type`
+ error[unresolved-attribute] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:745:28: Type `FragmentSpreadNode & ~FieldNode` has no attribute `name`
+ error[unresolved-attribute] src/graphql/validation/rules/overlapping_fields_can_be_merged.py:756:17: Type `InlineFragmentNode & ~FieldNode & ~FragmentSpreadNode` has no attribute `selection_set`
+ error[unresolved-attribute] src/graphql/validation/rules/unique_directives_per_location.py:68:25: Type `Node & ~SchemaDefinitionNode & ~SchemaExtensionNode` has no attribute `name`
- warning[unused-ignore-comment] tests/language/test_parser.py:658:53: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/language/test_parser.py:662:51: Unused blanket `type: ignore` directive
+ warning[possibly-unbound-attribute] tests/language/test_parser.py:656:16: Attribute `kind` on type `(Location & ~AlwaysTruthy & ~AlwaysFalsy) | (Token & ~AlwaysFalsy)` is possibly unbound
+ warning[possibly-unbound-attribute] tests/language/test_parser.py:657:16: Attribute `value` on type `(Location & ~AlwaysTruthy & ~AlwaysFalsy) | (Token & ~AlwaysFalsy)` is possibly unbound
- warning[unused-ignore-comment] tests/pyutils/test_ref_map.py:35:23: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/pyutils/test_ref_map.py:52:19: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/pyutils/test_ref_map.py:72:23: Unused blanket `type: ignore` directive
- Found 441 diagnostics
+ Found 485 diagnostics

pybind11 (https://github.com/pybind/pybind11)
+ error[unsupported-operator] pybind11/setup_helpers.py:319:38: Operator `+` is unsupported between objects of type `str` and `bytes`
- Found 228 diagnostics
+ Found 229 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ error[unresolved-attribute] strawberry/channels/handlers/http_handler.py:216:21: Type `@Todo(generic `typing.Awaitable` type) & ChannelsResponse & ~MultipartChannelsResponse` has no attribute `status`
+ error[unresolved-attribute] strawberry/channels/handlers/http_handler.py:218:29: Type `@Todo(generic `typing.Awaitable` type) & ChannelsResponse & ~MultipartChannelsResponse` has no attribute `headers`
+ error[unresolved-attribute] strawberry/codegen/plugins/print_operation.py:119:24: Type `Unknown & GraphQLVariableReference & ~GraphQLStringValue & ~GraphQLIntValue` has no attribute `value`
+ error[unresolved-attribute] strawberry/codegen/plugins/print_operation.py:128:24: Type `Unknown & GraphQLBoolValue & ~GraphQLStringValue & ~GraphQLIntValue & ~GraphQLVariableReference & ~GraphQLListValue & ~GraphQLEnumValue` has no attribute `value`
+ error[unresolved-attribute] strawberry/codegen/plugins/python.py:95:20: Type `(Unknown & GraphQLObjectType & ~GraphQLOptional & ~GraphQLList & ~GraphQLUnion) | (Unknown & GraphQLEnum & ~GraphQLOptional & ~GraphQLList & ~GraphQLUnion)` has no attribute `name`
+ error[unresolved-attribute] strawberry/codegen/plugins/python.py:99:17: Type `Unknown & GraphQLScalar & ~GraphQLOptional & ~GraphQLList & ~GraphQLUnion & ~GraphQLObjectType & ~GraphQLEnum` has no attribute `name`
+ error[unresolved-attribute] strawberry/codegen/plugins/python.py:101:56: Type `Unknown & GraphQLScalar & ~GraphQLOptional & ~GraphQLList & ~GraphQLUnion & ~GraphQLObjectType & ~GraphQLEnum` has no attribute `name`
+ error[unresolved-attribute] strawberry/codegen/plugins/typescript.py:65:20: Type `(Unknown & GraphQLObjectType & ~GraphQLOptional & ~Gr...*[Comment body truncated]*

---

_Comment by @AlexWaygood on 2025-06-09 13:13_

Evidently not quite right yet

---

_Closed by @AlexWaygood on 2025-06-09 13:13_

---

_Branch deleted on 2025-06-17 21:48_

---
