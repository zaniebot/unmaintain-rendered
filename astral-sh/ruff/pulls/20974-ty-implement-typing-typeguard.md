```yaml
number: 20974
title: "[ty] implement `typing.TypeGuard`"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: typeguard
created_at: 2025-10-19T23:00:18Z
updated_at: 2026-01-05T02:10:51Z
url: https://github.com/astral-sh/ruff/pull/20974
synced_at: 2026-01-12T15:57:13Z
```

# [ty] implement `typing.TypeGuard`

---

_@ericmarkmartin_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Resolve(s) astral-sh/ty#117, astral-sh/ty#1569

Implement `typing.TypeGuard`. Due to the fact that it [overrides anything previously known about the checked value](https://typing.python.org/en/latest/spec/narrowing.html#typeguard)---

> When a conditional statement includes a call to a user-defined type guard function, and that function returns true, the expression passed as the first positional argument to the type guard function should be assumed by a static type checker to take on the type specified in the TypeGuard return type, unless and until it is further narrowed within the conditional code block.

---we have to substantially rework the constraints system. In particular, we make constraints represented as a disjunctive normal form (DNF) where each term includes a regular constraint, and one or more disjuncts with a typeguard constraint. Some test cases (including some with more complex boolean logic) are added to `type_guards.md`.


## Test Plan

- update existing tests
- add new tests for more complex boolean logic with `TypeGuard`
- add new tests for `TypeGuard` variance


---

_Renamed from "implement `typing.TypeGuard`" to "[ty] implement `typing.TypeGuard`" by @AlexWaygood on 2025-10-19 23:01_

---

_Label `ty` added by @AlexWaygood on 2025-10-19 23:02_

---

_Comment by @github-actions[bot] on 2025-10-19 23:02_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-30 01:44:45.375614067 +0000
+++ new-output.txt	2025-12-30 01:44:47.534622953 +0000
@@ -756,25 +756,15 @@
 namedtuples_usage.py:43:5: error[not-subscriptable] Cannot delete subscript on object of type `Point` with no `__delitem__` method
 namedtuples_usage.py:52:1: error[invalid-assignment] Too many values to unpack: Expected 2
 namedtuples_usage.py:53:1: error[invalid-assignment] Not enough values to unpack: Expected 4
-narrowing_typeguard.py:17:9: error[type-assertion-failure] Type `tuple[str, str]` does not match asserted type `tuple[str, ...]`
-narrowing_typeguard.py:32:9: error[type-assertion-failure] Type `set[int]` does not match asserted type `set[object]`
-narrowing_typeguard.py:69:9: error[type-assertion-failure] Type `int` does not match asserted type `object`
-narrowing_typeguard.py:73:9: error[type-assertion-failure] Type `int` does not match asserted type `object`
-narrowing_typeguard.py:77:9: error[type-assertion-failure] Type `int` does not match asserted type `object`
-narrowing_typeguard.py:81:9: error[type-assertion-failure] Type `int` does not match asserted type `object`
-narrowing_typeguard.py:85:9: error[type-assertion-failure] Type `int` does not match asserted type `object`
-narrowing_typeguard.py:89:9: error[type-assertion-failure] Type `B` does not match asserted type `object`
-narrowing_typeguard.py:93:9: error[type-assertion-failure] Type `B` does not match asserted type `object`
+narrowing_typeguard.py:128:20: error[invalid-argument-type] Argument to function `takes_callable_str` is incorrect: Expected `(object, /) -> str`, found `def simple_typeguard(val: object) -> TypeGuard[int]`
+narrowing_typeguard.py:148:26: error[invalid-argument-type] Argument to function `takes_callable_str_proto` is incorrect: Expected `CallableStrProto`, found `def simple_typeguard(val: object) -> TypeGuard[int]`
 narrowing_typeis.py:21:9: error[type-assertion-failure] Type `tuple[str, ...]` does not match asserted type `tuple[str, ...] & ~tuple[str, str]`
 narrowing_typeis.py:35:18: error[invalid-assignment] Object of type `object` is not assignable to `int`
 narrowing_typeis.py:38:9: error[type-assertion-failure] Type `int` does not match asserted type `int & ~Awaitable[object]`
-narrowing_typeis.py:72:9: error[type-assertion-failure] Type `int` does not match asserted type `object`
-narrowing_typeis.py:76:9: error[type-assertion-failure] Type `int` does not match asserted type `object`
-narrowing_typeis.py:80:9: error[type-assertion-failure] Type `int` does not match asserted type `object`
-narrowing_typeis.py:92:9: error[type-assertion-failure] Type `B` does not match asserted type `object`
-narrowing_typeis.py:96:9: error[type-assertion-failure] Type `B` does not match asserted type `object`
 narrowing_typeis.py:132:20: error[invalid-argument-type] Argument to function `takes_callable_str` is incorrect: Expected `(object, /) -> str`, found `def simple_typeguard(val: object) -> TypeIs[int]`
 narrowing_typeis.py:152:26: error[invalid-argument-type] Argument to function `takes_callable_str_proto` is incorrect: Expected `CallableStrProto`, found `def simple_typeguard(val: object) -> TypeIs[int]`
+narrowing_typeis.py:169:17: error[invalid-argument-type] Argument to function `takes_typeguard` is incorrect: Expected `(object, /) -> TypeGuard[int]`, found `def is_int_typeis(val: object) -> TypeIs[int]`
+narrowing_typeis.py:170:14: error[invalid-argument-type] Argument to function `takes_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def is_int_typeguard(val: object) -> TypeGuard[int]`
 narrowing_typeis.py:191:18: error[invalid-argument-type] Argument to function `takes_int_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def bool_typeis(val: object) -> TypeIs[bool]`
 overloads_basic.py:39:1: error[invalid-argument-type] Method `__getitem__` of type `Overload[(__i: int, /) -> int, (__s: slice[Any, Any, Any], /) -> bytes]` cannot be called with key of type `Literal[""]` on object of type `Bytes`
 overloads_definitions.py:20:5: error[invalid-overload] Overloaded function `func1` requires at least two overloads
@@ -1030,4 +1020,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1032 diagnostics
+Found 1022 diagnostics

```

</details>




---

_Comment by @github-actions[bot] on 2025-10-19 23:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- typing-examples/mypy.py:418:5: error[unresolved-attribute] Class `object` has no attribute `__attrs_attrs__`
- Found 617 diagnostics
+ Found 616 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_internal/index/package_finder.py:997:21: warning[possibly-missing-attribute] Attribute `version` may be missing on object of type `InstallationCandidate | None`
- src/pip/_internal/index/package_finder.py:1010:17: warning[possibly-missing-attribute] Attribute `version` may be missing on object of type `InstallationCandidate | None`
- Found 611 diagnostics
+ Found 609 diagnostics

black (https://github.com/psf/black)
- src/black/__init__.py:1377:23: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Node | Leaf`
- src/black/handle_ipynb_magics.py:393:17: error[unresolved-attribute] Object of type `expr` has no attribute `attr`
- src/black/handle_ipynb_magics.py:447:16: error[unresolved-attribute] Object of type `expr` has no attribute `attr`
- src/black/handle_ipynb_magics.py:449:18: error[unresolved-attribute] Object of type `expr` has no attribute `attr`
- src/black/handle_ipynb_magics.py:455:49: error[unresolved-attribute] Object of type `expr` has no attribute `attr`
- src/black/handle_ipynb_magics.py:484:16: error[unresolved-attribute] Object of type `expr` has no attribute `attr`
- src/black/handle_ipynb_magics.py:493:18: error[unresolved-attribute] Object of type `expr` has no attribute `attr`
- src/black/handle_ipynb_magics.py:495:18: error[unresolved-attribute] Object of type `expr` has no attribute `attr`
- src/black/linegen.py:228:41: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Node | Leaf`
- src/black/linegen.py:1612:9: error[invalid-assignment] Object of type `Literal[""]` is not assignable to attribute `value` on type `Leaf | Node`
- src/black/linegen.py:1821:25: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Node | Leaf`
- src/black/linegen.py:1859:13: error[invalid-assignment] Object of type `Literal[""]` is not assignable to attribute `value` on type `Node | Leaf`
- src/black/linegen.py:1860:13: error[invalid-assignment] Object of type `Literal[""]` is not assignable to attribute `value` on type `Node | Leaf`
- src/black/nodes.py:746:32: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Leaf | Node`
- Found 68 diagnostics
+ Found 54 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/utils/decorators.py:109:26: error[invalid-await] `_T@_warn_spider_arg` is not awaitable
- scrapy/utils/decorators.py:120:31: error[not-iterable] Object of type `_T@_warn_spider_arg` is not async-iterable
- Found 1791 diagnostics
+ Found 1789 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:216:59: error[invalid-assignment] Object of type `staticmethod[(value: Any), Unknown]` is not assignable to `(Any, /) -> Unknown`
+ src/graphql/execution/execute.py:216:59: error[invalid-assignment] Object of type `staticmethod[(value: Any), Unknown | TypeGuard[Awaitable[Unknown]]]` is not assignable to `(Any, /) -> Unknown | TypeGuard[Awaitable[Unknown]]`

starlette (https://github.com/encode/starlette)
- starlette/routing.py:624:17: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[Unknown]`, found `((Any, /) -> AbstractAsyncContextManager[None, bool | None]) | ((Any, /) -> AbstractAsyncContextManager[Mapping[str, Any], bool | None])`
- starlette/routing.py:632:17: error[invalid-argument-type] Argument to function `_wrap_gen_lifespan_context` is incorrect: Expected `(Any, /) -> Generator[Any, Any, Any]`, found `((Any, /) -> AbstractAsyncContextManager[None, bool | None]) | ((Any, /) -> AbstractAsyncContextManager[Mapping[str, Any], bool | None])`
- Found 218 diagnostics
+ Found 216 diagnostics

flake8-pyi (https://github.com/PyCQA/flake8-pyi)
- flake8_pyi/visitor.py:1732:21: error[invalid-argument-type] Argument to function `_analyse_exit_method_arg` is incorrect: Expected `BinOp`, found `expr`
- flake8_pyi/visitor.py:1750:21: error[invalid-argument-type] Argument to function `_analyse_exit_method_arg` is incorrect: Expected `BinOp`, found `expr`
- flake8_pyi/visitor.py:1763:21: error[invalid-argument-type] Argument to function `_analyse_exit_method_arg` is incorrect: Expected `BinOp`, found `expr`
- Found 5 diagnostics
+ Found 2 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- github/AuthenticatedUser.py:757:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/AuthenticatedUser.py:784:73: error[not-iterable] Object of type `list[Label] | _NotSetType` may not be iterable
- github/AuthenticatedUser.py:790:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/AuthenticatedUser.py:817:73: error[not-iterable] Object of type `list[Label] | _NotSetType` may not be iterable
- github/AuthenticatedUser.py:823:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/AuthenticatedUser.py:873:31: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/AuthenticatedUser.py:875:32: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Branch.py:211:106: error[not-iterable] Object of type `list[str | tuple[str, int]] | _NotSetType` may not be iterable
- github/Branch.py:222:34: error[not-iterable] Object of type `list[str | tuple[str, int]] | _NotSetType` may not be iterable
- github/Branch.py:225:74: error[not-iterable] Object of type `list[str] | _NotSetType` may not be iterable
- github/Branch.py:378:106: error[not-iterable] Object of type `list[str | tuple[str, int]] | _NotSetType` may not be iterable
- github/Branch.py:384:30: error[not-iterable] Object of type `list[str | tuple[str, int]] | _NotSetType` may not be iterable
- github/Branch.py:387:70: error[not-iterable] Object of type `list[str] | _NotSetType` may not be iterable
- github/BranchProtection.py:169:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `str | _NotSetType`
- github/CheckRun.py:253:45: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/CheckRun.py:255:47: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Gist.py:259:107: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `dict[str, InputFileContent | None] | _NotSetType`
- github/Issue.py:463:32: error[not-iterable] Object of type `list[NamedUser | str] | _NotSetType` may not be iterable
+ github/Issue.py:459:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/MainClass.py:553:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `_NotSetType | datetime`
- github/MainClass.py:915:42: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `_NotSetType | (Repository & Unknown)`
- github/Milestone.py:202:41: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `_NotSetType | date`
- github/NamedUser.py:460:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `_NotSetType | datetime`
- github/Organization.py:919:88: error[not-iterable] Object of type `list[Repository] | _NotSetType` may not be iterable
- github/Organization.py:921:93: error[not-iterable] Object of type `list[Repository] | _NotSetType` may not be iterable
- github/Organization.py:1013:79: error[not-iterable] Object of type `list[Repository] | _NotSetType` may not be iterable
- github/Organization.py:1048:85: error[not-iterable] Object of type `list[Repository] | _NotSetType` may not be iterable
- github/Organization.py:1237:73: error[not-iterable] Object of type `list[Label] | _NotSetType` may not be iterable
- github/Organization.py:1239:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Organization.py:1438:40: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `NamedUser | _NotSetType`
- github/Organization.py:1440:53: error[not-iterable] Object of type `list[Team] | _NotSetType` may not be iterable
- github/PullRequest.py:561:30: warning[possibly-missing-attribute] Attribute `sha` may be missing on object of type `Commit | _NotSetType`
- github/PullRequest.py:686:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Repository.py:1716:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ github/Repository.py:1723:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ github/Repository.py:3213:103: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/Repository.py:1450:41: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
- github/Repository.py:1452:44: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
- github/Repository.py:1631:41: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
- github/Repository.py:1648:44: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `GitTree | _NotSetType`
- github/Repository.py:1719:44: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `Milestone | _NotSetType`
- github/Repository.py:1802:45: error[unresolved-attribute] Object of type `_NotSetType & ~date` has no attribute `isoformat`
- github/Repository.py:1860:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `Issue | _NotSetType`
- github/Repository.py:2287:19: error[no-matching-overload] No overload of function `quote` matches arguments
- github/Repository.py:2432:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Repository.py:2434:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Repository.py:2770:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
- github/Repository.py:2772:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
- github/Repository.py:2855:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
- github/Repository.py:2857:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
- github/Repository.py:2906:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
- github/Repository.py:2908:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
- github/Repository.py:3210:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `NamedUser | _NotSetType`
- github/Repository.py:3220:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Repository.py:3225:45: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `(NamedUser & ~str) | (_NotSetType & ~str)`
- github/Repository.py:3250:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Repository.py:3493:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Repository.py:3897:31: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Repository.py:3899:32: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Repository.py:4217:45: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Repository.py:4219:47: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- Found 354 diagnostics
+ Found 301 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `InvalidSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `AnySchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `NoneSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `BoolSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `IntSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `FloatSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `DecimalSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `StringSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `BytesSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `DateSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `TimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `DatetimeSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `TimedeltaSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `LiteralSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `MissingSentinelSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `EnumSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `IsInstanceSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `IsSubclassSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `CallableSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `ListSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `TupleSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `SetSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `FrozenSetSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `GeneratorSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `DictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `UnionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `TaggedUnionSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `ChainSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `LaxOrStrictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `JsonOrPythonSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `TypedDictSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `ModelFieldsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `DataclassArgsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsV3Schema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `CallSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `UrlSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `MultiHostUrlSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `DefinitionReferenceSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `UuidSchema`: Unknown key "schema"
- pydantic/_internal/_discriminated_union.py:304:71: error[invalid-key] Unknown key "schema" for TypedDict `ComplexSchema`: Unknown key "schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `ModelSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `AnySchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `NoneSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `BoolSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `InvalidSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `FloatSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `DecimalSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `StringSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `BytesSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `DateSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `TimeSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `DatetimeSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `TimedeltaSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `LiteralSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `MissingSentinelSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `EnumSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `IsInstanceSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `IsSubclassSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `CallableSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `DictSchema` - did you mean "keys_schema"?
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `AfterValidatorFunctionSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `BeforeValidatorFunctionSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `WrapValidatorFunctionSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `WithDefaultSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `UnionSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `TaggedUnionSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `ChainSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `LaxOrStrictSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `JsonOrPythonSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `TypedDictSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `ModelFieldsSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `DataclassArgsSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `DataclassSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `ArgumentsSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `ArgumentsV3Schema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `CallSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `CustomErrorSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `JsonSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `UrlSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `MultiHostUrlSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `DefinitionsSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `DefinitionReferenceSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `UuidSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `ComplexSchema`: Unknown key "items_schema"
- pydantic/_internal/_generate_schema.py:270:16: error[invalid-key] Unknown key "items_schema" for TypedDict `IntSchema`: Unknown key "items_schema"
- pydantic/json_schema.py:563:49: error[invalid-argument-type] Argument to function `populate_defs` is incorrect: Expected `InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements`, found `InvalidSchema | AnySchema | NoneSchema | ... omitted 53 union elements`
- pydantic/json_schema.py:585:41: error[invalid-argument-type] Argument to function `populate_defs` is incorrect: Expected `InvalidSchema | AnySchema | NoneSchema | ... omitted 49 union elements`, found `InvalidSchema | AnySchema | NoneSchema | ... omitted 53 union elements`
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `UrlSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `BytesSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `DateSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `TimeSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `DatetimeSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `TimedeltaSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `LiteralSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `MissingSentinelSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `EnumSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `IsInstanceSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `IsSubclassSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `CallableSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `ListSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `TupleSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `SetSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `FrozenSetSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `GeneratorSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `DictSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `UnionSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `StringSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `ChainSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `LaxOrStrictSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `JsonOrPythonSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `TypedDictSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `ModelFieldsSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `DataclassArgsSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsV3Schema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `CallSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `InvalidSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `MultiHostUrlSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `DefinitionReferenceSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `UuidSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `ComplexSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `DecimalSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `AnySchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `NoneSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `BoolSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `IntSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `FloatSchema`: Unknown key "schema"
- pydantic/json_schema.py:2178:39: error[invalid-key] Unknown key "schema" for TypedDict `TaggedUnionSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `MultiHostUrlSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `StringSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `BytesSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `DateSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `TimeSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `DatetimeSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `TimedeltaSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `LiteralSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `MissingSentinelSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `EnumSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `IsInstanceSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `IsSubclassSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `CallableSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `ListSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `TupleSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `SetSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `FrozenSetSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `GeneratorSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `DictSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `DecimalSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `TaggedUnionSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `ChainSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `LaxOrStrictSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `JsonOrPythonSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `TypedDictSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `ModelFieldsSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `DataclassArgsSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsV3Schema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `CallSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `UrlSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `InvalidSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `DefinitionReferenceSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `UuidSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `ComplexSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `ComputedField`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `AnySchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `NoneSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `BoolSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `IntSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `FloatSchema`: Unknown key "schema"
- pydantic/json_schema.py:2187:62: error[invalid-key] Unknown key "schema" for TypedDict `UnionSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `DataclassArgsSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `StringSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `BytesSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `DateSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `TimeSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `DatetimeSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `TimedeltaSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `LiteralSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `MissingSentinelSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `EnumSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `IsInstanceSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `IsSubclassSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `CallableSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `ListSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `TupleSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `SetSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `FrozenSetSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `GeneratorSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `DictSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `PlainValidatorFunctionSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `DecimalSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `TaggedUnionSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `ChainSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `LaxOrStrictSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `JsonOrPythonSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `TypedDictSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `ModelFieldsSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `InvalidSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `ArgumentsV3Schema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `CallSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `UrlSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `MultiHostUrlSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `DefinitionReferenceSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `UuidSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `ComplexSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `AnySchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `NoneSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `BoolSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `IntSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `FloatSchema`: Unknown key "schema"
- pydantic/json_schema.py:2859:57: error[invalid-key] Unknown key "schema" for TypedDict `UnionSchema`: Unknown key "schema"
- Found 3377 diagnostics
+ Found 3160 diagnostics

mypy (https://github.com/python/mypy)
- mypy/checker.py:2356:53: error[invalid-argument-type] Argument to bound method `check_setter_type_override` is incorrect: Expected `OverloadedFuncDef`, found `FuncDef | OverloadedFuncDef | Decorator`
- mypy/checker.py:5325:50: error[unresolved-attribute] Object of type `ProperType & ~TupleType` has no attribute `args`
- mypy/checker.py:8683:17: error[unresolved-attribute] Object of type `ProperType & ~TupleType` has no attribute `args`
- mypy/checker.py:9273:18: error[unresolved-attribute] Object of type `SymbolNode` has no attribute `items`
- mypy/constraints.py:351:38: error[invalid-argument-type] Argument to function `_unwrap_type_type` is incorrect: Expected `TypeType | UnionType`, found `ProperType`
- mypy/constraints.py:352:36: error[invalid-argument-type] Argument to function `_unwrap_type_type` is incorrect: Expected `TypeType | UnionType`, found `ProperType`
- mypy/plugins/enums.py:98:62: error[unresolved-attribute] Object of type `ProperType` has no attribute `args`
- mypy/plugins/enums.py:99:20: error[unresolved-attribute] Object of type `ProperType` has no attribute `args`
- mypy/semanal.py:4058:22: error[unresolved-attribute] Object of type `Expression` has no attribute `args`
- mypy/semanal.py:4060:86: error[invalid-argument-type] Argument to bound method `analyze_type_alias_type_params` is incorrect: Expected `CallExpr`, found `Expression`
- mypy/semanal_enum.py:200:26: error[unresolved-attribute] Object of type `Expression` has no attribute `value`
- mypy/typeshed/stdlib/inspect.pyi:192:43: error[not-subscriptable] Cannot subscript non-generic type
- mypy/typeshed/stdlib/inspect.pyi:200:54: error[not-subscriptable] Cannot subscript non-generic type
- mypyc/codegen/literals.py:63:41: error[invalid-argument-type] Argument to bound method `record_literal` is incorrect: Expected `str | bytes | int | ... omitted 5 union elements`, found `object`
- mypyc/codegen/literals.py:70:41: error[invalid-argument-type] Argument to bound method `record_literal` is incorrect: Expected `str | bytes | int | ... omitted 5 union elements`, found `object`
- mypyc/codegen/literals.py:169:44: error[invalid-argument-type] Argument to bound method `literal_index` is incorrect: Expected `str | bytes | int | ... omitted 5 union elements`, found `object`
- mypyc/irbuild/expression.py:869:16: error[unresolved-attribute] Object of type `RType` has no attribute `is_signed`
- mypyc/irbuild/expression.py:875:16: error[unresolved-attribute] Object of type `RType` has no attribute `is_signed`
- mypyc/irbuild/expression.py:887:12: error[unresolved-attribute] Object of type `RType` has no attribute `is_signed`
- mypyc/irbuild/ll_builder.py:389:47: error[invalid-argument-type] Argument to function `check_native_int_range` is incorrect: Expected `RPrimitive`, found `RType`
- mypyc/irbuild/ll_builder.py:557:76: error[unresolved-attribute] Object of type `RType` has no attribute `is_signed`
- mypyc/irbuild/ll_builder.py:1437:52: error[invalid-argument-type] Argument to bound method `fixed_width_int_op` is incorrect: Expected `RPrimitive`, found `RType`
- mypyc/irbuild/ll_builder.py:1440:25: error[invalid-argument-type] Argument to bound method `fixed_width_int_op` is incorrect: Expected `RPrimitive`, found `RType`
- mypyc/irbuild/ll_builder.py:1461:25: error[invalid-argument-type] Argument to bound method `fixed_width_int_op` is incorrect: Expected `RPrimitive`, found `RType`
- mypyc/irbuild/ll_builder.py:1464:52: error[invalid-argument-type] Argument to bound method `fixed_width_int_op` is incorrect: Expected `RPrimitive`, found `RType`
- mypyc/irbuild/ll_builder.py:1467:52: error[invalid-argument-type] Argument to bound method `fixed_width_int_op` is incorrect: Expected `RPrimitive`, found `RType`
- mypyc/irbuild/ll_builder.py:1565:43: error[unresolved-attribute] Object of type `Value` has no attribute `value`
- mypyc/irbuild/ll_builder.py:1565:56: error[unresolved-attribute] Object of type `Value` has no attribute `value`
- mypyc/irbuild/ll_builder.py:1805:16: error[unresolved-attribute] Object of type `RType` has no attribute `is_signed`
- mypyc/irbuild/ll_builder.py:1810:31: error[unresolved-attribute] Object of type `RType` has no attribute `size`
- Found 1783 diagnostics
+ Found 1753 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/exchange/exchange.py:2617:23: warning[possibly-missing-attribute] Attribute `ohlcvs` may be missing on object of type `ExchangeWS | None`
- freqtrade/exchange/exchange.py:2620:17: warning[possibly-missing-attribute] Attribute `klines_last_refresh` may be missing on object of type `ExchangeWS | None`
- freqtrade/exchange/exchange.py:2636:24: warning[possibly-missing-attribute] Attribute `get_ohlcv` may be missing on object of type `ExchangeWS | None`
- freqtrade/exchange/exchange.py:2667:17: warning[possibly-missing-attribute] Attribute `schedule_ohlcv` may be missing on object of type `ExchangeWS | None`
- Found 687 diagnostics
+ Found 683 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/_internal/utils.py:202:37: error[unresolved-attribute] Object of type `object` has no attribute `__wrapped__`
- src/antidote/core/_inject.py:348:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Any] | None`, found `object`
- src/antidote/core/_raw/__init__.py:104:16: error[unresolved-attribute] Object of type `(...) -> Any` has no attribute `__antidote_hardwired__`
- src/antidote/core/_raw/__init__.py:105:13: error[unresolved-attribute] Unresolved attribute `__antidote_maybe_app_catalog_onion__` on type `(...) -> Any`.
- src/antidote/core/_raw/__init__.py:107:16: error[unresolved-attribute] Object of type `(...) -> Any` has no attribute `__antidote_blueprint__`
- src/antidote/core/_raw/__init__.py:108:17: error[unresolved-attribute] Unresolved attribute `__antidote_blueprint__` on type `(...) -> Any`.
- src/antidote/core/_raw/__init__.py:109:21: error[unresolved-attribute] Object of type `(...) -> Any` has no attribute `__antidote_blueprint__`
- src/antidote/lib/lazy_ext/_provider.py:148:16: error[unresolved-attribute] Object of type `object` has no attribute `__antidote_debug__`
- src/antidote/lib/lazy_ext/_provider.py:152:13: error[unresolved-attribute] Object of type `object` has no attribute `__antidote_unsafe_provide__`
- Found 256 diagnostics
+ Found 247 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_queries.py:146:20: error[invalid-return-type] Return type does not match returned value: expected `Sequence[Any]`, found `Sequence[Any] | Mapping[str, Any]`
- psycopg/psycopg/_queries.py:473:50: error[invalid-argument-type] Argument to bound method `du

... (truncated 513 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     memo fields = ~403MB
+     memo fields = ~424MB


```

</details>




---

_Comment by @ericmarkmartin on 2025-10-19 23:32_

Actually I hadn't seen astral-sh/ty#690 before, but maybe this change isn't as terrible as I initially thought if it helps address that?

---

_Comment by @carljm on 2025-11-11 23:34_

Hi! Sorry this fell off my radar; it wasn't in my GH notifications because it was still in draft.

This looks pretty good to me, and TypeGuard support would be great to have; if you can get it rebased and the conformance suite changes look right, I'd happily merge it. If the constraint DNF stuff can help us resolve https://github.com/astral-sh/ty/issues/690 that's a bonus, although I kind of think we may need to eventually switch to an "every new narrowing is tracked as a new definition" approach instead, in order to also fix https://github.com/astral-sh/ty/issues/685

---

_Marked ready for review by @ericmarkmartin on 2025-11-12 23:26_

---

_Review requested from @carljm by @ericmarkmartin on 2025-11-12 23:26_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-11-12 23:26_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-11-12 23:26_

---

_Review requested from @dcreager by @ericmarkmartin on 2025-11-12 23:26_

---

_Review requested from @MichaReiser by @ericmarkmartin on 2025-11-12 23:26_

---

_Comment by @ericmarkmartin on 2025-11-12 23:29_

> This looks pretty good to me, and TypeGuard support would be great to have; if you can get it rebased and the conformance suite changes look right, I'd happily merge it

Rebased---fortunately not to bad conflict-wise and tests seem to pass. Not sure how to evaluate the conformance suite side but 🤞. 

> If the constraint DNF stuff can help us resolve [astral-sh/ty#690](https://github.com/astral-sh/ty/issues/690) that's a bonus

I'm not super confident about this, I'm mostly just going off of
> we just track an implicit AND of multiple narrowing constraints per definition. Since we have no way to represent an OR of narrowing constraints

from that issue


---

_Comment by @codspeed-hq[bot] on 2025-11-12 23:38_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ericmarkmartin%3Atypeguard?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #20974 will **degrade performance by 4.04%**

<sub>Comparing <code>ericmarkmartin:typeguard</code> (3071f8f) with <code>main</code> (9dadf27)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ericmarkmartin%3Atypeguard?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` sympy ``](https://codspeed.io/astral-sh/ruff/branches/ericmarkmartin%3Atypeguard?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asympy&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 50.8 s | 52.9 s | -4.04% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ericmarkmartin%3Atypeguard?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @carljm on 2025-11-14 23:24_

So regarding the conformance suite, the way to evaluate that is to open up the corresponding conformance suite test file (e.g. https://github.com/python/typing/blob/main/conformance/tests/narrowing_typeguard.py is the main relevant one here). Lines with an `# E` comment are supposed to emit a diagnostic, lines without are not supposed to.

Overall this PR is clearly an improvement (every change is in the right direction), but the conformance suite does reveal a gap in the implementation that we should probably fix, when a `TypeGuard` is an instance or class method. I would guess maybe we fail to skip the `self` / `cls` argument when deciding what is the "first argument" to which narrowing applies? See lines 69, 73, 77, 89, 93, where we still wrongly emit a diagnostic with this PR.

(When fixing this, we should also add corresponding mdtests to our own test suite.)

---

_Comment by @carljm on 2025-11-14 23:32_

It looks like the same is true for our existing implementation of `TypeIs`, I just filed https://github.com/astral-sh/ty/issues/1569

---

_Label `ecosystem-analyzer` added by @carljm on 2025-11-14 23:41_

---

_Comment by @carljm on 2025-11-15 00:32_

The ecosystem report looks generally fantastic! One thing I notice is that in cases like https://github.com/pydata/xarray/blob/main/xarray/namedarray/utils.py#L107 where the returned `TypeGuard` is a gradual type, this PR takes the top materialization of it, like we do for `TypeIs`. This is important for `TypeIs`, in order to get intersections to simplify and give the expected narrowing. I'm not sure we should do the same for `TypeGuard`. Because of the "replaces existing type" semantics, intersection simplification is less important for `TypeGuard`. And taking the top materialization causes us to emit diagnostics (which the author would likely consider false positives) in the xarray case, since we treat it as narrowing to `Mapping[object, object]` rather than `Mapping[Any, Any]`.

So I think we should remove the top materialization behavior from this PR and see how that impacts the ecosystem report.

---

_Comment by @carljm on 2025-11-15 00:34_

(I'm not sure why the ecosystem analyzer failed to post its comment here, but the report can be downloaded at https://github.com/astral-sh/ruff/actions/runs/19380669883/artifacts/4574376479 )

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:290 on 2025-11-15 00:36_

Shouldn't this be
```suggestion
/// `Conjunction { constraint: A & C, typeguard: Some(D) }` because the type guard
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:287 on 2025-11-15 00:37_

The type here is not necessarily an "intersection type", but it is a "type we will intersect with our previous knowledge of the type of the variable"

```suggestion
    /// The intersected constraints (represented as a type to intersect with)
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:290 on 2025-11-15 00:38_

Why would a conjunction be a union, not an intersection? And why would we need either one, when a typeguard clobbers previous information (as mentioned in the comment above)?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:328 on 2025-11-15 00:41_

Building intersections isn't free. I think we should probably make `constraint` an `Option` (rather than defaulting it to `object`) as well, so we can build this intersection only if we actually need to.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:342 on 2025-11-15 00:42_

I don't think this is quite right, as discussed below; we need to say something more like
```suggestion
///   => evaluates to `(P & A) | B` where `P` is our previously-known type
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:345 on 2025-11-15 00:43_

```suggestion
    /// Disjunction of conjunctions (DNF)
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:352 on 2025-11-15 00:44_

I sort of suspect that 4 is too large here, and either 1 or 2 will be better? We have to clone these quite a bit.

Did you do any experimentation to settle on 4? Can you try 1 and 2 and see if it makes any noticeable difference on benchmarks?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:380 on 2025-11-15 00:54_

I don't think this is right? As mentioned above, given a regular constraint of `A` OR a typeguard constraint of `B`, and a previously-known type of `P`, the answer we should narrow to is `(P & A) | B`, but the way this is currently implemented will give `P & (A | B)` instead. In the typeguard branch of the union we need to ignore the prior type.

Or, translating the same thing to testable Python code:

```py
from typing import TypeGuard, reveal_type

class P:
    pass

class A:
    pass

class B:
    pass

def is_b(val: object) -> TypeGuard[B]:
    return isinstance(val, B)

def _(x: P):
    if isinstance(x, A) or is_b(x):
        reveal_type(x)  # currently reveals `(P & A) | (P & B)`, should reveal `(P & A) | B`
```

I think this will require passing the `NarrowingConstraint` all the way out to the type-inference code (i.e. `InternalConstraints` should replace `NarrowingConstraints`), because there is no way to correctly implement this method without also taking in the prior type.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:429 on 2025-11-15 01:00_

I don't think this method can exist in a correct implementation, there is no correct way to do this translation without access to the prior type for each place.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/unsupported_special_forms.md`:24 on 2025-11-15 01:04_

I think we can just remove this, it's no longer an "unsupported special form"

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:23 on 2025-11-15 01:06_

I think you mean 
```suggestion
    # means `complex | int | float` semantically so `Intersection[complex,
```

But honestly I would just use a different type above and skip all this complex/int/float nonsense, it's not germane

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:215 on 2025-11-15 01:15_

Orthogonal to this PR, but I'm not sure this should even be a TODO. No other type checker draws this conclusion, and I think the odd semantics of `TypeGuard` make it unclear whether it's even valid to do so.
```suggestion
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:220 on 2025-11-15 01:18_

Also orthogonal, but this is wrong IMO, if anything we could reveal `tuple[Foo & Bar, Bar] | tuple[Bar, Foo]`, but no other type checker does this either. It seems of questionable utility.
```suggestion
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:226 on 2025-11-15 01:20_

Shouldn't this be just `Foo` per the semantics of `TypeGuard`?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/type_guards.md`:350 on 2025-11-15 01:21_

Shouldn't this be just `Foo`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:1542 on 2025-11-15 01:24_

As mentioned in another comment, I think for `TypeGuard`, given its "clobber" semantics, we maybe shouldn't do this? Or at least I'm curious what the impact on the ecosystem report would be. We do see some false positives in the ecosystem from doing this.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:9019 on 2025-11-15 01:25_

The bug with method guards would be fixed in here, I think.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:530 on 2025-11-15 01:31_

I wonder why we don't just do nothing here? We don't want to constrain, and there's already no constraint in `into`, so let's just... leave it that way. Seems to pass tests.
```suggestion
                // Place only appears in `from`, not in `into`. No constraint needed.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:516 on 2025-11-15 01:34_

This seems better, and passes tests, not sure why we didn't do it previously:

```suggestion
    // For places that appear in `into` but not in `from`, do not constrain
    into.constraints
        .retain(|key, _| from.constraints.contains_key(key));

```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:12445 on 2025-11-15 01:36_

Let's remove this or flesh it out

---

_@carljm reviewed on 2025-11-15 01:46_

Thank you for working on this, this will be a great feature to finally have!

Noticed a few more things on a more thorough once over, and via the conformance suite and ecosystem report.

---

_@ericmarkmartin reviewed on 2025-11-17 23:58_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types/narrow.rs`:290 on 2025-11-17 23:58_

I didn't think so...

```py
class A: ...
class B: ...
class C: ...
class D: ...

def is_a(x: object) -> TypeIs[A]: ...
def guard_b(x: object) -> TypeGuard[B]: ...
def is_c(x: object) -> TypeIs[C]: ...
def guard_d(x: object) -> TypeGuard[D]: ...
```

Now I think `Conjunction { constraint: A, typeguard: Some(B) }` is produced by `guard_b(x) and is_a(x)` (if it were the other way around, it'd just be `Conjunction { constraint: None, tyepguard: Some(B) }` (assuming for a second we already have optionality of `constraint`)). 

Likewise `Conjunction { constraint: C, typeguard: Some(D)})` is produced by `guard_d(x) and is_c(x)`. If you put those together, i.e. `guard_b(x) and is_a(x) and guard_d(x) and is_c(x)`, I think the `guard_d` should wipe out the `guard_b` and `is_a` and leave you with just the `guard_d` and the `is_c`.

---

_@ericmarkmartin reviewed on 2025-11-18 00:00_

---

_Review comment by @ericmarkmartin on `crates/ty_python_semantic/src/types/narrow.rs`:290 on 2025-11-18 00:00_

Yeah I think this was just really poor wording on my end

---

_@carljm reviewed on 2025-11-18 00:16_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:290 on 2025-11-18 00:16_

Oh yeah, I think you're right! Sorry, I think I was mixing up AND and OR somewhere in there.

---

_Comment by @carljm on 2025-12-05 01:47_

Hi @ericmarkmartin -- thanks again for this contribution! Do you have any timeframe on getting back to this PR? Would you like someone else to pick it up and push it forward? Would love to get this feature in!

---

_Comment by @ericmarkmartin on 2025-12-16 22:24_

Hey @carljm,

Sorry this fell off my radar a bit. I'd be happy to get back into this if it's still wanted!

---

_Comment by @carljm on 2025-12-16 22:26_

Definitely still wanted, if you're up for picking it back up I'll try to be speedy on reviews!

---

_Closed by @MichaReiser on 2025-12-23 08:52_

---

_Reopened by @MichaReiser on 2025-12-23 08:52_

---

_Comment by @ericmarkmartin on 2025-12-27 04:46_

This PR should also resolve https://github.com/astral-sh/ty/issues/1569 now, updated description as such

---

_Comment by @astral-sh-bot[bot] on 2025-12-27 05:55_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-key` | 0 | 215 | 0 |
| `invalid-argument-type` | 40 | 68 | 12 |
| `possibly-missing-attribute` | 29 | 75 | 1 |
| `unsupported-operator` | 67 | 6 | 2 |
| `unresolved-attribute` | 1 | 66 | 0 |
| `type-assertion-failure` | 2 | 0 | 26 |
| `not-iterable` | 0 | 18 | 1 |
| `unused-ignore-comment` | 7 | 10 | 0 |
| `invalid-return-type` | 3 | 5 | 6 |
| `unknown-argument` | 0 | 14 | 0 |
| `not-subscriptable` | 4 | 9 | 0 |
| `invalid-assignment` | 1 | 3 | 7 |
| `no-matching-overload` | 6 | 4 | 0 |
| `call-non-callable` | 0 | 6 | 0 |
| `invalid-await` | 0 | 5 | 0 |
| `missing-argument` | 1 | 0 | 0 |
| `too-many-positional-arguments` | 1 | 0 | 0 |
| **Total** | **162** | **504** | **55** |


**[Full report with detailed diff](https://82422ca8.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://82422ca8.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @MichaReiser on 2025-12-29 09:22_

Thank you for your continuous work on this. Almost the entire team is out this week. It may take a few days before someone finds time to review your PR. Happy holidays.

---

_Comment by @carljm on 2025-12-29 23:20_

Conformance tests look great! I filed https://github.com/astral-sh/ty/issues/2267, which I think captures what remains (after this PR) for us to fully pass the `TypeIs` and `TypeGuard` conformance tests.

Ecosystem also looks great! Mostly it's removing false positives due to our new understanding of `TypeGuard`.

There are a lot of new diagnostics on scipy -- I spot-checked these and they look like true positives (at least given the specified behavior of `TypeGuard`.) Scipy has a lot of functions that take in an object of unknown type, and then do something like `if not isscalar(obj) or (obj < 1200)`, where `isscalar` is a `TypeGuard`, and the union it narrows to includes types that can't be compared `< 1200`. Erroring on this looks correct, and other type checkers also error in similar usages.

The new diagnostics on apprise are a bit weird, and I haven't fully tracked down where a `TypeGuard` is even involved. But the underlying issue there looks like https://github.com/astral-sh/ty/issues/1714, so I'm not concerned that it's surfacing a problem with this PR.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2844 on 2025-12-30 00:34_

```suggestion
            // `TypeIs[T]` and `TypeGuard[T]` are subtypes of `bool`.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4370 on 2025-12-30 00:34_

```suggestion
            Type::TypeGuard(type_guard) => type_guard.is_bound(db),
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8094 on 2025-12-30 00:37_

I don't think we need this TODO here, unless you're aware of some specific reason it's needed?

Unlike `TypeIs`, `TypeGuard` is covariant, so it should be correct (even for a "materialize" mapping) to just propagate the mapping to the return type. The TODO above is because `TypeIs` is invariant, so it's not correct to just propagate a materialization through it.

```suggestion
```

---

_@carljm reviewed on 2025-12-30 00:52_

Still looking at the core implementation in `narrow.rs`, but submitting a few other comments... looking really good so far!

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:310 on 2025-12-30 01:00_

Did you consider eliminating this struct, making `typeguard_disjuncts` below a `SmallVec` of `Type<'db>'` instead, and eagerly intersecting refinements into the type instead of doing it lazily? Locally that seems to simplify things a bit, and passes all tests. I'm pretty sure it's semantically equivalent, because there wasn't any place where we would "break apart" a `TypeGuardConstraint` and consider its refinement separately from its base typeguard type; we would always keep them together (possibly intersecting more things into `refinement`) and just end up intersecting them anyway.

I'll try pushing a commit that does this and see how it impacts performance and ecosystem.

---

_@carljm reviewed on 2025-12-30 01:19_

---

_@carljm reviewed on 2025-12-30 01:27_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:340 on 2025-12-30 01:27_

```suggestion
        // The thing we actually need to deal with is the RHS `regular_disjunct`.
        // It gets intersected with the LHS `regular_disjunct` to form the new
        // `regular_disjunct`, and intersected with each LHS `typeguard_disjunct`
        // to form new additional `typeguard_disjuncts`.
```

---

_Merged by @carljm on 2025-12-30 01:54_

---

_Closed by @carljm on 2025-12-30 01:54_

---

_@carljm reviewed on 2025-12-30 01:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/narrow.rs`:310 on 2025-12-30 01:56_

Went ahead and merged with this simplification. If you think it's wrong, let me know (ideally with a code example we now fail) and I'll be happy to make a follow-up PR to restore this.

---

_Comment by @carljm on 2025-12-30 01:57_

Merged -- thank you for this!!

---

_Branch deleted on 2026-01-05 02:10_

---
